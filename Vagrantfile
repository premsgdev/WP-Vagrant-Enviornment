# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

      require 'yaml'
      settings = YAML.load_file 'env.yml'

      #Ubuntu VM
      #config.vm.box = "geerlingguy/ubuntu1604"
      config.vm.box = "geerlingguy/ubuntu1604"
      config.vm.box_version = "1.1.7"
      config.vm.network "private_network", ip: "192.168.1.11"

      config.vm.provider "virtualbox" do |v|
          v.name = "EQS-DEV-WP"
          v.memory = 2096
          v.cpus = 2
          #Solving slow download speeds in Vagrant/VirtualBox
          v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
          v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
          #Enabling multiple cores in Vagrant/VirtualBox
          v.customize ["modifyvm", :id, "--ioapic", "on"]
      end

      # Mount the current directory for configuration
      config.vm.synced_folder ".", "/vagrant/environment"

      # Mount the projects directory.
      # Preferred way: set an environment variable named 'EQS_VAGRANT_SERVICES_DIR' pointing to your local services directory.
      # Alternative way: replace <your services root dir> with your local services directory (will be overwritten when updating)..
      services_dir = settings['projectPath']
      config.vm.synced_folder  services_dir, "/var/www/html/"+settings['domain'], :nfs => { :mount_options => ["dmode=777","fmode=777"], :nfs_version => "3" }

      config.vm.provision "ansible_local" do |ansible|
        ansible.compatibility_mode = "2.0"
        ansible.playbook = "/vagrant/environment/ansible/master.yml"
      end
end
