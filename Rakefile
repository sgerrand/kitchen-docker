require "bundler/gem_tasks"
require 'cane/rake_task'
require 'tailor/rake_task'

desc "Run cane to check quality metrics"
Cane::RakeTask.new do |cane|
  cane.canefile = './.cane'
end

Tailor::RakeTask.new

desc "Display LOC stats"
task :stats do
  puts "\n## Production Code Stats"
  sh "countloc -r lib"
end

desc "Run all quality tasks"
task :quality => [:cane, :tailor, :stats]

task :default => [:quality, 'integration:docker']

begin
  require 'kitchen/rake_tasks'
  Kitchen::RakeTasks.new
rescue LoadError
  puts ">>>>> Kitchen gem not loaded, omitting tasks" unless ENV['CI']
end

desc 'Run Test Kitchen integration tests'
namespace :integration do
  desc 'Run Test Kitchen integration tests using docker'
  task :docker do
    sh %(bundle exec kitchen test #{ENV['KITCHEN_ARGS']} #{ENV['KITCHEN_REGEXP']})
  end
end
