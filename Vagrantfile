# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "chef/centos-7.1"
  config.puppet_install.puppet_version = '3.7.5'

  config.vm.provision "shell",
    inline: <<-EOS
      yum install -y ruby-devel rubygem-bundler git rubygem-rake
      gem install --no-rdoc --no-ri trollop -v '~> 2.0'
      gem install --no-rdoc --no-ri highline -v '~> 1.6.19'
      git clone https://github.com/jhoblitt/hiera-eyaml.git -b maint/fix-pkcs-plugin-option-defaults
      cd hiera-eyaml
      rake build
      gem install --local --no-rdoc --no-ri pkg/hiera-eyaml-2.0.8.gem
    EOS

  config.vm.provision :puppet do |puppet|
    puppet.hiera_config_path = "hiera.yaml"
    puppet.temp_dir = "/tmp/vagrant-puppet-1"
    puppet.working_directory = "/tmp/vagrant-puppet-1"
    config.vm.synced_folder "hiera/", "/tmp/vagrant-puppet-1/hiera"
    config.vm.synced_folder "keys/", "/tmp/vagrant-puppet-1/keys"
  end
end
