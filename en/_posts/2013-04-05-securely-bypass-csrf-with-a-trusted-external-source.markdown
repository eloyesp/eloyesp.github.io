---
title: "Securely bypass csrf with a trusted external source"
date: 2013-04-05 11:22
comments: true
tags: [ rails ]
---

You want your rails application to respond to an external api and recive
a warning about the verification of the csrf token. How to get rid of
this warning? What is csrf about.

<!-- more -->

Cross Site Request Forgery (csrf) is the use of an open session with a
site to use this credentials to force a legit user of the application to
do something on the site while he didn't mean that.

But what if you want to interact with the user from other site or you
want the application to recive commands from other sites without any
session.

Well, in this case you have to do something with the
`verify_authenticity_token` filter.

## What does it do? ##

It do just two thinks, once it found that the crsf is not present or is
invalid, it log a warning and destroy the session.

If the session is not required for this action, then you can simply skip
it to get rid of the warning, but do it **only for these actions!**. To
add security about the source of the call you can generate an api token
and validate it within the controller.

If you are accessing from another site, but with the user logged in, you
have tok mannage to add in the form the required token.
