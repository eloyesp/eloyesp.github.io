---
title: Using npm for rubygems
---

> Si no puedes vencer al enemigo, únete a el

I think it is pretty obvious that node is making for packages a better work than ruby, npm is a better tool to install and search for modules. Improving rubygems seems like the think we should be doing, but it will not solve the problem, when working in a ruby based web-app we will need npm anyway.

So why not using npm to install gems, is that even possible? Well, there are some issues, rubygems are not in the registry, they lack a `package.json` and ruby does not look for gems in the `node_modules` folder.

I was thinking, and those issues can be solved with something like the following script:

```ruby
#!/usr/bin/env ruby
# Usage: buildpackage some_folder/gem_name.gemspec
require 'json'
require 'fileutils'

directory, file = ARGV.first
FileUtils.cd directory

s = Gem::Specification.load file

fullname = "#{ s.name }-#{ s.version }"

package = { name: "@rubygems/#{s.name}",
            version: s.version,
            description: s.description,
            scripts: {
              postinstall: "ln -s $PWD $GEM_HOME/gems/#{ fullname }; ln -s $PWD/#{s.name}.minigemspec $GEM_HOME/specifications/#{ fullname }.gemspec"
            }
          }

dependencies = s.dependencies.map do |d|
  next unless d.runtime?
  [ "@rubygems/#{d.name}", d.requirement.as_list.join ]
end.compact

if dependencies.any?
  package[:dependencies] = dependencies.to_h
end

File.write "package.json", JSON.pretty_generate(package)
File.write "#{s.name}.minigemspec", s.for_cache.to_ruby
```

This will write a minimal `package.json` and a simplier gemspec to the gem, making that suitable for npm. There are three things to note:

### Scoped

The package is scoped in `@rubygems` as all the dependencies, so the packages can have dependencies with the same name as a "real" package.

### Linked

There is a postinstall script that will link the gem in the `GEM_HOME` (where `gem install` would have installed it) and the gemspec will be linked there too.

### Mini-spec

The spec linked is similar to the one that would install `gem install` (with stub comment) as most gemspecs will not work as is.

## TODO

I've only tested it locally with two simple gems (and one dependency), it is lacking more details in the package.json, it is lacking support for `executables` (or bins in the npm side), and compiled extensions.
