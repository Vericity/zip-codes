require 'bundler/gem_tasks'
require 'yaml'

task :convert do
  codes = {}
  File.open('lib/data/US.txt').each_line do |line|
    array = line.split('\t')
    codes[array[1]] = {
      state_code: array[4],
      state_name: array[3],
      city:       array[2]
    }
  end

  File.open('lib/data/US.yml', 'w') do |file|
    file.write codes.to_yaml
  end
end

task :tag, [:repo] do |_, args|
  args.with_defaults repo: `git config --get remote.origin.url`.strip

  dir_path = Bundler.default_gemfile.dirname
  gemspec_path = Dir["#{dir_path}/*.gemspec"].first
  spec = Bundler.load_gemspec gemspec_path
  version = spec.version
  tag = "v#{version}"

  command = "git tag -m \"Tagging version #{version}\" #{tag}"
  command << ' && '
  command << "git push #{args.repo} #{tag}"

  sh command
end

