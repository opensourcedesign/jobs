require "rubygems"
require 'rake'
require 'yaml'
require 'time'
require 'open-uri'

SOURCE = "."
CONFIG = {
  'version' => "0.2.13",
  'includes' => File.join(SOURCE, "_includes"),
  'themes' => File.join(SOURCE, "_includes", "themes"),
  'layouts' => File.join(SOURCE, "_layouts"),
  'jobs' => File.join(SOURCE, "jobs"),
  'post_ext' => "markdown",
  'theme_package_version' => "0.1.0"
}

# Usage: rake job title="Title" org="Organization Name" role="Job Role" [date="2012-02-09"]
desc "Create a new job in #{CONFIG['jobs']}"
task :job do
  abort("rake aborted: '#{CONFIG['jobs']}' directory not found.") unless FileTest.directory?(CONFIG['jobs'])
  title = ENV['title'] || "Job Title \# Try to keep it to three words"
  org = ENV['org'] || "Organization Name"
  role = ENV['role'] || "Job Role \# Ex: UI Designer, UX Designer, Icon Designer"
  slug = "#{title} - #{org}".downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '')
  begin
    date = (ENV['date'] ? Time.parse(ENV['date']) : Time.now).strftime('%Y-%m-%d')
    date_time = (ENV['date'] ? Time.parse(ENV['date']) : Time.now).strftime('%Y-%m-%d-%H-%M')
  rescue Exception => e
    puts "Error - date format must be YYYY-MM-DD, please check you typed it correctly!"
    exit -1
  end
  cat = ENV['cat'] || ""

  filename = File.join(CONFIG['jobs'], "#{date}-#{slug}.#{CONFIG['post_ext']}")
  if File.exist?(filename)
    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
  end

  puts "Creating new post: #{filename}"
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: jobs"
    post.puts "date_posted: #{date_time}"
    post.puts "title: \"#{title.gsub(/-/,' ')}\""
    post.puts "role: \"#{role.gsub(/-/,' ')}\""
    post.puts "organization: \"#{org.gsub(/-/,' ')}\""
    post.puts "github: github-username"
    post.puts "contributing_md: \# (optional) A link to your ocntibuting guidelines"
    post.puts "contributors_md: \# (optional) A list of contributors who are reach-out-able"
    post.puts "url: \# <your-org-url>"
    post.puts "tags: \# Ex. interface design, branding, logo"
    post.puts "status: \# Ex. searching, hired"
    post.puts "rate: \# Ex. gratis, $60 hour, $1000 total, etc"
    post.puts "---"
    post.puts ""
    post.puts "Write the description of the job here...
"
  end
end # task :job
