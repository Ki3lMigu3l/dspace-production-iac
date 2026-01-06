# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box= "ubuntu/jammy64"
  config.vm.hostname= "dspace-prod"

  config.vm.provider "virtualbox" do |vb|
    vb.name = "dspace-production-iac"
    vb.memory = "8192"
    vb.cpus = 4

    disk = File.expand_path("dspace-disk.vdi")
    
    unless File.exist?("dspace-disk.vdi")
      vb.customize [
        'createhd',
        '--filename', 'dspace-disk.vdi',
        '--size', 51200,
        '--variant', 'Standard'
      ]
      vb.customize [
        'storageattach', :id,
        '--storagectl', 'SATA Controller',
        '--port', '1',
        '--device', '0',
        '--type', 'hdd',
        '--medium', disk
      ]
    end
  end

  config.vm.network "private_network", ip: "192.168.56.10", netmask: "255.255.255.0"
  config.vm.network "forwarded_port", guest: 80, host: 80  
  config.vm.network "forwarded_port", guest: 4000, host: 4000  
  config.vm.network "forwarded_port", guest: 8080, host: 8080  
  config.vm.network "forwarded_port", guest: 8983, host: 8983 

  config.vm.provision "shell", inline: <<-SHELL
    echo "vagrant ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/vagrant
    chmod 440 /etc/sudoers.d/vagrant
    echo "SSH pronta para conexão com Ansible"
    echo "================================================"
    echo "DSpace 9 Ubuntu 22.04"
    echo ""
    echo "Operating System: Ubuntu 22.04"
    echo "User: vagrant"
    echo "Hostname: $(hostname)"
    echo "IP address: 192.168.56.10"
    echo "================================================"
  SHELL

  config.vm.synced_folder ".", "/vagrant", disabled: true
end
