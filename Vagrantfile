# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # config.vm.box = "CentOS-56-x64-packages-puppet-2.6.10-chef-0.10.6"
  # config.vm.box_url = "https://dl.dropbox.com/u/7196/vagrant/CentOS-56-x64-packages-puppet-2.6.10-chef-0.10.6.box"
  config.vm.box = "chef/centos-6.5"
  config.vm.hostname = "centos"
  # config.vm.box_url = "https://github.com/2creatives/vagrant-centos/releases/download/v6.5.1/centos65-x86_64-20131205.box"

  config.vm.network :forwarded_port, guest: 80, host: 8082
  config.vm.network :forwarded_port, guest: 443, host: 8443
  config.vm.synced_folder ".", "/var/www", :mount_options => ["dmode=777,fmode=666"]

  config.vm.provision :chef_solo do |chef|
    chef.log_level = :debug
    chef.cookbooks_path = ["chef/cookbooks", "chef/site-cookbooks"]

    chef.roles_path = "chef/roles"
    chef.data_bags_path = "chef/data_bags"

    chef.add_recipe     "apache2"
    chef.add_recipe     "apache2::mod_php5"
    chef.add_recipe     "apache2::mod_ssl"
    chef.add_recipe     "apache2::mod_rewrite"
    chef.add_recipe     "php"
    chef.add_recipe     "postgresql::client"

    chef.json = {
      :apache => {
        :version => "2.2",
        :default_site_enabled => true,
        :docroot_dir => "/var/www/ec-site/html",
        :listen_ports => [80, 443]
      },
      :php => {
        :version => "5.3",
        :directives => {
          :display_errors => 'On',
          "date.timezone" => "Asia/Tokyo",
        },
        :packages => ["php-mbstring", "php-pgsql", "php-pear"]
      }
    }
  end

  config.omnibus.chef_version = :latest
  config.berkshelf.enabled = true

end
