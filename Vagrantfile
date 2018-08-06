# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

# Overwrite host locale in ssh session
ENV["LC_ALL"] = "en_US.UTF-8"

Vagrant.configure("2") do |config|

  # Stick with Ubuntu 16 LTS and the box version
  config.vm.box = "ubuntu/xenial64"
  config.vm.box_version = "20180731.0.0"

  # Node running CKAN project
  config.vm.define "ckan" do |ckan|
    ckan.vm.hostname = "ckan"
    ckan.vm.network "private_network", ip: "172.17.177.21"
    ckan.vm.network "forwarded_port", guest: 22, host: 2221, id: "ssh"
    ckan.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "playbook.yml"
      ansible.tags = ["common", "ckan"]
    end
  end

  # Node running Jenkins
  config.vm.define "jenkins" do |jenkins|
    jenkins.vm.hostname = "jenkins"
    jenkins.vm.network "private_network", ip: "172.17.177.22"
    jenkins.vm.network "forwarded_port", guest: 22, host: 2220, id: "ssh"
    jenkins.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "playbook.yml"
      ansible.tags = ["common", "jenkins"]
    end
 end
end
