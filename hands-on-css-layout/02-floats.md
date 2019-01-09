---
layout: sublevel
title: Hands-On CSS Layout
---

# Floats

<p><img src="02-floating-boat.jpg" style="width: 150px; float: right; padding: 10px;" />This is a classic example of using floats. Here we have a paragraph with lots of text and an image, with the text flowing around it. Blah blah blah, need more text for plenty of wrapping. See I go on and on, saying nothing useful, just needing more text. Paper boats float. So do ducks. And witches, at least that's what I heard in a movie once. Most rocks won't float. Wood sometimes floats. Some furniture floats. Gosh, this is going on a really long time and I JUST NEED MORE TEXT SO IT WRAPS. I can't guarantee that this will work in all browsers though, so I will type more. I bet Socrates could float. Okay hopefully this is enough text. No? Not really. Let's talk about steak. I like it. Saltgrass is my favorite not-super-expensive steak house and they have an excellent chicken and shrimp embrochette. I am not sure of the spelling of that word.</p>

Then people wanted to use floats to do the layout of pages. That is a pretty crazy idea, but it was and is super common. Let's start laying out some principles.

> If an element is floated, it takes on the characteristics of a block element regardless of the CSS display setting and the content that follows it flows around it.

&lt;div&gt;A div&lt;/div&gt;&lt;span&gt;A span&lt;/span&gt;

The CSS.

<pre>
div, span {
  float: left;
  height: 40px;
  width: 300px;
}

div {
  background-color: #DDD;
}

span {
  background-color: #AAA;
}
</pre>
