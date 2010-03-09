#--
# Copyright (c) 2010 Engine Yard, Inc.
# Copyright (c) 2007-2009 Sun Microsystems, Inc.
# This source code is available under the MIT license.
# See the file LICENSE.txt for details.
#++

require 'spec/rake/spectask'
require 'spec/rake/verify_rcov'

MANIFEST = FileList["History.txt", "Manifest.txt", "README.txt", "LICENSE.txt", "Rakefile",
  "*.erb", "*.rb", "bin/*", "lib/**/*", "spec/**/*.rb", "spec/sample/**/*.*"]

begin
  File.open("Manifest.txt", "w") {|f| MANIFEST.each {|n| f << "#{n}\n"} }
  require 'hoe'
  require File.dirname(__FILE__) + '/lib/warbler/version'
  hoe = Hoe.spec("warbler") do |p|
    p.version = Warbler::VERSION
    p.rubyforge_name = "caldersphere"
    p.url = "http://caldersphere.rubyforge.org/warbler"
    p.author = "Nick Sieger"
    p.email = "nick@nicksieger.com"
    p.summary = "Warbler chirpily constructs .war files of your Rails applications."
    p.changes = p.paragraphs_of('History.txt', 0..1).join("\n\n")
    p.description = p.paragraphs_of('README.txt', 1...2).join("\n\n")
    p.extra_deps += [['rake', '>= 0.8.7'], ['jruby-jars', '>= 1.4.0'], ['jruby-rack', '>= 0.9.7'], ['rubyzip', '>= 0.9.4']]
    p.test_globs = ["spec/**/*_spec.rb"]
    p.rspec_options = ["--options", "spec/spec.opts"]
    p.clean_globs += ['spec/sample/MANIFEST*', 'spec/sample/web.xml*']
  end
  hoe.spec.files = MANIFEST
  hoe.spec.dependencies.delete_if { |dep| dep.name == "hoe" }

  task :gemspec do
    File.open("#{hoe.name}.gemspec", "w") {|f| f << hoe.spec.to_ruby }
  end
  task :package => :gemspec
rescue LoadError
  puts "You really need Hoe installed to be able to package this gem"
end

Rake::Task['rcov'].instance_variable_set("@prerequisites", [])
Rake::Task['rcov'].instance_variable_set("@actions", [])

Spec::Rake::SpecTask.new("spec:rcov") do |t|
  t.spec_opts ||= []
  t.spec_opts << "--options" << "spec/spec.opts"
  t.rcov = true
end

RCov::VerifyTask.new(:rcov => "spec:rcov") do |t|
  t.threshold = 100
end
