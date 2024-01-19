# -*- mode: ruby -*-
# vi: set ft=ruby :

$etchosts = <<-SCRIPT
cp /etc/hosts /root && \
echo '\n192.168.56.210 ansible.devopstraining.com' >> /etc/hosts && \
echo '192.168.56.211 web.devopstraining.com m-rocky9' >> /etc/hosts
SCRIPT


Vagrant.configure("2") do |config|



  config.vm.define "web" do |web|
    web.vm.box = "bento/rockylinux-9"
    web.vm.provider "virtualbox" do |vb|
      vb.memory = "768"
    end
    web.vm.network "private_network", ip: "192.168.56.211"
    web.vm.synced_folder "./shared", "/shared"
#    web.vm.provision "shell", inline: $etchosts
    web.vm.provision "shell", inline: "sudo useradd -m -p 'GV1w3oLU5KyLw' homework"
    web.vm.provision "shell", inline: "sudo usermod -aG wheel homework"
    web.vm.provision "shell", inline: "sudo hostnamectl set-hostname web"
    web.vm.network "forwarded_port", guest:80, host:8888
  end




  config.vm.define "ansible" do |ansible|
    ansible.vm.box = "bento/rockylinux-9"
    ansible.vm.provider "virtualbox" do |vb|
      vb.memory = "4096"
    end
    ansible.vm.network "private_network", ip: "192.168.56.210"
    ansible.vm.hostname = "ansible"
    ansible.vm.synced_folder "./shared", "/shared"
    ansible.vm.provision "shell", inline: $etchosts
    ansible.vm.provision "shell", path: "provision/ansible.sh"
    ansible.vm.provision "shell", inline: "ssh-keyscan m-rocky9 >> ~/.ssh/known_hosts" #ssh host key check (fingerprint)
    ansible.vm.provision "shell", inline: "hostnamectl set-hostname ansible"
    ansible.vm.network "forwarded_port", guest:8043, host:8043
  end
end
