---
tags: programming
---

HTML carousels are a bad thing, that [is already written][bad], but I've also noticed
that they are not going to leave us, because designers love them and product
owners love them. So I end up liking a lot a different approach taken by
"Javier Villanueva" in his article about [making sure at least they don't kill
the site performance][perf].

In his article he presents the way to have a slider using css **only**. That
way is great, but I found it is quite challenging to display more than a single
slide for what we call a carousel, I found a solution for that, and will share
it here.

<div class="codepen" data-height="300" data-default-tab="result" data-slug-hash="bGmvZQw" data-editable="true" data-user="eloyesp"  data-prefill='{"title":"Microslider variable slides","tags":[],"scripts":[],"stylesheets":[]}'>
  <pre data-lang="html">&lt;div class="grid">
  &lt;div class="item">bla&lt;/div>
  &lt;div class="item">ble&lt;/div>
  &lt;div class="item">blbblbk&lt;/div>
  &lt;div class="item">more&lt;/div>
  &lt;div class="item">a&lt;/div>
  &lt;div class="item">noknmlsd&lt;/div>
  &lt;div class="item">ksldf&lt;/div>
&lt;/div></pre>
  <pre data-lang="css" data-option-autoprefixer="true">.grid {
  --gap: 0.5rem;
  --slides: 5;
  display: grid;
  grid: auto / auto-flow calc((100% + var(--gap)) / var(--slides) - var(--gap));
  background: #16d15642;
  border: #16d156 solid 1px;
  padding: calc(var(--gap) + 0.5px);
  gap: calc(var(--gap) + 0.5px);
  max-width: 400px;
  margin: 1rem auto;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  scroll-padding: var(--gap);
}

.item {
border: solid 1px #666;
background: hsla(250, 80%, 40%, 0.2);
padding: 0.5rem;
scroll-snap-align: start;
}</pre></div>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

The problem is that the gap between slides makes it quite hard to get the
_right width_ for each item to get an integer numbers of items per view. So the
width is the result of a big `calc` that solves. As it is using variables you
can dynamically (or manually) change the number of slides per view.

As I'm using `scroll-snap-align: start`, it also requires adding
`scroll-padding` to the parent, and finally there is some rounding error that
makes it required to add half a pixel to the actual padding and gap.

[bad]: https://shouldiuseacarousel.com/
[perf]: https://itnext.io/javascript-sliders-will-kill-your-website-performance-5e4925570e2b
