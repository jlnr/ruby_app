# Ruby.app

Application wrapper to deploy Ruby apps on macOS using MRI/C Ruby, not MacRuby or JRuby.
So far, it has only been used to release [Ruby/Gosu](https://github.com/gosu/gosu) games.
It can probably be adapted to wrap applications or games written in other toolkits.

The idea is that `Ruby.app` contains a full, universal Ruby installation.
All you have to do is provide a `main.rb` file inside the `Ruby.app/Contents/Resources/` folder that starts your game/application.

# Alternatives

## Why not RubyMotion?

The short answer is that this project is much older than RubyMotion.
It is also free, and behaves the same as ‘MRI’ Ruby on the command-line.

## Why not use the macOS Ruby, or Platypus?

The first version of `Ruby.app` simply ran the user-supplied `main.rb` file using system Ruby (`/usr/bin/ruby`).
This is also what [Platypus](http://sveinbjorn.org/platypus) does. However,

* The Ruby that ships with macOS has been deprecated by Apple, and might disappear at any moment.
* Power users frequently mess around with system Ruby, at the very least they might install or remove libraries.
* Ruby/Gosu games tend to use some C extensions, and shipping these is extra painful.

# Build process

tl;dr after many long nights: I have given up on cross-compiling Ruby and its quirky C extensions.
Instead, I compile Ruby on two or three different Macs and merge the results with `lipo`.
Note: Since binaries compiled on one version of macOS are not guaranteed to work on older releases, I recommend using the oldest version of macOS you can find to build the Ruby app.

* Install rvm.
* Optional: Update the Rakefile with the desired Ruby version and gems.
* Run `rake` on a 64-bit Mac. This will install Ruby and all required gems via rvm, and then copy them into the `UniversalRuby` folder.
* Transfer this folder to a 32-bit Mac. Run `rake` on this Mac as well. It will merge the 32-bit binaries into the `UniversalRuby` folder.
* Optional: If you have updated Ruby, be sure to manually update `rbconfig.rb` from your rvm-built Ruby (at least the version number should match).
* You should now have a self-contained Ruby installation!

# License

Everything in this repository has been released under the MIT license. As for the Ruby installation that is contained in binary builds of the `Ruby.app`, please see the licenses for Ruby and its standard library.
