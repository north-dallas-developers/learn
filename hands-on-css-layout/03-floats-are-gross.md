---
layout: sublevel
title: Hands-On CSS Layout
---

# Why Some People Think Floats Are Gross

Other than just being an awkward way of talking about layout, floats have some oddities that people find annoying.

> Two elements cannot float next to each other if the width of the two is greater than the available width of the container.

&lt;div class=\"left\">Left&lt;/div>
&lt;div class=\"right\">Right&lt;/div>

<pre>
div, span {
  float: left;
  min-height: 200px;
  width: 51%;
}

.left {
  background-color: #DDD;
}

.right {
  background-color: #AAA;
}
</pre>

This often annoys people but at the same time it enables some interesting things.

> Width is determined by the following formula unless you change the box sizing: left margin + left border + left padding + content width + right padding + right border + right margin.

That's only true if you don't change the box model, but perhaps more on that later.

> I recommend always setting an explicit width for floated elements so they will behave consistently.

This is especially good to keep in mind when the content can change over time.

> Clear your floats when you are done with them so that the float behavior gets turned off.

&lt;div class=\"left\">Left&lt;/div>
&lt;div class=\"right\">Right&lt;/div>
&lt;p>This is a paragraph and I don't have it floated!&lt;/p>

<pre>
div, span {
  float: left;
  min-height: 200px;
}

.left {
  background-color: #DDD;
}

.right {
  background-color: #AAA;
}

p {
  clear: both;
}
</pre>

Floats take the floated elements out of the normal document flow, and this can have some strange side-effects. So "clear: both;" on the following element or use some sort of [clearfix](https://css-tricks.com/snippets/css/clear-fix/) strategy.

> If all the child elements of a container are floated, that container will collapse its size unless an overflow value is set.

&lt;div class=\"outer\">
  &lt;div class=\"inner\">First&lt;/div>
  &lt;div class=\"inner\">Second&lt;/div>
&lt;/div>

<pre>
.outer {
  background-color: #AAA;
  overflow: auto;
}

.inner {
  background-color: #DDD;
  float: left;
  margin: 5px;
}
</pre>

<p>Remove the overlow then the float and see what happens.</p>

## Exercise

Remember [the horizontal menu we made with inline-block](https://codepen.io/mallioch/pen/JbYGKV?editors=1100#0)? Use floats instead.