# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'
machines = YAML.load_file('machines.yml')
user= ENV['USER']
default= machines.delete("defaults")
File.write('hosts', machines.map{|e|"#{e[0]}.local"}.join("\n"))


Vagrant.configure("2") do |config|
  config.vm.provision "shell", inline: "apt install -y avahi-daemon libnss-mdns"
  config.vm.provision "shell", inline: "sudo useradd #{user} -m -s /bin/bash|| echo 'user already created'"
  config.vm.provision "shell", inline: "sudo mkdir -p /home/#{user}/.ssh/"
  config.vm.provision "shell", inline: "sudo chown -R vagrant /home/#{user}"
  config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "/home/#{user}/.ssh/authorized_keys" 
  config.vm.provision "shell", inline: "sudo chown -R #{user}:#{user} /home/#{user}"
 
 
  config.vm.provision "shell", inline: "echo #{user} ALL=NOPASSWD:ALL > /etc/sudoers.d/#{user}"
  config.vm.provision "shell", inline: "chmod 0440 /etc/sudoers.d/#{user}"
  config.vm.provision "shell", inline: "usermod -a -G sudo #{user}"



  if ARGV[0] == "ssh"
    config.ssh.username = user
  config.ssh.private_key_path = ['~/.ssh/id_rsa'] 

  end
  config.vm.provider "virtualbox" do |vm_config, override|
    vm_config.customize [
      "modifyvm", :id,
      "--natdnshostresolver1", "on",
    ]
  end


  machines.each.with_index{|vm,i|
  name= vm[0]
  image=  vm[1] ? vm[1]["image"] ? vm[1]["image"] : default["image"]: default["image"]
  memory=  vm[1] ? vm[1]["memory"] ? vm[1]["memory"] : default["memory"]: default["memory"]
  cpus=  vm[1] ? vm[1]["cpus"] ? vm[1]["cpus"] : default["cpus"]: default["cpus"]

  config.vm.define name do |minio|
    minio.vm.box =  image
  minio.vm.network "private_network", ip: "192.168.33.1#{i}"
  minio.vm.hostname = "#{name}.local"
  # minio.dns.tld = "minio#{e}.mysite.test"

    minio.vm.provider "virtualbox" do |vb|
    vb.name = name
    vb.gui = false
    vb.memory = memory
    vb.cpus = cpus

  end
  end
}

end
