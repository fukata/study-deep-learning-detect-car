# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.synced_folder ".",          "/vagrant"

  config.vm.define "dev" do |c|
    c.vm.box = "ubuntu/xenial64"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "site.yml"
    ansible.sudo = true
    ansible.tags = ENV['ANSIBLE_TAGS']
    ansible.skip_tags = ENV['ANSIBLE_SKIP_TAGS']
    ansible.verbose = ENV['ANSIBLE_VERBOSE']
    ansible.groups = {
      "blog-servers" => %(dev),
      "development:children" => %w(worker-servers)
    }
  end

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--ioapic", "on"]
    vb.customize ["modifyvm", :id, "--memory", "2048"]
    vb.customize ["modifyvm", :id, "--cpus", "2"]
    # fix lazy when IPv6
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]

    # sync time host to guest
    vb.customize ["setextradata", :id, "VBoxInternal/Devices/VMMDev/0/Config/GetHostTimeDisabled", 0]
  end
end
