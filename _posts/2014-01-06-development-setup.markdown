---
layout: post
title: "Development Setup"
date: 2014-01-06 11:29:44 -0300
comments: true
categories: rails ruby
---

When you join a new application is a brand new world, and nothing seems
to work as expected. You would like a useful README, some scripts to get
you running fast and good documentation. These never happens, but I
thing that this new dev will add useful information from that experience
as this is **not** only needed for him to start.

They have to be able to setup the app and find documentation with the
source, if they don't then **you** have an issue. The application have
to be easy to start on development, because if it is not easy it will
not be easy to **deploy** or to **scale**.

So make sure you can clone the repo and get a working environment with a
single command. If there is no single command you should be adding a
script. Make a development version of the environment variables
available on the repo, to make this even possible.

Move all the configuration to environment variables to make deploys
easier to manage. Having a development version of them help documenting
them. You can use [dotenv][1] to get that with rails.

~~~ sh
MYSQLUSER=useful_for_development
MYSQLPASSWORD=password_is_the_same_in_dev_machines
~~~

If you need some backend applications then use [foreman][2] to start
them using a single Procfile (that will then document that):

~~~ yaml
web:      bundle exec rails server
redis:    redis-server
sidekick: sidekiq -C config/sidekiq.yml
~~~

And foreman reads your .env file by default!

Doing this should prevent some headaches later.

  [1]: https://github.com/bkeepers/dotenv/
  [2]: https://github.com/ddollar/foreman
