require 'rubygems'
require 'rake/testtask'
require 'rake/rdoctask'

require 'rspec/core'
require 'rspec/core/rake_task'
RSpec::Core::RakeTask.new(:spec)

$LOAD_PATH.unshift File.expand_path('../lib', __FILE__)
require 'typus/version'

desc 'Default: run unit tests.'
task :default => :test

desc 'Test the typus plugin.'
Rake::TestTask.new(:test) do |t|
  t.libs << 'lib'
  t.libs << 'test'
  t.pattern = 'test/**/*_test.rb'
  t.verbose = true
end

desc 'Generate plugin documentation.'
Rake::RDocTask.new(:rdoc) do |rdoc|
  rdoc.rdoc_dir = 'rdoc'
  rdoc.title    = 'Typus'
  rdoc.options << '--line-numbers' << '--inline-source'
  rdoc.rdoc_files.include('README.rdoc')
  rdoc.rdoc_files.include('lib/**/*.rb')
end

desc 'Build the gem.'
task :build do
  system "gem build typus.gemspec"
  system "gem install typus-#{Typus::VERSION}.gem"
end

desc 'Build and release the gem.'
task :release => :build do
  system "git commit -m 'Bump version to #{Typus::VERSION}' lib/typus/version.rb"
  system "git tag v#{Typus::VERSION}"
  system "git push && git push --tags"
  system "gem push typus-#{Typus::VERSION}.gem"
  system "git clean -fd && rm -f typus-#{Typus::VERSION}.gem"
  system "git branch -D 3-0-stable"
  system "git checkout -b 3-0-stable && git push -f"
  system "git checkout master"
end

desc 'Deploy'
task :deploy do
  system "cd test/fixtures/rails_app && cap deploy"
end
