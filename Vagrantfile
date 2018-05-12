require 'yaml'

Vagrant.configure("2") do |config|
   config.vm.define "frontend", autostart: false do |fe|
    fe.vm.box = "hashicorp/precise64"
    fe.vm.hostname = 'frontend'

    fe.vm.network :private_network, ip: "192.168.0.10"
    fe.vm.network "forwarded_port", guest: 80, host: 80
    fe.vm.provision "file", source: "~/load-balancer.conf", destination: "~/load-balancer.conf"

    fe.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 256]
      v.customize ["modifyvm", :id, "--name", "frontend"]
    end

    fe.vm.provision 'ansible' do |ansible|
       ansible.playbook = 'nginx.yml'
       ansible.host_key_checking = false
    end
  end

   config.vm.define "backend-1", autostart: false do |be1|
    be1.vm.box = "alejandrofcarrera/precise64-docker"
    be1.vm.hostname = 'backend-1'

    be1.vm.network :private_network, ip: "192.168.0.15"

    be1.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 256]
      v.customize ["modifyvm", :id, "--name", "backend-1"]
    end    

    be1.vm.synced_folder "/synced", "/sync", create: true

    be1.vm.provision 'ansible' do |ansible|
      ansible.playbook = 'docker-minio.yml'
      ansible.host_key_checking = false
    end
   end

   config.vm.define "backend-2", autostart: false do |be2|
    be2.vm.box = "alejandrofcarrera/precise64-docker"
    be2.vm.hostname = 'backend-2'

    be2.vm.network :private_network, ip: "192.168.0.20"

    be2.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 256]
      v.customize ["modifyvm", :id, "--name", "backend-2"]
    end
   
    be2.vm.synced_folder "/synced", "/sync", create: true
  
    be2.vm.provision 'ansible' do |ansible|
      ansible.playbook = 'docker-minio.yml'
      ansible.host_key_checking = false
    end
   end
end
