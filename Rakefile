#!/usr/bin/env rake
# Rakefile
# @Author:      Brian Finney (bri@nfinney.com)
# @Website:     
# @License:     GPL (see http://www.gnu.org/licenses/gpl.txt)
# @Created:     2009-01-01
# @Original:    http://21ccw.blogspot.com/2008/04/using-rake-for-erlang-unit-testing.html
# @Revision:    0.0
#
# = Description
# = Usage
# = TODO
# = CHANGES

require 'rake/clean'

# Rakefile: 
INCLUDE = "include"

ERLC_FLAGS = "-I#{INCLUDE} +warn_unused_vars +warn_unused_import"

SRC = FileList['src/*.erl']
OBJ = SRC.pathmap("%{src,ebin}X.beam")

CLEAN.include("ebin/*.beam")

directory 'ebin'

rule ".beam" => ["%{ebin,src}X.erl"] do |t|
  sh "erlc -D EUNIT -pa ebin -W #{ERLC_FLAGS} -o ebin #{t.source}"
end

task :compile => ['ebin'] + OBJ

task :default => :compile

task :test => [:compile] do
  puts "Modules under test:"
  OBJ.each do |obj|
    obj[%r{.*/(.*).beam}]
    mod = $1
    test_output = `erl -pa ebin -run #{mod} test -run init stop`

    if /\*failed\*/ =~ test_output
      test_output[/(Failed.*Aborted.*Skipped.*Succeeded.*$)/]
    else
      test_output[/1>\s*(.*)\n/]
    end

    puts "#{mod}: #{$1}"
  end
end
