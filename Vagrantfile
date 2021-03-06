# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  net_ip = "192.168.50"
  default_mem = "1024"
  default_cpu = 1

  minions = [
    ["ubuntu14", "#{net_ip}.12", default_mem, default_cpu, "ubuntu/trusty64" ],
    ["ubuntu16", "#{net_ip}.13", default_mem, default_cpu, "ubuntu/xenial64" ],
    ["centos6",  "#{net_ip}.14",  default_mem, default_cpu, "bento/centos-6.9" ],
    ["centos7",  "#{net_ip}.15",  default_mem, default_cpu, "bento/centos-7.4" ],
    ["debian7",  "#{net_ip}.16",  default_mem, default_cpu, "bento/debian-7.11" ],
    ["debian8",  "#{net_ip}.17",  default_mem, default_cpu, "bento/debian-8.8" ],
  ]

  master_os = "bento/centos-7.4"


  config.vm.define :master, primary: true do |master_config|
    master_config.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.cpus = default_cpu
      vb.name = "master"
    end
      master_config.vm.box = "#{master_os}"
      master_config.vm.host_name = 'saltmaster.local'
      master_config.vm.network "private_network", ip: "#{net_ip}.10"
      master_config.vm.synced_folder "salt/", "/srv/salt"
      master_config.vm.synced_folder "vagrant/pillar/", "/srv/pillar"

      master_config.vm.provision :salt do |salt|
        salt.master_config = "vagrant/etc/master"
        salt.install_type = "stable"
        salt.install_master = true
        salt.no_minion = true
        salt.verbose = true
        salt.colorize = true
        salt.bootstrap_options = "-P -c /tmp"
      end
    end

    minions.each do |vmname,ip,mem,cpu,os|
      config.vm.define "#{vmname}" do |minion_config|
        minion_config.vm.provider "virtualbox" do |vb|
          vb.memory = "#{mem}"
          vb.cpus = 1
          vb.name = "#{vmname}"
        end
        minion_config.vm.box = "#{os}"
        minion_config.vm.hostname = "#{vmname}.local"
        minion_config.vm.network "private_network", ip: "#{ip}"

        minion_config.vm.provision :salt do |salt|
          salt.minion_config = "vagrant/etc/#{vmname}"
          salt.install_type = "stable"
          salt.verbose = true
          salt.colorize = true
          salt.bootstrap_options = "-P -c /tmp"
        end
      end
    end
  end