# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  if (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
    config.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=700,fmode=600"]
  else
    config.vm.synced_folder ".", "/vagrant"
  end
  config.vm.define "dockerhost" do |dock_config|
    dock_config.vm.box = "ubuntu/trusty64"
    dock_config.vm.hostname = "dockerhost"
    dock_config.vm.network "private_network", ip: "10.100.198.100"
    dock_config.vm.provision :shell, path: "scripts/bootstrap_ansible.sh"
    dock_config.vm.provision :shell, inline: "PYTHONUNBUFFERED=1 ansible-playbook /vagrant/ansible/docker.yml -c local"
    dock_config.vm.provider "virtualbox" do |v|
      v.memory = 2048
    end
  end
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end
  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = false
    config.vbguest.no_install = true
    config.vbguest.no_remote = true
  end
end
