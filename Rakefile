require 'json'
require 'yaml'

desc "change v1/api.yml v2/api.yml v3/api.yml to v1/api.json v2/api.json v3/api.json"
task :gen, [:version] do |t, args|
  puts "generating json from yml.."
  version = args[:version] || "v1"
  ["v1", "v2", "v3"].each do |version|
    File.open("#{version}/api.json", "w") do |f|
      f.write(YAML.load_file("#{version}/api.yml").to_json)
    end
  end
end

desc "copy vx/api.json to dist/vx"
task :cp do
  puts "copying api.json to dist"
  `cp v1/api.json dist_latest/v1/api.json`
  `cp v2/api.json dist_latest/v2/api.json`
  `cp v3/api.json dist_latest/v3/api.json`
end

desc "build static assets"
task build: [:gen, :cp]

desc "deploy to s3"
task :publish do
  puts "deploying to s3 ..."
  system 's3_website push --force'
end
