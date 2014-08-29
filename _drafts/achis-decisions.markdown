---
layout: post

---

Achis is a private gem that is designed to handle ACH providers for
Servicing. Achis have three main enemies, Servicing, providers and ruby
dependencies. Achis is designed to hide the horrible names that they use.

## Template pattern for providers.

The first architecture decision is to protection from the ugly providers.

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

Variable naming is one of the main problem I see in services, so we
protect from it all the time, we encapsulate Transactions and Returns in
objects with human named attributes. We also provide aliases so Services
codes remains working, but they are only aliases.

## Development processes

WIP.
