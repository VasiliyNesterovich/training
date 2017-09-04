Vagrant.configure(2) do |config|
	config.vm.box = "bertvv/centos72"
	config.vm.box_check_update = false
	
	config.vm.define "server1" do |server1|
		server1.vm.hostname = "server1"
		server1.vm.network "private_network", ip: "172.20.20.10"
		server1.vm.provider "virtualbox" do |vb|
  		  vb.gui = true
		  vb.memory = 300
		end
		server1.vm.provision "add ip and dns", type: "shell",
		inline: "echo '172.20.20.11 server2' >> /etc/hosts"
		server1.vm.provision "add ip and dns2", type: "shell",
		inline: "echo '172.20.20.12 server3' >> /etc/hosts"
		server1.vm.provision "firewall kill", type: "shell",
		inline: "systemctl stop firewalld"
		server1.vm.provision "tomcat install", type: "shell",
		inline: "yum install tomcat tomcat-webapps tomcat-admin-webapps -y"
		server1.vm.provision "tomcat enable", type: "shell",
		inline: "systemctl enable tomcat"
		server1.vm.provision "tomcat create folder", type: "shell",
		inline: "mkdir /usr/share/tomcat/webapps/test"
		server1.vm.provision "index.html", type: "shell",
		inline: "echo 'tomcat 111111111' >> /usr/share/tomcat/webapps/test/index.html"
		server1.vm.provision "tomcat start", type: "shell",
		inline: "systemctl start tomcat"
	end

	config.vm.define "server2" do |server2|
		server2.vm.hostname = "server2"
		server2.vm.network "private_network", ip: "172.20.20.11"
		server2.vm.provider "virtualbox" do |vb|
 			vb.gui = true
			vb.memory = 300  
	        end
		server2.vm.provision "add ip and dns", type: "shell",
		inline: "echo '172.20.20.10 server1' >> /etc/hosts"
		server2.vm.provision "add ip and dns2", type: "shell",
		inline: "echo '172.20.20.12 server3' >> /etc/hosts"
		server2.vm.provision "firewall kill", type: "shell",
		inline: "systemctl stop firewalld"
		server2.vm.provision "tomcat install", type: "shell",
		inline: "yum install tomcat tomcat-webapps tomcat-admin-webapps -y"
		server2.vm.provision "tomcat enable", type: "shell",
		inline: "systemctl enable tomcat"
		server2.vm.provision "tomcat create folder", type: "shell",
		inline: "mkdir /usr/share/tomcat/webapps/test"
		server2.vm.provision "index.html", type: "shell",
		inline: "echo 'tomcat 222222222' >> /usr/share/tomcat/webapps/test/index.html"
		server2.vm.provision "tomcat start", type: "shell",
		inline: "systemctl start tomcat"		
	end

	config.vm.define "server3" do |server3|
	        server3.vm.hostname = "server3"
		server3.vm.network "private_network", ip: "172.20.20.12"
		server3.vm.network  "forwarded_port", guest: 80, host: 18080
		server3.vm.provider "virtualbox" do |vb|
			vb.gui = true
			vb.memory = 300
		end
		server3.vm.provision "add ip and dns", type: "shell",
		inline: "echo -e '172.20.20.11 server2\n172.20.20.10 server1' >> /etc/hosts"
		server3.vm.provision "firewall kill", type: "shell",
		inline: "systemctl stop firewalld"
		server3.vm.provision "httpd install", type: "shell",
		inline: "yum install httpd -y"
		server3.vm.provision "copy connector", type: "shell",
		inline: "cp /vagrant/mod_jk.so /etc/httpd/modules/"
		server3.vm.provision "create workers.properties", type: "shell",
		inline:<<-JOPA2
		  echo -e 'worker.list=lb
		  worker.lb.type=lb
		  worker.lb.balance_workers=server1,server2
		  worker.server1.host=172.20.20.10
		  worker.server1.port=8009
		  worker.server1.type=ajp13
		  worker.server2.host=172.20.20.11
		  worker.server2.port=8009
		  worker.server2.type=ajp13' >> /etc/httpd/conf/workers.properties
		  JOPA2
		server3.vm.provision "edit httpd.conf", type: "shell",
		inline:<<-JOPA
		  echo -e 'LoadModule jk_module modules/mod_jk.so
		  JkWorkersFile conf/workers.properties
		  JkShmFile /tmp/shm
		  JkLogFile logs/mod_jk.log
		  JkLogLevel info
		  JkMount /test* lb' >> /etc/httpd/conf/httpd.conf
		  JOPA
		server3.vm.provision "httpd start", type: "shell",
		inline: "systemctl start httpd"
	end
end