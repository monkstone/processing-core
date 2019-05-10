
require 'erb'

desc 'Create jar Manifest'
task :create_manifest do
  manifest = ERB.new <<~MANIFEST
    Implementation-Title: core
    Implementation-Version: 4.0.0
    Class-Path: gluegen-rt.jar jog-all.jar
  MANIFEST
  File.open('MANIFEST.MF', 'w') do |f|
    f.puts(manifest.result(binding))
  end
end

task default: [:init, :compile, :install]

# depends on installed processing, with processing on path
desc 'Create Manifest and Copy Jars'
task init: :create_manifest do
  processing_root = File.dirname(`readlink -f $(which processing)`) # for Archlinux etc
  jar_dir = File.join(processing_root, 'core', 'library')
  opengl = Dir.entries(jar_dir).grep(/amd64|macosx-universal/)
  opengl.concat %w[jogl-all.jar gluegen-rt.jar]
  opengl.each do |gl|
    FileUtils.cp(File.join(jar_dir, gl), File.join('.', 'lib'))
  end
end

desc 'Install'
task :install do
  sh "mvn install"
end

desc 'Document'
task :javadoc do
  sh 'mvn javadoc:javadoc'
end

desc 'Compile'
task :compile do
  sh 'mvn package'
end
