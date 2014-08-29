---
layout: post
title: Achis design principles
author: Eloy Espinaco

---

Achis is a private gem that is designed to handle ACH providers for
Servicing. Achis have three main enemies, Servicing, providers and ruby
dependencies. Achis is designed to hide the names that they use.

## Template pattern for providers.

The first architecture decision is to protect from the ugly providers.

![> You shall not pass](/images/you-shall-not-pass.jpg)

~~~ ruby
# Download return files and parse them
#
def pull(date)
  files = list_for_date date

  @processed_files = []
  files.map do |remote_file|
    tmp_file = get_file(remote_file)
    @processed_files << tmp_file
    parse_returns tmp_file
  end.flatten
ensure
  connection.close if @connection
end
~~~

Providers have no control on external interface, they will never
define pull or push they can't change the gem. The gem main work is to
hide the complexity, not to communicate that.

## Adapter pattern for external dependencies.

We need to rule over connections.

![> One ring to rule them all](/images/one-ring-to-rule-them-all.jpg)

~~~ ruby
require 'double_bag_ftps'

module Achis
  module ConnectionAdapters
    class FTPS

      attr_accessor :ftps

      def self.start(*args)
        new(*args)
      end

      def initialize host, user, password, account = nil, options
        self.ftps = DoubleBagFTPS.new
        ftps.passive = true
        ftps.debug_mode = true if ENV['DEBUG_CONNECTION']
        ftps.ssl_context = set_ssl_context(options)
        ftps.connect host
        ftps.login user, password, account
      end

      def send_file file_path, remote_path
        ftps.puttextfile file_path, remote_path
      end

      def get_file remote_path
        ftps.gettextfile remote_path, nil
      end

  def glob path, pattern
    ftps.nlst(path).select do |remote_file|
      File.fnmatch pattern, remote_file
    end
  end

  def close
    ftps.close
  end
~~~

end

We need protection of connections interfaces, they use names like
`nlst` or `gettextfile`, we need to encapsulate that horrible names.

This also allows testing providers in isolation and faster development.

## Good and descriptive names

![> We need to work with gollum](/images/working-with-gollum.jpg)

~~~ ruby
module Achis
  class ReturnRecord

    alias :unique_transaction_id :transaction_id
    alias :return_reason_code :nacha_code
    alias :return_date :date
    alias :correction_detail :description

  end
end
~~~

Variable naming is one of the main problem I see in services, so we
protect from it all the time, we encapsulate Transactions and Returns in
objects with human named attributes. We also provide aliases so Services
code keeps working, but they are only aliases.

## Open source

![> Keep corruption away](/images/isildur.jpg)

One of the main goals is to keep the gem clean, for this, the rule is
that Achis should be easy to realease "open source". That makes sure
that Services does not corrupt the code.

This ensure that we keep flexibility and best practices. When code is to
be published you make it better.

## Thanks

![Enjoy](/images/the-green-dragon.jpg)

### Links:

- [Adopt open source process constraints][1]

- [Open source almost everything][2]

 [1]: http://tomayko.com/writings/adopt-an-open-source-process-constraints
 [2]: http://tom.preston-werner.com/2011/11/22/open-source-everything.html
