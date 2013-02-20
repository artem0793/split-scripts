desc "Split out Drupal projects."
task :split_projects do
  current_dir = File.dirname(File.expand_path(__FILE__))
  core_modules = Array.new

  # Get stale drupal git commit
  #
  # Pull repo changes
  #
  # Get current drupal git commit
  #
  # * Before recompiling each project, check whether changes between old and new.
  # * Example: git diff 9e49307 5a8a967 core/modules/options

  project_types = ['modules', 'themes', 'profiles']

  project_types.each do |project_type|
    core_project_path = "#{current_dir}/drupal/core/#{project_type}/*"
    Dir.glob(core_project_path).each do |path|
      core_projects << File.split(path).last
    end

    core_projects.each do |project_name|
      Dir.chdir("#{current_dir}/drupal") {
        system "git subtree split -P core/#{project_type}/#{project_name} -b fracture-#{project_name}"
      }
      require 'inifile'
      project_ini = IniFile.load("#{current_dir}/drupal/core/#{project_type}/#{project_name}/#{project_name}.info")
      project_path = "#{current_dir}/#{project_type}/#{project_name}"
      Dir::mkdir(project_path) unless File.exists?(project_path)
      Dir.chdir(project_path) {
        system "git init"
        system "git fetch ../../drupal fracture-#{project_name}"
        system "git checkout -b master FETCH_HEAD"
        system "hub create drupal-fracture/#{project_name} -d '#{project_ini['global']['description']}'"
        system "git push -u origin master"
        json = File.new("composer.json", "w")
        json.puts <<-EOF
{
    "name": "drupal-fracture/#{project_name}",
    "version": "8.0.0",
    "type": "drupal-#{project_type[0..-2]}",
    "require": {
        "composer/installers": "*"
    }
}
        EOF
        json.close
      }
    end
  end
end
