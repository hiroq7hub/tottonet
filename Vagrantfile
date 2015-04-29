# -*- mode: ruby -*-
# vi: set ft=ruby :
 
# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"
 
SERVER_SCRIPT = <<EOF
  sudo yum update -y
  sudo yum install -y wget ntp openssl-devel
  sudo ntpdate ntp.nict.jp
  (cd /tmp && wget https://web-dl.packagecloud.io/chef/stable/packages/el/5/chef-server-core-12.0.1-1.x86_64.rpm)
  sudo rpm -Uvh /tmp/chef-server-core-12.0.1-1.x86_64.rpm
EOF
 
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vbguest.auto_update = false
  config.omnibus.chef_version = :latest

  config.vm.define :chef_server do |host|
    host.vm.box = 'centos64'
    host.vm.box_url = 'https://github.com/2creatives/vagrant-centos/releases/download/v6.4.2/centos64-x86_64-20140116.box'
    host.vm.hostname = 'chef-server'
    host.vm.network :private_network, ip: '192.168.33.12'
    host.vm.provision :shell, :inline => SERVER_SCRIPT
  end
 
 config.vm.define :chef_client do |host|
    host.vm.box = 'centos65'
    host.vm.box_url = 'https://github.com/2creatives/vagrant-centos/releases/download/v6.4.2/centos64-x86_64-20140116.box'
    host.vm.hostname = 'chef-client'
    host.vm.network :private_network, ip: '192.168.33.13'
    host.vm.provision 'chef-client' do |chef|
      chef.chef_server_url = "http://192.168.33.12"
      chef.validation_key_path = "validation.pem"
    end
  end
end
