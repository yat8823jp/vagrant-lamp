# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "CentOS-56-x64-packages-puppet-2.6.10-chef-0.10.6"
  config.vm.box_url = "https://dl.dropbox.com/u/7196/vagrant/CentOS-56-x64-packages-puppet-2.6.10-chef-0.10.6.box"
  # config.vm.box = "centos"
  config.vm.hostname = "centos"
  # config.vm.box_url = "https://github.com/2creatives/vagrant-centos/releases/download/v6.5.1/centos65-x86_64-20131205.box"

  config.vm.network :forwarded_port, guest: 80, host: 8082
  config.vm.network :forwarded_port, guest: 3306, host: 8806

  config.vm.provision :chef_solo do |chef|
    chef.add_recipe     "apache2"
    chef.add_recipe     "apache2::mod_php5"
    chef.add_recipe     "php"
    chef.add_recipe     "vim"

    chef.json = {
      :apache => {
        :default_site_enabled => true
      }
    }
  end

  config.omnibus.chef_version = :latest
  config.berkshelf.enabled = true
  
end
