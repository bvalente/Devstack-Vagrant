# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

	
	config.vm.box = "ubuntu/xenial64"

	#criar uma interface para comunicar com o SO host
	config.vm.network "private_network", ip: "192.168.33.10"


	config.vm.provider "virtualbox" do |vb|

		#aumentar a ram e cpu 
		vb.memory = "6114"
		vb.cpus = 2
		
	end


	config.vm.provision "shell", inline: <<-SHELL
		
		#update do OS
		apt-get update
		apt-get install -y git
		apt-get install -y apache2
		
		#criar o utilizador stack
		useradd -s /bin/bash -d /opt/stack -m stack
		echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack

		#download do devstack
		git clone -b stable/pike https://git.openstack.org/openstack-dev/devstack /opt/stack/devstack
		
		#configurar o ficheiro local.conf
		echo "[[local|localrc]]\nADMIN_PASSWORD=secret\nDATABASE_PASSWORD=secret\nRABBIT_PASSWORD=secret\nSERVICE_PASSWORD=secret\nHOST_IP=192.168.33.10\n" | tee /opt/stack/devstack/local.conf
		
		#é necessário alterar as permissoes porque os ficheiros foram escritos com "root"
		chown -R stack:stack /opt/stack
		
		#correr o devstack com o utilizador stack
		su - stack -c '/opt/stack/devstack/stack.sh'
		
		
	SHELL
end
