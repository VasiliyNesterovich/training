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
		server1.vm.network "private_network", ip: "172.20.20.10"
		config.vm.provision "add ip and dns", type: "shell",
		inline: "echo '172.20.20.11 server2' >> /etc/hosts"

	end

	config.vm.define "server2" do |server2|
		server2.vm.hostname = "server2"
		config.vm.provision "yum", type: "shell",
		inline: "yum install git -y"
		server2.vm.network "private_network", ip: "172.20.20.11"
		config.vm.provision "add ip and dns", type: "shell",
		inline: "echo '172.20.20.10 server1' >> /etc/hosts"
		config.vm.provision "clone", type: "shell",
		inline: "git clone https://github.com/VasiliyNesterovich/training.git"
		config.vm.provision "checkout", type: "shell",
		inline: "git checkout task1"
		config.vm.provision "cat", type: "shell",
		inline: "cat training/task1"
		#config.vm.provision "add", type: "shell",
		#inline: "git add Vagrantfile"
		#config.vm.provision "commit", type: "shell",
		#inline: "git commit -m 'Vagrantfile'"
		#config.vm.provision "ping", type: "shell",
		#inline: "ping -c 3 server1"

	end
end
