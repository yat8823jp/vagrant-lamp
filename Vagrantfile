# coding: utf-8
# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

$shell = <<SCRIPT
### optional #############################
## 以下のコメントを有効にすると, remote_db のコピーをローカルに作成する
sudo -u postgres createuser -d -S -R test_db_user
sudo -u postgres createdb -U test_db_user -E utf-8 test_db
#
# sudo -u vagrant echo "remote_host:5432:remote_db:remote_db_user:password" > .pgpass
# chown vagrant:vagrant .pgpass
# chmod 600 .pgpass
# sudo -u vagrant pg_dump -h remote_host -U remote_db_user remote_db | psql -U test_db_user test_db
### END optional #########################

echo 'Congratulations!!! Install Success. Please access http://ec.dev'
SCRIPT


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "nrel/CentOS-6.5-i386"
  #config.vm.hostname = "centos"

  #hostname 20150311
  config.vm.hostname = "ec.dev"

  # config.vm.box = "CentOS-56-x64-packages-puppet-2.6.10-chef-0.10.6"
  # config.vm.box_url = "https://dl.dropbox.com/u/7196/vagrant/CentOS-56-x64-packages-puppet-2.6.10-chef-0.10.6.box"
  # config.vm.box_url = "https://github.com/2creatives/vagrant-centos/releases/download/v6.5.1/centos65-x86_64-20131205.box"

  config.vm.network :forwarded_port, guest: 80, host: 8888
  config.vm.network :forwarded_port, guest: 443, host: 8443

  #hostname 20150311
  config.vm.network :private_network, ip: "192.168.33.250"

  config.vm.synced_folder ".", "/var/tmp/www", :mount_options => ["dmode=777,fmode=666"]

  config.vm.provision :chef_solo do |chef|
    chef.log_level = :debug
    chef.cookbooks_path = ["chef/cookbooks", "chef/site-cookbooks"]

    chef.roles_path = "chef/roles"
    chef.data_bags_path = "chef/data_bags"

    chef.add_recipe     "iptables::disabled"
    chef.add_recipe     "apache2"
    chef.add_recipe     "apache2::mod_php5"
    chef.add_recipe     "apache2::mod_ssl"
    chef.add_recipe     "apache2::mod_rewrite"
    chef.add_recipe     "php"
    chef.add_recipe     "postgresql::client"
    chef.add_recipe     "postgresql::server"

    chef.json = {
      :apache => {
        :version => "2.2",
        :default_site_enabled => true,
        :docroot_dir => "/var/tmp/www/ec-site/html",
        :listen_ports => [80, 443]
      },
      :php => {
        :version => "5.3",
        :directives => {
          :display_errors => 'On',
          "date.timezone" => "Asia/Tokyo",
        },
        :packages => ["php-mbstring", "php-pgsql", "php-pear", "php-xml", "php-gd", "php-devel"]
      },
      :postgresql => {
        :password => {
          postgres: 'password'
        },
        :pg_hba => [
          {:type => 'local', :db => 'all', :user => 'postgres', :addr => nil, :method => 'trust'},
          {:type => 'local', :db => 'all', :user => 'all', :addr => nil, :method => 'trust'},
          {:type => 'host', :db => 'all', :user => 'all', :addr => '127.0.0.1/32', :method => 'trust'},
          {:type => 'host', :db => 'all', :user => 'all', :addr => '::1/128', :method => 'trust'}
        ]
      }
    }
  end

  config.omnibus.chef_version = '11.12.4'
  # config.berkshelf.enabled = true

  config.vm.provision "shell", inline: $shell
end
