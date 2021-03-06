# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
Vagrant.require_version ">= 1.6.3"

$script = <<SCRIPT
  
  cd /vagrant;

  # Run logstash-forwarder
  docker run -i --rm -h `hostname` -v `pwd`/config/conf:/opt/conf-stdin \
                                   -v `pwd`/config/ssl:/etc/ssl \
                                   pasangsherpa/logstash-forwarder;

SCRIPT

# All Vagrant configuration is done below. The VAGRANTFILE_API_VERSION
# in Vagrant.configure configures the configuration version (we support
# older styles for backwards compatibility). Please don't change it unless
# you know what you're doing.
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Setup CentOS 6.6 box
  config.vm.box = "chef/centos-6.6"

  # Set hostname to host machine's hostname
  config.vm.hostname = "#{`hostname`[0..-2]}-vagrant"

  # This is a hack around the networking slowness in the VM.
  config.vm.provider :virtualbox do |vb|
    vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
    vb.customize ['modifyvm', :id, '--natdnsproxy1', 'on']
  end

  # Enable Docker provisioning
  config.vm.provision "docker"

  config.vm.provision "shell", inline: $script

end
