desc "Split out Drupal projects."
task :split_projects do
  current_dir = File.dirname(File.expand_path(__FILE__))
  core_modules = Array.new

  project_types = ['modules', 'themes']

  core_modules_path = "#{current_dir}/drupal/core/modules/*"
  Dir.glob(core_modules_path).each do |path|
    core_modules << File.split(path).last
  end

  core_modules.each do |module_name|
    Dir.chdir("#{current_dir}/drupal") {
      system "git subtree split -P core/modules/#{module_name} -b fracture-#{module_name}"
    }
    require 'inifile'
    module_ini = IniFile.load("#{current_dir}/drupal/core/modules/#{module_name}/#{module_name}.info")
    module_path = "#{current_dir}/modules/#{module_name}"
    Dir::mkdir(module_path) unless File.exists?(module_path)
    Dir.chdir(module_path) {
      system "git init"
      system "git fetch ../../drupal fracture-#{module_name}"
      system "git checkout -b master FETCH_HEAD"
      system "hub create drupal-fracture/#{module_name} -d '#{ini['global']['description']}'"
      system "git push -u origin master"
      json = File.new("composer.json", "w")
      json.puts <<-EOF
{
    "name": "drupal-fracture/#{module_name}",
    "version": "8.0.0",
    "type": "drupal-module",
    "require": {
        "composer/installers": "*"
    }
}
      EOF
      json.close
    }
  end
end
