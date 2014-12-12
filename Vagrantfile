# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

diskfile1 = './tmp/diskfile1.vdi'
diskfile2 = './tmp/diskfile2.vdi'
diskfile3 = './tmp/diskfile3.vdi'
diskfile4 = './tmp/diskfile4.vdi'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.define :client do |role|
    role.vm.box = "ubuntu-precise"
	role.vm.box_url = "http://files.vagrantup.com/precise64.box"
    role.vm.hostname = "client"
	
	role.vm.provider :virtualbox do |vb|
		#vb.gui = true
 
		unless File.exist?(diskfile1)
			vb.customize ['createhd', '--filename', diskfile1, '--size', 2048]
		end
		unless File.exist?(diskfile2)
			vb.customize ['createhd', '--filename', diskfile2, '--size', 2048]
		end
		unless File.exist?(diskfile3)
			vb.customize ['createhd', '--filename', diskfile3, '--size', 2048]
		end
		unless File.exist?(diskfile4)
			vb.customize ['createhd', '--filename', diskfile4, '--size', 2048]
		end
		vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--type', 'hdd', '--medium', diskfile1]	
		vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 2, '--type', 'hdd', '--medium', diskfile2]	
		vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 3, '--type', 'hdd', '--medium', diskfile3]	
		vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 4, '--type', 'hdd', '--medium', diskfile4]	
	end
	
	#role.vm.provision :shell, :inline => "sudo apt-get update && sudo apt-get install wget vim -y"

	# Provision BTRFS
	# http://www.howtoforge.com/a-beginners-guide-to-btrfs
	role.vm.provision :shell, :inline => "sudo apt-get install btrfs-tools -y"

	# ZFS
	# http://www.jamescoyle.net/how-to/478-create-a-zfs-volume-on-ubuntu
	role.vm.provision :shell, :inline => "sudo apt-get update && sudo apt-get install python-software-properties -y"
	role.vm.provision :shell, :inline => "sudo apt-add-repository --yes ppa:zfs-native/stable && sudo apt-get update && sudo apt-get install ubuntu-zfs -y"
	
  end
end
