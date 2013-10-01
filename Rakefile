#!/usr/bin/env ruby
require 'haml'

UPSTREAM_URI = "kevin@vps.compbox.org:/var/www/webdev/"
OUTPUT_DIR = "out"
task :default => [:clean,:build]
task :build => [OUTPUT_DIR,:convert_haml]
task :update => [:build,:upload_upstream]

task :convert_haml do
	FileList['./src/*.haml'].each do |fin|
		destination = OUTPUT_DIR + '/' + File.basename(fin).gsub(/\.haml$/, '.html')
		File.open destination, "w" do |fout|
			fout << Haml::Engine.new(File.read(fin)).render
		end
	end
end

task :clean do
	rm_rf OUTPUT_DIR
end

file OUTPUT_DIR do
	directory "out"
	Dir["public"].each do |f|
		cp_r f, OUTPUT_DIR
	end
end

task :upload_upstream do
	system "rsync --verbose --recursive --delete #{OUTPUT_DIR}/ #{UPSTREAM_URI}"
end
