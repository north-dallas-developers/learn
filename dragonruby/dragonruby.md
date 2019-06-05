# HEAD is now at ba8c0437 Render just a black screen to show the dimensions of the GTK.

```ruby
def tick args
  args.outputs.solids << [0, 0, 1280, 720]
end
```

# HEAD is now at bc0e201e Render a tiny square in the center of the screen (tell people to play around with colors and alpha).

```ruby
def tick args
  args.outputs.solids << [0, 0, 1280, 720]
  args.outputs.solids << [640, 360, 3, 3, 255, 255, 255]
end
```

# HEAD is now at bc0e201e Render a tiny square in the center of the screen (tell people to play around with colors and alpha).

```ruby
def tick args
  args.outputs.solids << [0, 0, 1280, 720]
  args.outputs.solids << [640, 360, 3, 3, 255, 255, 255]
end
```

# HEAD is now at b5f97062 Show the usage of app state, and have them figure out how to get the little pixel to wrap around. Show them how to reset the game too.

```ruby
def tick args
  args.state.x ||= 640
  args.state.y ||= 360
  args.outputs.solids << [0, 0, 1280, 720]
  args.outputs.solids << [args.state.x, args.state.y, 3, 3, 255, 255, 255]
  args.state.x += 1
  args.state.y += 1
end
```

# HEAD is now at 5dbf2ef9 The answer to wrapping.

```ruby
def tick args
  args.state.x ||= 640
  args.state.y ||= 360
  args.outputs.solids << [0, 0, 1280, 720]
  args.outputs.solids << [args.state.x, args.state.y, 3, 3, 255, 255, 255]
  args.state.x += 1
  args.state.y += 1
  args.state.x = 0 if args.state.x > 1280
  args.state.y = 0 if args.state.y > 720
end
```

# HEAD is now at 322fb6b0 Explain the concept of a game loop and pipeline. Ask them to figure out how to render 10 of these stars. But give them a crash course of arrays using the console.

```ruby
def defaults args
  args.state.x ||= 640
  args.state.y ||= 360
end

def render args
  args.outputs.solids << [0, 0, 1280, 720]
  args.outputs.solids << [args.state.x, args.state.y, 3, 3, 255, 255, 255]
end

def calc args
  args.state.x += 1
  args.state.y += 1
  args.state.x = 0 if args.state.x > 1280
  args.state.y = 0 if args.state.y > 720
end

def tick args
  defaults args
  render args
  calc args
end
```

# HEAD is now at 7ae98684 Solution, and then talk about destructuring. Tell them to add variable speed and color.

```ruby
def defaults args
  args.state.x ||= 640
  args.state.y ||= 360
  args.state.stars ||= 10.map do
    [1280 * rand, 720 * rand]
  end
end

def render args
  args.outputs.solids << [0, 0, 1280, 720]
  args.outputs.solids += args.state.stars.map do |x, y|
    [x, y, 3, 3, 255, 255, 255]
  end
end

def calc args
  args.state.stars = args.state.stars.map do |x, y|
    x += 1
    y += 1
    x = 0 if x > 1280
    y = 0 if y > 720
    [x, y]
  end
end

def tick args
  defaults args
  render args
  calc args
end
```

# HEAD is now at 7e4de6f8 Show them sprites. Rant about "Continuity of Design TM" and how Ruby embodies this. Tell them to add the solar system. Show them new_entity.

```ruby
def defaults args
  args.state.x ||= 640
  args.state.y ||= 360
  args.state.stars ||= 10.map do
    [1280 * rand, 720 * rand, 10 * rand + 1, 255 * rand, 255 * rand, 255 * rand]
  end
end

def render args
  args.outputs.solids << [0, 0, 1280, 720]
  args.outputs.sprites += args.state.stars.map do |x, y, _, r, g, b|
    [x, y, 10, 10, 'sprites/star.png', 0, 255, r, g, b]
  end
end

def calc args
  args.state.stars = args.state.stars.map do |x, y, speed, r, g, b|
    x += speed
    y += speed
    x = 0 if x > 1280
    y = 0 if y > 720
    [x, y, speed, r, g, b]
  end
end

def tick args
  defaults args
  render args
  calc args
end

def r
  $dragon.reset
end
```

# HEAD is now at 0846aa95 Planets added. Talk about the power of arrays. Tell them to get the planets rotating (show them some repl stuff, talk a little bit about trig).

```ruby
def defaults args
  args.state.x ||= 640
  args.state.y ||= 360
  args.state.stars ||= 10.map do
    [1280 * rand, 720 * rand, 10 * rand + 1, 255 * rand, 255 * rand, 255 * rand]
  end

  args.state.sun ||= args.state.new_entity(:sun) do |s|
    s.x = 640
    s.y = 360
    s.s = 100
    s.path = 'sprites/sun.png'
  end

  args.state.planets = [
    [:mercury,   55,  5],
    [:venus,    100, 10],
    [:earth,    120, 10],
    [:mars,     140,  8],
    [:jupiter,  200, 30],
    [:saturn,   250, 20],
    [:uranus,   300, 15],
    [:neptune,  320, 15],
    [:pluto,    380,  5],
  ].map do |name, distance, size|
    args.state.new_entity(name) do |p|
      p.path = "sprites/#{name}.png"
      p.x = args.state.sun.x + distance
      p.y = 360
      p.s = size
    end
  end
end

def to_sprite entity
  [entity.x - entity.s.half, entity.y - entity.s.half, entity.s, entity.s, entity.path]
end

def render args
  args.outputs.solids << [0, 0, 1280, 720]

  args.outputs.sprites += args.state.stars.map do |x, y, _, r, g, b|
    [x, y, 10, 10, 'sprites/star.png', 0, 255, r, g, b]
  end

  args.outputs.sprites << to_sprite(args.state.sun)
  args.outputs.sprites += args.state.planets.map { |p| to_sprite p }
end

def calc args
  args.state.stars = args.state.stars.map do |x, y, speed, r, g, b|
    x += speed
    y += speed
    x = 0 if x > 1280
    y = 0 if y > 720
    [x, y, speed, r, g, b]
  end
end

def tick args
  defaults args
  render args
  calc args
end

def r
  $dragon.reset
end
```

# HEAD is now at d4866902 Show them the solution show them the passage of time. Give them a crash course on inputs using the repl, and tell them to add a ship to scene that you can control. Tell them to add thruster particles coming out of the ship too.

```ruby
def defaults args
  args.state.x ||= 640
  args.state.y ||= 360
  args.state.stars ||= 10.map do
    [1280 * rand, 720 * rand, 10 * rand + 1, 255 * rand, 255 * rand, 255 * rand]
  end

  args.state.sun ||= args.state.new_entity(:sun) do |s|
    s.s = 100
    s.path = 'sprites/sun.png'
  end

  args.state.planets = [
    [:mercury,   55,  5,          88],
    [:venus,    100, 10,         225],
    [:earth,    120, 10,         365],
    [:mars,     140,  8,         687],
    [:jupiter,  380, 30, 365 *  11.8],
    [:saturn,   450, 20, 365 *  29.5],
    [:uranus,   500, 15, 365 *    84],
    [:neptune,  540, 15, 365 * 164.8],
    [:pluto,    580,  5, 365 * 247.8],
  ].map do |name, distance, size, year_in_days|
    args.state.new_entity(name) do |p|
      p.path = "sprites/#{name}.png"
      p.distance = distance
      p.s = size
      p.year_in_days = year_in_days
    end
  end
end

def to_sprite args, entity
  x = 0
  y = 0

  if entity.year_in_days
    day = args.state.tick_count
    day_in_year = day % entity.year_in_days
    percentage_of_year = day_in_year.fdiv(entity.year_in_days)
    angle = 365 * percentage_of_year
    x = angle.vector_x(entity.distance)
    y = angle.vector_y(entity.distance)
  end

  [640 + x - entity.s.half, 360 + y - entity.s.half, entity.s, entity.s, entity.path]
end

def render args
  args.outputs.solids << [0, 0, 1280, 720]

  args.outputs.sprites += args.state.stars.map do |x, y, _, r, g, b|
    [x, y, 10, 10, 'sprites/star.png', 0, 255, r, g, b]
  end

  args.outputs.sprites << to_sprite(args, args.state.sun)
  args.outputs.sprites += args.state.planets.map { |p| to_sprite args, p }
end

def calc args
  args.state.stars = args.state.stars.map do |x, y, speed, r, g, b|
    x += speed
    y += speed
    x = 0 if x > 1280
    y = 0 if y > 720
    [x, y, speed, r, g, b]
  end
end

def tick args
  defaults args
  render args
  calc args
end

def r
  $dragon.reset
end
```

# HEAD is now at e20efadd Solution, and music next.

```ruby
def defaults args
  args.state.x ||= 640
  args.state.y ||= 360
  args.state.stars ||= 10.map do
    [1280 * rand, 720 * rand, 10 * rand + 1, 255 * rand, 255 * rand, 255 * rand]
  end

  args.state.sun ||= args.state.new_entity(:sun) do |s|
    s.s = 100
    s.path = 'sprites/sun.png'
  end

  args.state.planets = [
    [:mercury,   55,  5,          88],
    [:venus,    100, 10,         225],
    [:earth,    120, 10,         365],
    [:mars,     140,  8,         687],
    [:jupiter,  380, 30, 365 *  11.8],
    [:saturn,   450, 20, 365 *  29.5],
    [:uranus,   500, 15, 365 *    84],
    [:neptune,  540, 15, 365 * 164.8],
    [:pluto,    580,  5, 365 * 247.8],
  ].map do |name, distance, size, year_in_days|
    args.state.new_entity(name) do |p|
      p.path = "sprites/#{name}.png"
      p.distance = distance
      p.s = size
      p.year_in_days = year_in_days
    end
  end

  args.state.ship ||= args.state.new_entity(:ship) do |s|
    s.x = 1280 * rand
    s.y = 720 * rand
    s.angle = 0
  end
end

def to_sprite args, entity
  x = 0
  y = 0

  if entity.year_in_days
    day = args.state.tick_count
    day_in_year = day % entity.year_in_days
    percentage_of_year = day_in_year.fdiv(entity.year_in_days)
    angle = 365 * percentage_of_year
    x = angle.vector_x(entity.distance)
    y = angle.vector_y(entity.distance)
  end

  [640 + x - entity.s.half, 360 + y - entity.s.half, entity.s, entity.s, entity.path]
end

def render args
  args.outputs.solids << [0, 0, 1280, 720]

  args.outputs.sprites += args.state.stars.map do |x, y, _, r, g, b|
    [x, y, 10, 10, 'sprites/star.png', 0, 255, r, g, b]
  end

  args.outputs.sprites << to_sprite(args, args.state.sun)
  args.outputs.sprites += args.state.planets.map { |p| to_sprite args, p }
  args.outputs.sprites << [args.state.ship.x, args.state.ship.y, 20, 20, 'sprites/ship.png', args.state.ship.angle]
end

def calc args
  args.state.stars = args.state.stars.map do |x, y, speed, r, g, b|
    x += speed
    y += speed
    x = 0 if x > 1280
    y = 0 if y > 720
    [x, y, speed, r, g, b]
  end
end

def process_inputs args
  if args.inputs.keyboard.left
    args.state.ship.angle += 1
  elsif args.inputs.keyboard.right
    args.state.ship.angle -= 1
  end

  if args.inputs.keyboard.up
    args.state.ship.x += args.state.ship.angle.x_vector
    args.state.ship.y += args.state.ship.angle.y_vector
  end
end

def tick args
  defaults args
  render args
  calc args
  process_inputs args
end

def r
  $dragon.reset
end
```

# HEAD is now at 3f0bc4e6 Done.

```ruby
def defaults args
  args.state.x ||= 640
  args.state.y ||= 360
  args.state.stars ||= 100.map do
    [1280 * rand, 720 * rand, rand.fdiv(10), 255 * rand, 255 * rand, 255 * rand]
  end

  args.state.sun ||= args.state.new_entity(:sun) do |s|
    s.s = 100
    s.path = 'sprites/sun.png'
  end

  args.state.planets = [
    [:mercury,   65,  5,          88],
    [:venus,    100, 10,         225],
    [:earth,    120, 10,         365],
    [:mars,     140,  8,         687],
    [:jupiter,  280, 30, 365 *  11.8],
    [:saturn,   350, 20, 365 *  29.5],
    [:uranus,   400, 15, 365 *    84],
    [:neptune,  440, 15, 365 * 164.8],
    [:pluto,    480,  5, 365 * 247.8],
  ].map do |name, distance, size, year_in_days|
    args.state.new_entity(name) do |p|
      p.path = "sprites/#{name}.png"
      p.distance = distance * 0.7
      p.s = size * 0.7
      p.year_in_days = year_in_days
    end
  end

  args.state.ship ||= args.state.new_entity(:ship) do |s|
    s.x = 1280 * rand
    s.y = 720 * rand
    s.angle = 0
  end
end

def to_sprite args, entity
  x = 0
  y = 0

  if entity.year_in_days
    day = args.state.tick_count
    day_in_year = day % entity.year_in_days
    entity.random_start_day ||= day_in_year * rand
    percentage_of_year = day_in_year.fdiv(entity.year_in_days)
    angle = 365 * percentage_of_year
    x = angle.vector_x(entity.distance)
    y = angle.vector_y(entity.distance)
  end

  [640 + x - entity.s.half, 360 + y - entity.s.half, entity.s, entity.s, entity.path]
end

def render args
  args.outputs.solids << [0, 0, 1280, 720]

  args.outputs.sprites += args.state.stars.map do |x, y, _, r, g, b|
    [x, y, 10, 10, 'sprites/star.png', 0, 100, r, g, b]
  end

  args.outputs.sprites << to_sprite(args, args.state.sun)
  args.outputs.sprites += args.state.planets.map { |p| to_sprite args, p }
  args.outputs.sprites << [args.state.ship.x, args.state.ship.y, 20, 20, 'sprites/ship.png', args.state.ship.angle]
end

def calc args
  args.state.stars = args.state.stars.map do |x, y, speed, r, g, b|
    x += speed
    y += speed
    x = 0 if x > 1280
    y = 0 if y > 720
    [x, y, speed, r, g, b]
  end

  if args.state.tick_count.mod_zero?(4929)
    args.outputs.sounds << 'sounds/bg.wav'
  end
end

def process_inputs args
  if args.inputs.keyboard.left || args.inputs.controller_one.key_held.left
    args.state.ship.angle += 1
  elsif args.inputs.keyboard.right || args.inputs.controller_one.key_held.right
    args.state.ship.angle -= 1
  end

  if args.inputs.keyboard.up || args.inputs.controller_one.key_held.a
    args.state.ship.x += args.state.ship.angle.x_vector
    args.state.ship.y += args.state.ship.angle.y_vector
  end
end

def tick args
  defaults args
  render args
  calc args
  process_inputs args
end

def r
  $dragon.reset
end
```
