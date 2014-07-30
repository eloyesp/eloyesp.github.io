---
layout: post
title: "Forking Tasks"
date: 2013-12-30 14:34
comments: true
categories: ruby rails performance
---

There is a task that takes too much time, we didn't want the user to
wait until it finish so I need to return soon.

I didn't want anything that takes too much effort, I wanted something
simple, so I didn't want to setup a queue or something like that.

The code I had was:

~~~ ruby
def submit
  @opportunity_id = create_opportunity
  add_line_items
  close_opportunity # return nil > 20' later
end
~~~

The idea was to use a forked process to do the hard work, while the user
receive a nice message an keep going. To help with that, I installed
[Spawnling][spawnling] to help.

~~~ ruby
gem 'spawnling', '~> 2.1'    # Background process handling
~~~

Testing that was hard because my tests only passed in the forked process
(think about it), but Spawnling have a config that help with that.

~~~ ruby
before do
  # we need to use a single process to test
  Spawnling.default_options method: :yield
end
~~~

Then we can safely change the code to use it.

~~~ ruby
def submit
  Spawnling.new nice: 3 do
    @opportunity_id = create_opportunity
    add_line_items
    close_opportunity # return nil > 20' later
  end
end
~~~

You can even test the speed, but I don't think that it makes any sense
now (I've done that while developing, but then removed the tests).

  [spawnling]: https://github.com/tra/spawnling
