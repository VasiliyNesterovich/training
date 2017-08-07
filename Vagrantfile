Vagrant.configure(2) do |config|
	config.vm.box = "bertvv/centos72"
	config.vm.box_check_update = false
	#config.vm.network  "forwarded_port", guest: 8080, host: 18080
	config.vm.provider "virtualbox" do |vb|
		vb.gui = true
		#vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
		#this option don't work
	end

	config.vm.define "server1" do |server1|
		server1.vm.hostname = "server1"
		#server1.vm.network "private_network", ip: "172.20.20.10"
		server1.vm.network "public_network"
		#ping between machines via DNS names works only in public network
		#
	end

	config.vm.define "server2" do |server2|
		server2.vm.hostname = "server2"
		config.vm.provision "yum", type: "shell",
		inline: "sudo yum install git -y"
		#server2.vm.network "private_network", ip: "172.20.20.11"
		server2.vm.network "public_network"
		config.vm.provision "clone", type: "shell",
		inline: "git clone https://github.com/VasiliyNesterovich/training.git"
		config.vm.provision "cat", type: "shell",
		inline: "cat task1"
		config.vm.provision "add", type: "shell",
		inline: "git add Vagrantfile"
		config.vm.provision "commit", type: "shell",
		inline: "git commit -m 'Vagrantfile'"
	end
end
