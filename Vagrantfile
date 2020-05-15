# -*- mode: ruby -*-
# vi: set ft=ruby :
#
# Vagrantfile for ansible-common-role

Vagrant.configure("2") do |config|
	# config.vm.network 'forwarded_port', guest: 80, host: 8080
	config.vm.synced_folder '.', '/vagrant', disabled: true
	config.ssh.insert_key = false
	config.vm.provider :virtualbox do |vb|
		#vb.gui = true
		vb.memory = '1024'
	end
	#
	# provision on all machines to allow ssh w/o checking
	#
	config.vm.provision "shell", inline: <<-SHELLALL
		echo "...disabling CheckHostIP..."
		sed -i.orig -e "s/#   CheckHostIP yes/CheckHostIP no/" /etc/ssh/ssh_config
# 		for i in /etc/sysconfig/network-scripts/ifcfg-eth1 /etc/sysconfig/network-scripts/ifcfg-enp0s8; do
# 			if [ -e ${i} ]; then echo "...displaying ${i}..."; cat ${i}; fi
# 		done
	SHELLALL


	config.vm.define "c6" do |c6|
		c6.vm.box = "centos/6"
		c6.ssh.insert_key = false
		c6.vm.network 'private_network', ip: '192.168.10.106'
		c6.vm.hostname = 'c6'
		c6.vm.provision "shell", inline: <<-SHELL
      yum install -y python libselinux-python
    SHELL

		c6.vm.provision "ansible" do |ansible|
			ansible.compatibility_mode = "2.0"
			ansible.playbook = "site.yml"
			ansible.inventory_path = "./inventory"
			# ansible.verbose = "v"
			# ansible.raw_arguments = [""]
		end
	end

	config.vm.define "c7" do |c7|
		c7.vm.box = "centos/7"
		c7.ssh.insert_key = false
		c7.vm.network 'private_network', ip: '192.168.10.107'
		c7.vm.hostname = 'c7'
		c7.vm.provision "shell", inline: <<-SHELL
      yum install -y python libselinux-python
    SHELL

		c7.vm.provision "ansible" do |ansible|
			ansible.compatibility_mode = "2.0"
			ansible.playbook = "site.yml"
			ansible.inventory_path = "./inventory"
		end
	end
	
	config.vm.define "c8" do |c8|
		c8.vm.box = "centos/8"
		c8.ssh.insert_key = false
		c8.vm.network 'private_network', ip: '192.168.10.108'
		c8.vm.hostname = 'c8'
    c8.vm.provision "shell", inline: <<-SHELL
      dnf install -y epel-release
      dnf makecache
      dnf install -y ansible
      alternatives --set python /usr/bin/python3
    SHELL

		c8.vm.provision "ansible" do |ansible|
			ansible.compatibility_mode = "2.0"
			ansible.playbook = "site.yml"
			ansible.inventory_path = "./inventory"
		end
	end

	config.vm.define "d9" do |d9|
		d9.vm.box = "debian/stretch64"
		d9.ssh.insert_key = false
		d9.vm.network 'private_network', ip: '192.168.10.109'
		d9.vm.hostname = 'd9'
		
		d9.vm.provision "ansible" do |ansible|
			ansible.compatibility_mode = "2.0"
			ansible.playbook = "site.yml"
			ansible.inventory_path = "./inventory"
		end
	end

	config.vm.define "d10" do |d10|
		d10.vm.box = "debian/buster64"
		d10.ssh.insert_key = false
		d10.vm.network 'private_network', ip: '192.168.10.110'
		d10.vm.hostname = 'd10'
		
		d10.vm.provision "ansible" do |ansible|
			ansible.compatibility_mode = "2.0"
			ansible.playbook = "site.yml"
			ansible.inventory_path = "./inventory"
		end
	end


	# fedora-21+vagrant doesn't set 2nd network config correctly
	config.vm.define "f21" do |f21|
		f21.vm.box = "bento/fedora-21"
		f21.ssh.insert_key = false
		f21.vm.network 'private_network', ip: '192.168.10.121'
		f21.vm.hostname = 'f21'

		f21.vm.provision "shell", inline: <<-SHELL
			echo "...fixing enp0s8..."
			sed -i -e "s/BOOTPROTO=none/BOOTPROTO=static/" /etc/sysconfig/network-scripts/ifcfg-enp0s8
			echo "...restarting network..."
			systemctl restart network
			sleep 5
			systemctl restart network
			ip addr
# 			for i in /etc/sysconfig/network-scripts/ifcfg-eth1 /etc/sysconfig/network-scripts/ifcfg-enp0s8; do
# 				if [ -e ${i} ]; then echo "...displaying ${i}..."; cat ${i}; fi
# 			done
			echo "...installing python2 (this may take a while)..."
			yum install -y python libselinux-python selinux-policy-default
		SHELL
		f21.vm.provision "ansible" do |ansible|
			ansible.compatibility_mode = "2.0"
			ansible.playbook = "site.yml"
			ansible.inventory_path = "./inventory"
		end
	end

	# these two versions are tested because 21 still used yum while 22 used dnf
	config.vm.define "f22" do |f22|
		f22.vm.box = "bento/fedora-22"
		f22.ssh.insert_key = false
		f22.vm.network 'private_network', ip: '192.168.10.122'
		f22.vm.hostname = 'f22'

		f22.vm.provision "ansible" do |ansible|
			ansible.compatibility_mode = "2.0"
			ansible.playbook = "site.yml"
			ansible.inventory_path = "./inventory"
		end
	end

	# virtualbox runs the older 5.0 on Fedora 23 and below so test that it works
	config.vm.define "f23" do |f23|
		f23.vm.box = "akanto/fedora-23-server"
		f23.ssh.insert_key = false
		f23.vm.network 'private_network', ip: '192.168.10.123'
		f23.vm.hostname = 'f23'

		f23.vm.provision "ansible" do |ansible|
			ansible.compatibility_mode = "2.0"
			ansible.playbook = "site.yml"
			ansible.inventory_path = "./inventory"
		end
	end

	config.vm.define "f29" do |f29|
		f29.vm.box = "generic/fedora29"
		f29.ssh.insert_key = false
		f29.vm.network 'private_network', ip: '192.168.10.129'
		f29.vm.hostname = 'f29'

		f29.vm.provision "ansible" do |ansible|
			ansible.compatibility_mode = "2.0"
			ansible.playbook = "site.yml"
			ansible.inventory_path = "./inventory"
		end
	end

	config.vm.define "f32" do |f32|
		f32.vm.box = "fedora/32-cloud-base"
		f32.ssh.insert_key = false
		f32.vm.network 'private_network', ip: '192.168.10.132'
		f32.vm.hostname = 'f32'
    f32.vm.provision "shell", inline: <<-SHELL
      echo "IGNORE THIS:  [WARNING]: Module invocation had junk after the JSON data:
AttributeError(\"module \'platform\' has no attribute \'dist\'\")
KeyError(\'ansible_os_family\')"
#       dnf install -y python
    SHELL

		# requires ansible_python_interpreter=/usr/bin/python3 in inventory
		f32.vm.provision "ansible" do |ansible|
			ansible.compatibility_mode = "2.0"
			ansible.playbook = "site.yml"
			ansible.inventory_path = "./inventory"
		end
	end


	config.vm.define "u12" do |u12|
		u12.vm.box = "ubuntu/precise64"
		u12.vm.network 'private_network', ip: '192.168.10.112'
		u12.vm.hostname = 'u12'
    u12.vm.provision "shell", inline: <<-SHELL
      apt-get -y install python
    SHELL

		u12.vm.provision "ansible" do |ansible|
			ansible.compatibility_mode = "2.0"
			ansible.playbook = "site.yml"
			ansible.inventory_path = "./inventory"
		end
	end

	config.vm.define "u14" do |u14|
		u14.vm.box = "ubuntu/trusty64"
		u14.vm.network 'private_network', ip: '192.168.10.114'
		u14.vm.hostname = 'u14'
    u14.vm.provision "shell", inline: <<-SHELL
      apt-get -y install python
    SHELL

		u14.vm.provision "ansible" do |ansible|
			ansible.compatibility_mode = "2.0"
			ansible.playbook = "site.yml"
			ansible.inventory_path = "./inventory"
		end
	end

	config.vm.define "u16" do |u16|
		u16.vm.box = "ubuntu/xenial64"
		u16.vm.network 'private_network', ip: '192.168.10.116'
		u16.vm.hostname = 'u16'
    u16.vm.provision "shell", inline: <<-SHELL
      apt-get -y install python
    SHELL

		u16.vm.provision "ansible" do |ansible|
			ansible.compatibility_mode = "2.0"
			ansible.playbook = "site.yml"
			ansible.inventory_path = "./inventory"
		end
	end

	config.vm.define "u18" do |u18|
		u18.vm.box = "ubuntu/bionic64"
		u18.vm.network 'private_network', ip: '192.168.10.118'
		u18.vm.hostname = 'u18'
    u18.vm.provision "shell", inline: <<-SHELL
      apt-get -y install python
    SHELL

		u18.vm.provision "ansible" do |ansible|
			ansible.compatibility_mode = "2.0"
			ansible.playbook = "site.yml"
			ansible.inventory_path = "./inventory"
		end
	end

  # https://www.reddit.com/r/Ubuntu/comments/ga187h/focal64_vagrant_box_issues/
	config.vm.define "u20" do |u20|
		u20.vm.box = "ubuntu/focal64"
		u20.vm.network 'private_network', ip: '192.168.10.120'
		u20.vm.hostname = 'u20'
    u20.vm.provision "shell", inline: <<-SHELL
      apt-get -y install python
      apt autoremove -y
    SHELL

		u20.vm.provision "ansible" do |ansible|
			ansible.compatibility_mode = "2.0"
			ansible.playbook = "site.yml"
			ansible.inventory_path = "./inventory"
		end
	end

end
