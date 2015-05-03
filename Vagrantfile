# -*- mode: ruby -*-
# vi: set ft=ruby :

MACHINES = [
  {
    "id" => "test-centos01",
    "vm" => {
      "box"     => "centos6.5.3",
      "box_url" => "https://github.com/2creatives/vagrant-centos/releases/download/v6.5.3/centos65-x86_64-20140116.box"
    }
  }
]

ANSIBLE = {
  "groups" => {
    "test" => [ "test-centos01" ],
  }
}

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  require 'rbconfig'
  is_windows = (RbConfig::CONFIG['host_os'] =~ /mswin|mingw|cygwin/)
  if is_windows
    # Provisioning configuration for shell script.
    config.vm.provision "shell" do |sh|
      sh.path = "windows.sh"
      sh.args = "vagrant.yml"
    end
  else
    config.vm.provision :ansible do |ansible|
      ansible.sudo = true
      ansible.playbook = "vagrant.yml"
      ansible.verbose = "v"
      ansible.host_key_checking = false

      ansible.groups = ANSIBLE["groups"]
    end
  end

  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
    v.cpus = 2
  end
  
  MACHINES.each do |machine|
    config.vm.define machine["id"] do |node|
      node.vm.hostname = machine["id"]
      node.vm.box = machine["vm"]["box"]
      node.vm.box_url = machine["vm"]["box_url"]
      node.vm.network :private_network, type: "dhcp"
      node.vm.network "forwarded_port", guest: 8081, host: 8081, auto_correct: true
    end

  end

end
