---
layout: post
title: "Research on testing"
date: 2012-12-11 13:12s
comments: true
categories: 
---

# {{ page.title }} #

This is not my first blog post about testing, I have made one before, but with
other system that difficult blogging code. You can see [here] [1].

What can I say about testing that isn't already said?

Well I can say that **it is really difficult**. At least to begin with.

At the begining it is difficult because you will have to worry about failing
tests twice, once because failing implementations, and also because bad written
tests.

Then it become totaly nonsense, because you will spend more time fixing test
about code that already works. Then... why to add tests?

There are a lot of answers for that, but the frustation of failing tests with
working implementation talks loud and need some answer. But there is a good
answer for it, your tests are failing because your working code have bad smell,
then it is dificult to test and you have failing tests and bad working code.

I will not give you an example, instead will give you [the source] [2] where
this idea come from.

So the general idea when you make TDD is not simply working code, is it about
testable code and as automated test are code, it will help you make code that
can work together with others.

  [1]: http://linuxapesta.blogspot.com.ar/2012/07/testeando-mi-clase.html "Testeando mi clase"
  [2]: http://blog.codeclimate.com/blog/2012/11/28/your-objects-the-unix-way/ "Your Objects The unix-way"
