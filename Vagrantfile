disk_centos = 'c:\Users\Andrey\VirtualBox VMs\centos\extra_disk.vdi'

Vagrant.configure("2") do |config|
	config.vm.define "centos" do |centos|
		centos.vm.box = "centos/8"
		centos.vm.network "forwarded_port", id: "ssh", host: 2223, guest: 22
		centos.vm.network "private_network", ip: "10.11.10.1", virtualbox__intnet: true
		centos.vm.hostname = "centos"

		centos.vm.provision "shell" do |s|
			ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
			s.inline = <<-SHELL
			mkdir -p /root/.ssh
			echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
			echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
			SHELL
		end

		centos.vm.provider "virtualbox" do |v|
			v.name = "centos"
			v.memory = 2048
			v.cpus = 1
			unless File.exist?(disk_centos)
				v.customize ['createhd', '--filename', disk_centos, '--size', 20 * 1024]
			end
			v.customize ['storageattach', :id, '--storagectl', 'IDE', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', disk_centos]
		end
	end

	config.vm.define "ubuntu" do |ubuntu|
		ubuntu.vm.box = "ubuntu/focal64"
		ubuntu.vm.network "forwarded_port", id: "ssh", host: 2224, guest: 22
		ubuntu.vm.network "private_network", ip: "10.11.10.2", virtualbox__intnet: true
		ubuntu.vm.hostname = "ubuntu"

		ubuntu.vm.provision "shell" do |s|
			ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
			s.inline = <<-SHELL
			mkdir -p /root/.ssh
			echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
			echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
			SHELL
		end

		ubuntu.vm.provider "virtualbox" do |v|
			v.name = "ubuntu"
			v.memory = 2048
			v.cpus = 1
		end
	end
end
