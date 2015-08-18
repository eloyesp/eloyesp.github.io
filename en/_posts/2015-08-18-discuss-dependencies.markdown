---
layout: post
title: Discuss dependencies
---

Choosing the right dependencies is an important decision that may lead an
application to success or failure. However it is common to find applications
with lots of unnecessary dependencies that were added without any review
only based on intuition, popularity and marketing. We lack even basic
parameters to discuss alternatives.

It may be caused by the popularity of the idea of being <q>a matter of
taste</q>, but I think that there we can find some categories that enable us to
discuss in a more scientific way which dependency is better for a specific use
case.

## Functionality

We add dependencies looking for features that we need, but we don't want to
code ourselves, this way we have less work to do, that's amazing and make
dependencies great.

But, that is only part of the story, as writing code is only part of the work.
We also need to maintain, integrate, configure and adapt it to the changing
requirements of our application. That can be harder when the code has
**technical debt** and every code has some. Adding a feature through a
dependency seems free at first, but the debt is inherited.

For dependencies, technical debt get worse because there are features that we
don't use, but debt on the is inherited anyway.

Also, dependencies are transitive, so debt is added by each dependency of our
dependencies.

## Modularization

On the other hand, dependencies may be a way to improve the application
architecture, as they are naturally modular.

But it may have flaws when libraries **contaminate** global space, modify
global variables, do *monkey patching* or change global state without
explicit commands.

Another possible breach is using a library across your application, as a change
in the interface will need a big refactor on the app.

## Synergy

It is important to note that there are dependencies meant to change or evolve,
while there are others that solve a specific problem that are quite **stable**.

With stable libraries, there are no evolution to think about, but it is very
important to know the roadmap and objectives of an evolving one, as the
**synergy** between the app and the library will ease future developments,
enabling collaboration with other developers. But if they diverge it will
become difficult to integrate and the number of unused features will grow.

# Conclusion

In my humble opinion, it is important to start discoursing more about adding
dependencies, I think that notions as "technical debt", "contamination",
"stability" and "synergy" may help on those debates.

# Further reading

[Finding modules by Substack][1] and [Popularity by Soberan][2] may be
interesting to know different criteria to chose minimalist libraries.

 [1]: http://substack.net/finding_modules
 [2]: http://soveran.com/popularity.html
