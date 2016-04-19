---
title: Firefox experience
---

I'm been using Firefox for a long time (9 years now, more, I'm not even sure), 
it was a good browser, but something is going wrong.

I've started writing this post on October 2014 when I heard about the new 
Firefox Sync, it was an obscure change and a deep change.

The original Sync had some important differences with the existent 
implementation, and Firefox provided a good detailed explanation of the 
advantages on the approach in [the announce][1]. One of the oddities, was being 
a bit cumbersome, because it needed a passphrase, but it was required to make 
the information encrypted in the server, that way your info was never public.

> Do it all securely: Weave Sync encrypts user data before uploading it to 
Mozilla’s servers, so that only you can access your data. ((source)[2])

The new sync uses passwords, that runs against the server, so the information 
is accessible from the outside. There is a simple test around that, ask what 
happens if you [loose your password][3]. If you can recover your data without 
the password, someone with access to the server also can.

Firefox is transforming in a "big ball of mud", Firefox updates needed a new 
service application because it was too heavy.

In the meantime, while this post was a draft, they added ["Firefox Hello"][4] 
and they improved a lot the [dev-tools][6] (instead of helping [firebug][5]). 
Instead of a modular architecture to build upon, they are building a big 
monolith.

> By far, the most well-known web developer tool in a web browser is the 
Firebug extension, and without a doubt, for a long time it set the bar for how 
web developing and debugging should be.

By now, Firefox displays web-pages with styles, runs JavaScript, keeps 
bookmarks and history, store passwords, keeps form data, manage tabs, submits 
requests, interfaces plugins, display images and reproduce video and audio, 
provides video conferences, and is extendible.

The big issue here is that freedom cannot be built in suites, modularization is 
the key in open source development, and trying to build a big free software is 
just a contradiction. 

 [1]: https://blog.mozilla.org/labs/2007/12/introducing-weave/
 [2]: https://blog.mozilla.org/labs/2009/07/weave-0-5-released/
 [3]: https://support.mozilla.org/en-US/kb/ive-lost-my-firefox-sync-account-information#w_iaove-forgotten-my-sync-password-ae-how-do-i-reset-it_2
 [4]: https://www.mozilla.org/en-US/firefox/hello/
 [5]: https://blog.getfirebug.com/2014/11/10/firebug-3-next-generation-of-firebug/
 [6]: https://hacks.mozilla.org/2011/11/firefox-tons-of-tools-for-web-developers/
