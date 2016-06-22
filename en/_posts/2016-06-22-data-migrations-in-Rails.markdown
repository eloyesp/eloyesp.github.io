---
title: Data Migrations in Rails
---

There are times where there are changes to make on data on a production database,
the first bad idea that comes to mind is using a Rails Migration, but that is not
a good idea, as already explained in [the blog post: Data migrations in Rails][1].

There they recommend using a temporary rake task for that, but they mention some
drawback:

> The downside is that we need remember to either add this rake task to our
deployment script or run the rake task manually after deployment. We will also
need to clean up after ourselves and remove the temporary rake task once the
changes have been deployed and implemented.

The problem is that here they are abusing rake, as the first approach is abusing
migrationsthis. Rake tasks are not good for this as they have no scope separation,
then adding complex tasks is asking for problems, they are difficult to test, they
add noice in the project.

There is another solution, just write a script.

So instead of the rake task:

```ruby
# lib/tasks/temporary/users.rake
namespace :users do
  desc "Update confirmed users to receive newsletter"
  task set_newsletter: :environment do
    users = User.confirmed
    puts "Going to update #{users.count} users"

    ActiveRecord::Base.transaction do
      users.each do |user|
        user.mark_newsletter_received!
        print "."
      end
    end

    puts " All done now!"
  end
end
```

You just write a script:

```ruby
# tmp/update_confirmed_users_to_receive_newsletterusers.rb
users = User.confirmed
puts "Updating #{users.count} users."

User.transaction do
  users.each do |user|
    user.mark_newsletter_received!
    print "."
  end
  puts " All done now!"
end
```

Running the script is straighforward, just use [`rails runner`][2]:
`rails r tmp/update_confirmed_users_to_receive_newsletterusers.rb`.

As you are not going to mantain that it doesn't make any sence to commit them, but
you may store those files anywhere and document how to use them in a readme when
they are used repeatedly.

# Writings scripts

To build those scripts it will be usefull to write tests, in those tests just `require`
the file to run the script, you can also `load` the file to run it multiple times and
use transactions to tests with different data:

```ruby
test "should only mark confirmed users" do
  unconfirmed = User.create! name: 'Foo'
  confirmed = User.create! name: 'Foo', email: 'a@example.com', confirmed: true
  load 'tmp/update_confirmed_users_to_receive_newsletterusers'
  assert confirmed.newsletter_received
  refute unconfirmed.newsletter_received
end
```

Environment variables or arguments can be used to configure *dry runs*, secrets, etc:

```ruby
# tmp/update_confirmed_users_to_receive_newsletterusers.rb
...
User.transaction do
  users.each { |user| ... }
  puts "All done now!"
  if ENV['DRY_RUN']
    puts "Reverting now (dry run)"
    raise ActiveRecord::Rollback
  end
end
```

To discuss with other developers, it may be usefull to use gists, it have all the
adventages of github without polluting the main repo.

**Note for environments as heroku** that you cannot copy the file and then run it, in that
case, you can download the gist and run it in the same session or running it directly quoting
the script.

 [1]: https://robots.thoughtbot.com/data-migrations-in-rails
 [2]: http://guides.rubyonrails.org/command_line.html#rails-runner
