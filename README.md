---
tags:
level:
languages:
resources:
---

# Cssi 1.9 Advanced CSS
You've seen how to select and style individual CSS elements. Now, we'll look at how to size and position elements on the page.

Sizing and positioning is tricky - the view window isn't always the same size, and elements often need to change size and position relative to each other and to the window.

##Box Model
You can think of all elements on the page as boxes. Each of these boxes has several properties that contribute to its size. These are:
Content: The text, images, child elements, etc. that are inside the element.
Padding: The space between the content and the border. It shares the same background color, image, etc. as the content.
Border: The (possibly decorative) border around your element.
Margin: The space around your element that you want to keep clear of the edges of other elements. It will not share the same background settings as the content.

![box model diagram](http://www.washington.edu/accesscomputing/webd2/student/unit3/images/boxmodel.gif)

By default, when you set width or height on your element, it refers to the width or height of the CONTENT BOX ONLY. You must account for extra width/height if you add any padding, border, and/or margin. If you would like for width and height to also include the padding, use box-sizing: padding-box;, and if you would like it to include both the padding and the border (but still not the margin), use box-sizing: border-box;. In the instructions below, we assume that box-sizing is content-box (the default described above).

##Sizing Content
Content is sized most commonly either by using pixels:

```
p {
  width: 100px;
  height: 50px;
}
```
or by using percents:
```
p {
  width: 75%;
  height: 120%;
}
```
In the first example, the width and height will be the same no matter the element’s relationship to other elements.  The second one, though, takes its width and height based on its parent’s width and height. So, for instance, if the parent of our second element is 100px wide and 200px high, our second element will be 75px wide (75% of 100) and 240px high (120% of 200).

##Sizing Padding and Margins
Padding is sized by the properties padding-top, padding-bottom, padding-right, padding-left, and padding. Similarly, margin is sized by margin-top, margin-bottom, margin-right, margin-left, and margin. These properties are most commonly sized using pixels.

Example:
```
p {
  padding-top: 20px;
  padding-right: 200px;
  margin-left: 20px;
}
```
It is very common to want padding and/or margin on all sides of an element, but typing out each side separately gets very tedious. That’s where the padding and margin properties come in!

Examples:
```
p {
  padding: 100px 20px 30px 50px;
}

div {
  margin: 20px 50px;
  padding: 40px;
}
```
In our `<p>` tag, we see four values listed, which correspond to our four sides: top, right, bottom, left (be careful of the order!). In our `<div>` tag, though, we see a margin property with only two values and a padding property with only one. This is because it is very common to want either a consistent amount of padding/margin on all four sides, or to have the same amount on the top/bottom and the same amount on the left/right. This means our `<div>` has 40px of padding all around, and has 20px of margin on the top and bottom and 50px on the left and right (again, careful of the order! Top/bottom is first, then left/right).

NOTE: Margins do something frustrating special called “margin collapsing.” When two elements, each with a specified margin, line up one on top of the other, their margins are “collapsed,” meaning that, instead of adding the two margins together, the browser chooses the largest of the two margins, and that becomes the space between the elements. This only happens with top/bottom margins, though, not left/right margins. For example, if we had two elements like the following:
```
p {
  margin: 50px;
}

div {
  margin: 20px;
}
```
If the `<p>` were above the `<div>` (or vice-versa), the space between them would be max(50, 20) = 50px, but if <p> were to the left of `<div>`, the space between them would be 20 + 50 = 70px.
Margin collapsing actually has more rules than this, but this is the most common case.

##Sizing Borders
Similar to padding and margin, border can be set using border-top, border-bottom, border-right, border-left and border. Unlike with padding and margin, though, we set the style and the color of the border in addition to its width.
The syntax goes
```
  border: width style color;
```
Examples:
```
border: 3px solid red;
border: 10px dashed #0000FF;
```
You may also wish to have a border with rounded corners. This is achieved using the property border-radius, which can be set using pixels or percents.

Example:
```
border-radius: 10px;
```
border-radius is also how we make elements appear like ovals or circles. If you set your border-radius to 50%, it will automatically become an oval. If, in addition, your content + padding makes a square, setting your border-radius to 50% will make a circle.

##Display and Positioning
This section covers some of the least intuitive parts of CSS. Buckle your seatbelts.
###Display
The display property has [many possible settings](https://developer.mozilla.org/en-US/docs/Web/CSS/display). The most commonly used are none, inline, block, and inline-block.
```
display: none;
```
As you might expect, this setting means that the block is not displayed on the page at all. In addition, it will not take up any space on the page, even if you set its width and height. Why, then, would we want this setting? It is especially useful for things like drop-down menus that only appear when hovered over. For example,
```
nav.dropdown {
  display: none;
  width: 200px;
  position: absolute;
}

a.link-to-dropdown:hover > nav.dropdown {
  display: block;
}
```
Here we set all the properties of the drop-down menu in its original definition, but set its display to none. Then when the user hovers over its parent, all the other properties stay the same, but the dropdown’s display is changed to block, making it visible (note: we could have waited to define all of the other properties under :hover, but prefer this way. It is more semantic; the drop-down menu always exists, it just becomes visible when hovered over).
```
display: inline;
```
An element with display: inline; takes up no more space than is necessary to cover its content. If there are multiple elements whose display is set to inline, they will be allowed to share the same line. [Here](https://developer.mozilla.org/en-US/docs/Web/HTML/Inline_elemente) is a list of all the elements whose display property defaults to inline; the most common are `<a>`, `<br>`, and `<span>`, as well as all of the form elements (`<input>`, `<textarea>`, `<button>`, etc.).

```
display: block;
```
An element with display: block; will automatically take up the entire line it is on if no width is set (even if its content doesn’t take up the whole line); otherwise it will make a rectangle with a height just enough to cover all its content. An element whose display is set to block will not share a line with any other element unless some other property intervenes (see [Float](https://docs.google.com/document/d/1txE9GpKF3CtZXZBgcma5v3W-FRlOEuZJZ8QHFAvlG4w/edit#heading=h.vmcqvnili4n0)). [Here](https://developer.mozilla.org/en-US/docs/Web/HTML/Block-level_elements) is a list of all the elements whose display property defaults to block; the most common are `<div>`, `<p>`, `<ul>`, `<ol>`, all the headings, `<article>`, `<section>`, and `<nav>`.
```
display: inline-block;
```
As you may guess, an element with display: inline-block; shares some of its properties with display: inline; elements and some with display: block; elements. It sets its width and height in the same way as block, but can exist on the same line as other inline and inline-block elements. There are no tags that default to inline-block.

##Examples
Here is some HTML and CSS for two elements. Below you can see an example of what happens with the elements when their display property is set to inline, block, and inline-block, respectively.
```
<!-- HTML -->
<div class="red">This is the first element.</div>
<div class="green">This is the second element.</div>

/* CSS */
div.red {
  background: red;
  width: 100px;
}

div.green {
  background: green;
  width: 100px;
}
```

![Examples of three different display properties](http://4.bp.blogspot.com/-TiwOixlooJk/U4UyEnv_XpI/AAAAAAAACFs/NuuLz2IvoZ4/s1600/css-display-block-vs-inline-block.png)

##Float
Elements have a property float, which can be set to left, right, or none. float is a property that should be used on block elements, and, in fact, if you set float on a non-block element, its display property will be over-written to become block (more details here). By default the float property is set to none.
If float is set to left, the element will automatically “float” up and to the left until it hits either the edge of its parent element or the edge of another element with float: left; (the rule is similar for float: right;).

Example:
```
<!-- HTML -->
<section>
   <div style="background: red"></div>
   <div style="background: green"></div>
   <div style="background: blue"></div>
   <div style="background: orange"></div>
   <div style="background: purple"></div>
</section>
<!-- NOTE: In general, it’s bad practice to use the ‘style’ attribute; you should use CSS instead. We’re using it here for conciseness. -->

/* CSS */
section {
  width: 300px;
  height: 500px;
  border: 2px solid black;
}

```
![Three examples of floats](https://lh4.googleusercontent.com/-P-MsEKlMZi4/UkLJQdWErkI/AAAAAAAAAYA/F93J7DKS0UQ/s642/CU01034D_1.png)  
Pro-tip: Using display: inline-block; is messy. Don’t use it. Instead, always use display: block; and float: left; (or right) in combination.

##Clear
Say we have an element we have floated, and another non-floating element that we want to appear below the first:
```
<!-- HTML -->
<div></div>
<section></section>

/* CSS */
div {
  float: right;
  height: 100px;
  width: 100px;
  background: blue;
}

section {
  height: 300px;
  background: orange;
}
```
Looks good, but what do we get?

![Blue box in the top corner of orange box](http://i.imgur.com/2lCVfvO.png)

Uh oh, we wanted the orange box below the blue box, not behind. It turns out that blocks that are floated take up no space in relation to elements that are not floated.

Luckily for us, we have the property clear which will tell the non-floating elements to “clear” (i.e., appear below) our floating blocks. We can give them the option to clear: left; (only clear the blocks floated left), clear: right; (only clear the blocks floated right), or clear: both; (clear all floating blocks).

So, change our CSS:
```
section {
  …
  clear: both;
}
```
And we get:

![Blue box on top of orange box](http://i.imgur.com/wLhnXJl.png)

Which is what we expected.

##Clearfix
Here’s an example of a common CSS problem. We want to put, say, a picture and a block of text and have them float next to each other inside another block (we’ll make it pink with an orange border). Here’s our code:
```
<!-- HTML -->
<section>
  <div>:)</div>
  <p>Here is a lovely example of text.</p>
</section>

/* CSS */
section {
  border: 2px solid orange;
  background: pink;
}

div {
  float: left;
  font-size: 100px;
  background: #fff;
  border: 1px solid black;
  padding: 0 20px 20px 20px;
  margin: 10px;
}

p {
  float: left;
  padding: 20px
}
```
The code looks great! But here’s what we get:
![Smileyface text no  background](http://i.imgur.com/mqxHnTF.png)

What happened to our pink background box? Well, it turns out that when you float an element, it no longer takes up space in its parent element. So in our example above, the `<section>` element collapsed to its minimum height, rendering only the border as a single orange line.
How do we fix this? One option is to hard-code in a height for the `<section>`. This could be ok if we only ever use the `<section>` in this particular context, but what if the text or picture changes? Maybe for some people we want the text to be paragraphs and paragraphs long, and for others we want it to be only a few words. It would be much better if we could find a way for the `<section>` to take up exactly as much space as needed to wrap around its contents.

The solution to this is called a “clearfix,” and to code it we need a very special block of CSS:
```
.clearfix:after {
  content: "";
  display: block;
  clear: both;
}
```
What this is saying is that, for every element with class=”clearfix”, after all its child elements we insert a block with no real content that clears everything floated on both sides.

Now, just add the above to our CSS and change our HTML like so:
```
<section class="clearfix">
   …
</section>
```
And voilá!
![Smileyface text no  background](http://i.imgur.com/vc3ccGc.png)


Just what we wanted.

##Position
The last major topic in display and positioning is the position property. position has four possible values: static, relative, absolute, and fixed. By default, all elements have `position: static;`.
```
position: static;
```
All positioning we have talked about so far has assumed position was set to static. For `position: static;`, the display property can be set to things other than block, but the top, bottom, left, and right properties do not work.
```
position: relative;
```
An element with `position: relative;` takes up space as you would expect in the normal flow of the page, but you can then move it using the top, bottom, left, and right properties, usually measured in pixels. It is moved relative to where it would have otherwise been on the page.
Example:
```
<!-- HTML -->
<section>
   <div></div>
</section>

/* CSS */
section {
   border: 2px solid black;
   width: 100px;
}

div {
   position: relative;
   height: 100px;
   width: 100px;
   top: 150px;
   left: 300px;
   background: green;
}
```
We get:
![White box, green box](http://i.imgur.com/ThNKJBb.png)


The green box normally would have ended up in the square border, but it has been placed 150px down from the top and 300px from the left. Also notice that, while we set a width for our `<section>`, we did not set a height. It maintains a height of 100px, though, because our 100px-high `<div>` is still taking up the space it would have in its parent element.
This behavior is not something we usually want, so in most cases `position: relative;` should only be used in conjunction with `position: absolute;`, as described later.
```
position: absolute;
```
An object with `position: absolute;` does not take up space in the normal flow of objects. Without any of the top, bottom, left, and right properties set it will appear in the top left corner of its nearest parent element with a position other than static. If all of its parent elements have `position: static;` (or no position set), it will appear in the top left corner of the window.

Example:
```
<!-- HTML →
<div></div>
<p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p> <!-- random text -->

/* CSS */
div {
  position: absolute;
  top: 20px;
  right: 150px;
  width: 100px;
  height: 100px;
  background: blue;
}
```
We get:

![Blue box on text - absolute](http://i.imgur.com/DY1GNMQ.png)


Notice that the block takes up no space in relation to the text, either where it currently is or where it would have been before we moved it with top, bottom, left, and/or right.  It has been placed 20px from the top and 150px from the right.

Pro-tip: If you use `position: absolute;` on an element, always set `position: relative;` for the element’s parent. This will help prevent surprises with positioning in differently-sized windows and shouldn’t affect how the parent renders at all.
```
position: fixed;
```
An element with `position: fixed;` does not take up space in the normal flow of objects. It is very similar to `position: absolute;` except for one key feature: it will always stay in the exact same place in the window, even if you scroll in any direction. This makes it a popular choice for headers (and sometimes footers) if you want them to follow the user around the page.

Warning: `position: fixed;` is a dangerous feature. If you make a typo, or if the user’s window is configured differently than what you planned for, it is possible to make an element whose position is fixed appear off the screen originally. This means that, since it will always stay in the same place relative to the window, the user will never see it. Use `position: fixed;` only with care.
