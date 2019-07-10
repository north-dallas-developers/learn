---
layout: sublevel
title: Hands-On DragonRuby - Session 2
---

## 1 - Me: Left Paddle

```ruby
def tick args

  args.outputs.solids << [5, 300, 15, 100]

end
```

## 2 - Me: Move the shield

Note: you can reset the game by hitting the backtick backtick key (on the top left) and typing `$dragon.reset`.

```ruby
def tick args

  # Default start position
  args.state.shield_y ||= 300

  # If the "up" key goes up...
  if args.inputs.keyboard.key_up.up
    # ...change the y value
    args.state.shield_y += 20
  end

  args.outputs.solids << [5, args.state.shield_y, 15, 100]

end
```

## 3 - You: Can you make it move downward?

```ruby
def tick args

  args.state.shield_y ||= 300

  if args.inputs.keyboard.key_up.up
    args.state.shield_y += 20
  end
  
  # This makes it move down when the "down" key is pushed
  if args.inputs.keyboard.key_up.down
    args.state.shield_y -= 20
  end

  args.outputs.solids << [5, args.state.shield_y, 15, 100]

end
```

## 4 - Me: Give the shield a color

```ruby
def tick args

  args.state.shield_y ||= 300

  if args.inputs.keyboard.key_up.up
    args.state.shield_y += 20
  end
  
  if args.inputs.keyboard.key_up.down
    args.state.shield_y -= 20
  end

  # Add three numbers to the end for RGB. You can create any color as a combination of red, green, and blue.
  args.outputs.solids << [5, args.state.shield_y, 15, 100, 50, 100, 200]

end
```


## 5 - Me: How would you keep the shield from going off the top of the screen?

```ruby
def tick args

  args.state.shield_y ||= 300

  if args.inputs.keyboard.key_up.up
    args.state.shield_y += 20
    
    # If the y value is too high, it is off the screen. So this enforces a max y.
    if args.state.shield_y > 620
      args.state.shield_y = 620
    end
  end
  
  if args.inputs.keyboard.key_up.down
    args.state.shield_y -= 20
  end

  args.outputs.solids << [5, args.state.shield_y, 15, 100, 50, 100, 200]

end
```

## 6 - You: How would you keep the shield from going off the bottom of the screen?

```ruby
def tick args

  args.state.shield_y ||= 300

  if args.inputs.keyboard.key_up.up
    args.state.shield_y += 20
    
    if args.state.shield_y > 620
      args.state.shield_y = 620
    end
  end
  
  if args.inputs.keyboard.key_up.down
    args.state.shield_y -= 20

    # If the y value is too low, it is off the screen. So this enforces a min y.
    if args.state.shield_y < 0
      args.state.shield_y = 0
    end
  end

  args.outputs.solids << [5, args.state.shield_y, 15, 100, 50, 100, 200]

end
```

## 7 - Me: Create an animated square

```ruby
def tick args

  args.state.shield_y ||= 300
  # set a default place for the ball
  args.state.ball_x ||= 900
  args.state.ball_y ||= 360

  if args.inputs.keyboard.key_up.up
    args.state.shield_y += 20
    
    if args.state.shield_y > 620
      args.state.shield_y = 620
    end
  end
  
  if args.inputs.keyboard.key_up.down
    args.state.shield_y -= 20

    if args.state.shield_y < 0
      args.state.shield_y = 0
    end
  end
  
  # moves the ball left
  args.state.ball_x -= 1
  

  args.outputs.solids << [5, args.state.shield_y, 15, 100, 50, 100, 200]
  
  # add the ball
  args.outputs.solids << [args.state.ball_x, args.state.ball_y, 10, 10]

end
```

### 8 - Me: Auto-reset

```ruby
# With this at the top, the game resets every time we save the file
$gtk.reset

def tick args

  args.state.shield_y ||= 300
  args.state.ball_x ||= 900
  args.state.ball_y ||= 360

  if args.inputs.keyboard.key_up.up
    args.state.shield_y += 20
    
    if args.state.shield_y > 620
      args.state.shield_y = 620
    end
  end
  
  if args.inputs.keyboard.key_up.down
    args.state.shield_y -= 20

    if args.state.shield_y < 0
      args.state.shield_y = 0
    end
  end
  
  args.state.ball_x -= 1
  
  args.outputs.solids << [5, args.state.shield_y, 15, 100, 50, 100, 200]
  
  args.outputs.solids << [args.state.ball_x, args.state.ball_y, 10, 10]

end
```

## 9 - Me: Reset ball on collision

```ruby
$gtk.reset

def tick args

  args.state.shield_y ||= 300
  args.state.ball_x ||= 900
  args.state.ball_y ||= 360

  if args.inputs.keyboard.key_up.up
    args.state.shield_y += 20
    
    if args.state.shield_y > 620
      args.state.shield_y = 620
    end
  end
  
  if args.inputs.keyboard.key_up.down
    args.state.shield_y -= 20

    if args.state.shield_y < 0
      args.state.shield_y = 0
    end
  end
  
  args.state.ball_x -= 5

  # This works but is hard to read and maintain
  if [5, args.state.shield_y, 15, 100, 50, 100, 200].intersects_rect? [args.state.ball_x, args.state.ball_y, 10, 10]
    args.state.ball_x = 900
  end
    
  args.outputs.solids << [5, args.state.shield_y, 15, 100, 50, 100, 200]
  
  args.outputs.solids << [args.state.ball_x, args.state.ball_y, 10, 10]

end 
```

## 10 - Me: Using variables to clean up code

```ruby
$gtk.reset

def tick args

  args.state.shield_y ||= 300
  args.state.ball_x ||= 900
  args.state.ball_y ||= 360

  if args.inputs.keyboard.key_up.up
    args.state.shield_y += 20
    
    if args.state.shield_y > 620
      args.state.shield_y = 620
    end
  end
  
  if args.inputs.keyboard.key_up.down
    args.state.shield_y -= 20

    if args.state.shield_y < 0
      args.state.shield_y = 0
    end
  end
  
  args.state.ball_x -= 5

  # by putting the rectangles in variables, things become easier to read
  shield = [5, args.state.shield_y, 15, 100, 50, 100, 200]
  ball = [args.state.ball_x, args.state.ball_y, 10, 10]

  if shield.intersects_rect? ball
    args.state.ball_x = 900
  end
    
  args.outputs.solids << shield
  args.outputs.solids << ball

end
```

## 11 - Me: Adding labels for scores

```ruby
$gtk.reset

def tick args

  args.state.shield_y ||= 300
  args.state.ball_x ||= 900
  args.state.ball_y ||= 360

  if args.inputs.keyboard.key_up.up
    args.state.shield_y += 20
    
    if args.state.shield_y > 620
      args.state.shield_y = 620
    end
  end
  
  if args.inputs.keyboard.key_up.down
    args.state.shield_y -= 20

    if args.state.shield_y < 0
      args.state.shield_y = 0
    end
  end
  
  args.state.ball_x -= 5

  shield = [5, args.state.shield_y, 15, 100, 50, 100, 200]
  ball = [args.state.ball_x, args.state.ball_y, 10, 10]

  if shield.intersects_rect? ball
    args.state.ball_x = 900
  end
    
  args.outputs.solids << shield
  args.outputs.solids << ball

  # Adding some labels for keeping track of scores
  args.outputs.labels << [20, 20, "Losses: %i" % 0]
  args.outputs.labels << [20, 40, "Wins: %i" % 0]

end
```

## 12 - You: Can you add a value to state to represent wins, add 1 when the ball hits the shield, and display it?

```ruby
$gtk.reset

def tick args

  args.state.shield_y ||= 300
  args.state.ball_x ||= 900
  args.state.ball_y ||= 360
  # Create a default value for 0
  args.state.wins ||= 0

  if args.inputs.keyboard.key_up.up
    args.state.shield_y += 20
    
    if args.state.shield_y > 620
      args.state.shield_y = 620
    end
  end
  
  if args.inputs.keyboard.key_up.down
    args.state.shield_y -= 20

    if args.state.shield_y < 0
      args.state.shield_y = 0
    end
  end
  
  args.state.ball_x -= 5

  shield = [5, args.state.shield_y, 15, 100, 50, 100, 200]
  ball = [args.state.ball_x, args.state.ball_y, 10, 10]

  if shield.intersects_rect? ball
    args.state.ball_x = 900
    # Increment whenever the ball hits the shield
    args.state.wins += 1
  end
    
  args.outputs.solids << shield
  args.outputs.solids << ball

  args.outputs.labels << [20, 20, "Losses: %i" % 0]
  # Use the value
  args.outputs.labels << [20, 40, "Wins: %i" % args.state.wins]

end
```

## 13 - You: Can you figure out how to add up losses when the ball misses?

```ruby
$gtk.reset

def tick args

  args.state.shield_y ||= 300
  args.state.ball_x ||= 900
  args.state.ball_y ||= 360
  args.state.wins ||= 0
  # Create a default value for 0
  args.state.losses ||= 0

  if args.inputs.keyboard.key_up.up
    args.state.shield_y += 20
    
    if args.state.shield_y > 620
      args.state.shield_y = 620
    end
  end
  
  if args.inputs.keyboard.key_up.down
    args.state.shield_y -= 20

    if args.state.shield_y < 0
      args.state.shield_y = 0
    end
  end
  
  args.state.ball_x -= 5

  shield = [5, args.state.shield_y, 15, 100, 50, 100, 200]
  ball = [args.state.ball_x, args.state.ball_y, 10, 10]

  if shield.intersects_rect? ball
    args.state.ball_x = 900
    args.state.wins += 1
  end

  # If ball goes off the screen
  if ball[0] < 0
    args.state.ball_x = 900
    args.state.losses += 1
  end
    
  args.outputs.solids << shield
  args.outputs.solids << ball

  # Use the value
  args.outputs.labels << [20, 20, "Losses: %i" % args.state.losses]
  args.outputs.labels << [20, 40, "Wins: %i" % args.state.wins]

end
```

## 14 - Me: Change vertical position of the ball

```ruby
$gtk.reset

def tick args

  args.state.shield_y ||= 300
  args.state.ball_x ||= 900
  args.state.ball_y ||= 360
  args.state.wins ||= 0
  args.state.losses ||= 0

  if args.inputs.keyboard.key_up.up
    args.state.shield_y += 20
    
    if args.state.shield_y > 620
      args.state.shield_y = 620
    end
  end
  
  if args.inputs.keyboard.key_up.down
    args.state.shield_y -= 20

    if args.state.shield_y < 0
      args.state.shield_y = 0
    end
  end
  
  args.state.ball_x -= 5
  
  # on each tick, change the y value
  args.state.ball_y -= 1.8

  shield = [5, args.state.shield_y, 15, 100, 50, 100, 200]
  ball = [args.state.ball_x, args.state.ball_y, 10, 10]

  if shield.intersects_rect? ball
    args.state.ball_x = 900
    # reset starting place
    args.state.ball_y = 360
    args.state.wins += 1
  end

  if ball[0] < 0
    args.state.ball_x = 900
    # reset starting place
    args.state.ball_y = 360
    args.state.losses += 1
  end
    
  args.outputs.solids << shield
  args.outputs.solids << ball

  args.outputs.labels << [20, 20, "Losses: %i" % args.state.losses]
  args.outputs.labels << [20, 40, "Wins: %i" % args.state.wins]

end
```

## 15 - Me: Extracting functions

```ruby
$gtk.reset

# new function to start new round
def reset_round
  $gtk.args.state.ball_x = 900
  $gtk.args.state.ball_y = 360
end

def tick args
  
  args.state.shield_y ||= 300
  args.state.ball_x ||= 900
  args.state.ball_y ||= 360
  args.state.wins ||= 0
  args.state.losses ||= 0

  if args.inputs.keyboard.key_up.up
    args.state.shield_y += 20
    
    if args.state.shield_y > 620
      args.state.shield_y = 620
    end
  end
  
  if args.inputs.keyboard.key_up.down
    args.state.shield_y -= 20

    if args.state.shield_y < 0
      args.state.shield_y = 0
    end
  end
  
  args.state.ball_x -= 5
  args.state.ball_y -= 1.8

  shield = [5, args.state.shield_y, 15, 100, 50, 100, 200]
  ball = [args.state.ball_x, args.state.ball_y, 10, 10]

  if shield.intersects_rect? ball
    # reset
    reset_round
    args.state.wins += 1
  end

  if ball[0] < 0
    # reset
    reset_round
    args.state.losses += 1
  end
    
  args.outputs.solids << shield
  args.outputs.solids << ball

  args.outputs.labels << [20, 20, "Losses: %i" % args.state.losses]
  args.outputs.labels << [20, 40, "Wins: %i" % args.state.wins]

end
```


## 16 - Me: Change vertical direction of the ball randomly

```ruby
$gtk.reset

# new function to start new round
def reset_round
  $gtk.args.state.ball_x = 900
  $gtk.args.state.ball_y = 360
  
  # randomly pick a positive or negative directions
  if rand(2) == 0
    $gtk.args.state.direction = -1
  else
    $gtk.args.state.direction = 1
  end
end

def tick args
  
  args.state.shield_y ||= 300
  args.state.ball_x ||= 900
  args.state.ball_y ||= 360
  args.state.wins ||= 0
  args.state.losses ||= 0
  # add a default direction
  args.state.direction ||= -1

  if args.inputs.keyboard.key_up.up
    args.state.shield_y += 20
    
    if args.state.shield_y > 620
      args.state.shield_y = 620
    end
  end
  
  if args.inputs.keyboard.key_up.down
    args.state.shield_y -= 20

    if args.state.shield_y < 0
      args.state.shield_y = 0
    end
  end
  
  args.state.ball_x -= 5
  # multiply the change in y * the direction to go up or down
  args.state.ball_y -= args.state.direction * 1.8

  shield = [5, args.state.shield_y, 15, 100, 50, 100, 200]
  ball = [args.state.ball_x, args.state.ball_y, 10, 10]

  if shield.intersects_rect? ball
    reset_round
    args.state.wins += 1
  end

  if ball[0] < 0
    reset_round
    args.state.losses += 1
  end
    
  args.outputs.solids << shield
  args.outputs.solids << ball

  args.outputs.labels << [20, 20, "Losses: %i" % args.state.losses]
  args.outputs.labels << [20, 40, "Wins: %i" % args.state.wins]

end
```

## 17 - Me: Randomly set the amount of the change in y

```ruby
$gtk.reset

# new function to start new round
def reset_round
  $gtk.args.state.ball_x = 900
  $gtk.args.state.ball_y = 360
  
  if rand(2) == 0
    $gtk.args.state.direction = -1
  else
    $gtk.args.state.direction = 1
  end
  
  # after each reset, change the amount
  $gtk.args.state.dy = rand() * 1.9
end

def tick args
  
  args.state.shield_y ||= 300
  args.state.ball_x ||= 900
  args.state.ball_y ||= 360
  args.state.wins ||= 0
  args.state.losses ||= 0
  args.state.direction ||= -1
  # Add a variable with default for the change in y per tick
  args.state.dy ||= rand() * 1.9

  if args.inputs.keyboard.key_up.up
    args.state.shield_y += 20
    
    if args.state.shield_y > 620
      args.state.shield_y = 620
    end
  end
  
  if args.inputs.keyboard.key_up.down
    args.state.shield_y -= 20

    if args.state.shield_y < 0
      args.state.shield_y = 0
    end
  end
  
  args.state.ball_x -= 5
  args.state.ball_y -= args.state.direction * args.state.dy

  shield = [5, args.state.shield_y, 15, 100, 50, 100, 200]
  ball = [args.state.ball_x, args.state.ball_y, 10, 10]

  if shield.intersects_rect? ball
    reset_round
    args.state.wins += 1
  end

  if ball[0] < 0
    reset_round
    args.state.losses += 1
  end
    
  args.outputs.solids << shield
  args.outputs.solids << ball

  args.outputs.labels << [20, 20, "Losses: %i" % args.state.losses]
  args.outputs.labels << [20, 40, "Wins: %i" % args.state.wins]

end
```