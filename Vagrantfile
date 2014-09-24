# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Set box
  config.vm.box = 'puppetlabs/ubuntu-12.04-64-puppet'

  # Set box memory to 1024
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "1024"]
  end

  # Always run apt-get update
  config.vm.provision "shell", inline: "apt-get update -y"

  # Install RubyGems so `Provider gem is not functional on this host` doesnt happen
  config.vm.provision "shell", inline: "apt-get install rubygems -y"

  # Create an extra shared folder for the puppet files folder
  config.vm.synced_folder "puppet/files", "/etc/puppet/files"

  # Puppet provisioning
  config.vm.provision :puppet,
    :options => ["--fileserverconfig=/vagrant/fileserver.conf", "--verbose"] do |puppet|
    #:options => ["--verbose"] do |puppet|
    puppet.manifests_path = "puppet/manifests"
    puppet.module_path    = "puppet/modules"
    puppet.manifest_file  = "site.pp"
  end

  # Sensu server
  config.vm.define :server do |conf|
    conf.vm.hostname = 'mon01.vagrant.local'
    conf.vm.network :private_network, ip: "192.168.50.4"
    conf.vm.network :forwarded_port, guest: 3000, host: 3000
  end

  # Sensu client
  config.vm.define :client do |conf|
    conf.vm.hostname = 'web01.vagrant.local'
    conf.vm.network :private_network, ip: "192.168.50.5"
  end

end
