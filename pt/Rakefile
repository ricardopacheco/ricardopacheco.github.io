require "rubygems"
require "tmpdir"
require "bundler/setup"
require "jekyll"

# Your github username with repository name
GITHUB_REPONAME = "ricardopacheco/ricardopacheco.github.io"

desc "Generate static site"
task :generate do
  Jekyll::Site.new(Jekyll.configuration({
    "source"      => ".",
    "destination" => "_site"
  })).process
end

desc "Generate static site and publish on github"
task publish: %I[generate] do
  Dir.mktmpdir do |tmp|
    cp_r "_site/.", tmp

    pwd = Dir.pwd
    Dir.chdir tmp
    File.open(".nojekyll", "wb") { |f| f.puts("Site generated locally.") }

    system "git init"
    system "git add ."
    message = "Page updated on #{Time.now.utc}"
    system "git commit -m #{message.inspect}"
    system "git remote add origin git@github.com:#{GITHUB_REPONAME}.git"
    system "git push --force origin main"

    Dir.chdir pwd
  end
end
