#!/usr/bin/env rake
# Rakefile
# @Author:      Brian Finney (bri@nfinney.com)
# @Website:     
# @License:     GPL (see http://www.gnu.org/licenses/gpl.txt)
# @Created:     2009-01-01
# @Original:    http://medevyoujane.com/blog/2008/8/21/erlang-make-rake-and-emake.html
# @Revision:    0.1
#
# = Description
# = Usage
# = TODO
# = CHANGES
require 'rake'
require 'rake/clean'

# Rakefile: 
# Configuration
START_MODULE = "CHANGE ME"
TEST_MODULE = "eunit"
TEST_FLAG = "EUNIT"
MNESIA_DIR = "/tmp"

# No Need to change
PWD = Dir.getwd()
INCLUDE = "include"
ERLC_FLAGS = "-I#{INCLUDE} +warn_unused_vars +warn_unused_import"

SRC = FileList['src/**/*.erl']
OBJ = SRC.pathmap("%{src,ebin}X.beam")
MODS = OBJ.map() do |obj|
    obj[/^(.*\/)?(.+?).beam$/,2]
end

CLEAN.include(['**/*.dump'])
CLOBBER.include(['**/*.beam'])

directory 'ebin'

rule ".beam" =>  ["%{ebin,src}X.erl"] do |t|
  sh "erlc -D #{TEST_FLAG} -pa ebin -W #{ERLC_FLAGS} -o ebin #{t.source}"
end

desc "Compile all"
task :compile => ['ebin'] + OBJ

desc "Open up a shell"
task :shell => [:compile] do
    sh("erl -sname #{START_MODULE} -pa #{PWD}/ebin")
end

desc "Open up a shell and run #{START_MODULE}:start()" 
task :run => [:compile] do
    sh("erl -sname #{START_MODULE} -pa #{PWD}/ebin -run #{START_MODULE} start")
end

desc "Run Unit Tests" 
task :test => [:compile] do
  puts "Testing Modules with #{TEST_MODULE}: #{MODS.join(', ')}\n"
  sh("erl -noshell -s #{TEST_MODULE} test #{MODS.join(' ')} -s init stop")
end

desc "Generate Documentation"
task :doc do
    sh("cd doc && erl -noshell -run edoc files ../#{SRC.join(" ../")} -run init stop")
end

task :default => :compile
