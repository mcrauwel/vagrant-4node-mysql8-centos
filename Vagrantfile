# -*- mode: ruby -*-
# vi: set ft=ruby :

# Configure for environment
RAM_ASSIGNED = if ENV['VAGRANT_VM_MEMORY_SIZE']!=nil then ENV['VAGRANT_VM_MEMORY_SIZE'] else 1024 end

# Number of VMs
NUM_VMS = 4

# Basebox to use for environments (required for lab release)
VM_BOX = "centos/7"

# Customize to avoid network clashes of VMs
IP_BLOCK = "192.168.60."


Vagrant.configure("2") do |config|
  config.vm.boot_timeout = 600

  (1..NUM_VMS).each do |machine_id|
    config.vm.box_check_update = false
    config.vm.define "node#{machine_id}" do |node|
      node.vm.box = VM_BOX
      node.vm.provider :virtualbox do |vb|
          vb.customize ["modifyvm", :id, "--memory", RAM_ASSIGNED]
          vb.customize ["modifyvm", :id, "--cpus", "1"]
          vb.name = "node#{machine_id}"
      end
      node.vm.network "private_network", ip: "#{IP_BLOCK}#{100+machine_id}"
      node.vm.hostname = "node#{machine_id}"
    
      node.vm.provision "shell", inline: <<-SHELL
        for i in {1..#{NUM_VMS}}
        do
          let z=100+$i
          echo "#{IP_BLOCK}$z node$i" >> /etc/hosts
        done
        yum localinstall -y -q https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm  
	yum localinstall -y -q http://www.percona.com/downloads/percona-release/redhat/0.1-6/percona-release-0.1-6.noarch.rpm
      SHELL
    end
  end
end
