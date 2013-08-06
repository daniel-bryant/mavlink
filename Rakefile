require 'rake/extensiontask'
require 'rubygems/package_task'

##
# Rake::ExtensionTask comes from the rake-compiler and understands how to
# build and cross-compile extensions.
#
# See https://github.com/luislavena/rake-compiler for details

Rake::ExtensionTask.new 'my_malloc' do |ext|

  # This causes the shared object to be placed in lib/my_malloc/my_malloc.so
  #
  # It allows lib/my_malloc.rb to load different versions if you ship a
  # precompiled extension that supports multiple ruby versions.

  ext.lib_dir = 'lib/my_malloc'
end

s = Gem::Specification.new 'my_malloc', '1.0' do |s|
  s.summary = 'my malloc wrapper'
  s.authors = %w[nobody@example]

  # this tells RubyGems to build an extension upon install

  s.extensions = %w[ext/my_malloc/extconf.rb]

  # naturally you must include the extension source in the gem

  s.files = %w[
    MIT-LICENSE
    Rakefile
    ext/my_malloc/extconf.rb
    ext/my_malloc/my_malloc.c
    lib/my_malloc.rb
  ]
end

# The package task builds the gem in pkg/my_malloc-1.0.gem so you can test
# installing it.

Gem::PackageTask.new s do end

# This isn't a good test, but does provide a sanity check

task test: %w[compile] do
  ruby '-Ilib', '-rmy_malloc', '-e', 'p MyMalloc.new(5).free'
end

task default: :test

