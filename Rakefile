$LOAD_PATH.unshift "./lib"

src_dir = 'src/main/java'
build_dir = 'target'
directory build_dir

# Plain, free-form command-line
namespace :plain do
  desc "Compile the code"
  task :compile => build_dir do
    files = FileList["src/**/*.java"]
    cpath =  FileList["lib/*.jar"].join(':')
    sh "javac -d #{build_dir} -classpath #{cpath} #{files}"
  end

  desc "Create a jar file of the compiled code"
  task :jar => :compile do
    sh "jar cf greeter.jar -C #{build_dir} ."
  end
end

# Ant-based tasks
require 'ant'
namespace :ant do
  desc "Compile the code using Ant"
  ant_task :compile => build_dir do
    puts "Compiling java from #{src_dir} to #{build_dir}"
    javac :srcdir => src_dir, :destdir => build_dir do
      classpath do
        FileList["lib/*.jar"].each do |jar|
          pathelement :path => jar
        end
      end
    end
  end

  desc "Create a jar file of the compiled code using Ant"
  ant_task :jar => "ant:compile" do
    puts "Creating greeter.jar"
    jar :destfile => "greeter.jar", :basedir => build_dir
  end
end

# RSpec
require 'spec/rake/spectask'
spec_dir = 'src/spec/ruby'
desc "Run RSpec on the project"
Spec::Rake::SpecTask.new do |t|
  t.spec_files = FileList["#{spec_dir}/**/*_spec.rb"]
end

# Cucumber
require 'cucumber/rake/task'
$CLASSPATH << build_dir

desc "Run Cucumber on the project"
Cucumber::Rake::Task.new(:features => "ant:compile")

task :default => :features

# Clean
require 'rake/clean'
CLEAN << build_dir << "greeter.jar"
