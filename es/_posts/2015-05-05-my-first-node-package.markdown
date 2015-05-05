---
title: First Node Package
layout: post
---

I've been trying Node and NPM for a while and I'm learning a lot about that. To
learn, the recommended approach is to build something that may be basic, but
helps learning a lot in the way.

Then I started building my first package for node, it took two days for actual
coding, but [it works][1].

It started with a [post about bash][2] that we started with [Leo][3] to
investigate how to simple Unix filters in JavaScript to use in bash pipes. We
made something workable, but it was a bit cumbersome for the simple case.

~~~ javascript
#!/usr/bin/env node

var through = require('through');
var split = require('split');

process.stdin.pipe(split()).
  .pipe(through(function(line) {
    this.queue('<li>' + line + '</li>\n'); // <- here is the interesting stuff
  }))
  .pipe(process.stdout);
~~~

Can we make this simple case even more simple? I'm not sure if the
simplification is justified in this case, but *I've learn a lot doing it*.

So we extracted as much as possible to a very simple package to make this possible.

~~~ javascript
#!/usr/bin/env node

var filter = require('filter-helper');

filter(function(line) {
  return '<li>' + line + '</li>';
});
~~~

We loose a lot of flexibility now, but the interesting stuff is clear now.

I've say I've learnt a lot, and this doesn't sound as much work, but there is
a lot of things that surround this package that I didn't know, and one is tests
in npm. In this case, the library abuses `process.stdin` so testing was not
even clear.

One of the coolest things in node is [NPM][4], that I think is really well
designed, so for testing I used `bash` that is a weird idea, but it
[worked][5].

 [1]: https://www.npmjs.com/package/filter-helper
 [2]: http://eloyesp.github.io/es/2015/04/28/cuando-usar-bash/ "When to use bash (in Spanish)"
 [3]: https://github.com/leolower
 [4]: http://npmjs.com/
 [5]: https://github.com/eloyesp/filter-helper/blob/master/test/run
