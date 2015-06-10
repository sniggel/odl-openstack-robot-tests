# -*- mode: ruby -*-
# # vi: set ft=ruby :
 
# Specify minimum Vagrant version and Vagrant API version
Vagrant.require_version ">= 1.6.0"
VAGRANTFILE_API_VERSION = "2"

# Require YAML module
require 'yaml'
 
# Read YAML file with box details
servers = YAML.load_file('env.yaml')

# Create boxes
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
 
  # Iterate through entries in YAML file
  servers.each do |servers|
    config.vm.define servers["name"] do |srv|
      ## Extract yaml variables
      controller = servers["controller"]
      hostname = servers["name"]
      local_ip = servers["local_ip"]
      os = servers["box"]
      ram = servers["ram"]

      srv.vm.box = os
      srv.vm.network "public_network", ip: local_ip
      srv.vm.provider :virtualbox do |vb|
        vb.name = hostname
        vb.memory = ram
      end # vb

      # Set guest environment variables
      command1 = 'export CONTROLLER=\"' + controller + '\"'
      command2 = 'export LOCAL_IP=\"' + local_ip + '\"'
      srv.vm.provision :shell, privileged: true, inline: 'echo ' + command1 + ' >> /etc/profile'
      srv.vm.provision :shell, privileged: true, inline: 'echo ' + command2 + ' >> /etc/profile'
      srv.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"

      ## SSH config
      srv.ssh.forward_x11 = false

      ## puppet
      if hostname == "ovs1" or hostname == "ovs2"
      elsif hostname == "controller"
      elsif hostname == "robot"
      end

      srv.vm.provision "puppet" do |puppet|
        puppet.manifests_path = "puppet/manifests"
        puppet.manifest_file  = "site.pp"
        puppet.options = "--verbose --debug"
      end# puppet

    end # srv
  end # servers
end # config