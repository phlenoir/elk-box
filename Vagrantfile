# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "centos/7"
  config.vm.hostname = "elk"
  
  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  config.vm.network "forwarded_port", guest: 80, host: 8081
  config.vm.network "forwarded_port", guest: 9200, host: 9201, protocol: "tcp", id: "elastic"
  config.vm.network "forwarded_port", guest: 5601, host: 5602, protocol: "tcp", id: "kibana"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # log in with ssh -i .vagrant/machines/default/virtualbox/private_key vagrant@192.168.33.10
  config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"
  #  config.vm.synced_folder ".", "/vagrant", type: "virtualbox"
	
  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
   config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
     # Customize the amount of memory on the VM:
     vb.memory = "2048"
   end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision :shell, path: "install_kafka.sh"
   config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
		sudo yum makecache
		# update kernel to the latest version
		#sudo yum -y update
		# install kernel headers
		sudo yum -y install wget kernel-devel kernel-headers gcc make perl
		sudo yum install java-1.8.0-openjdk -y
		sudo yum install epel-release -y
		sudo yum install wget -y
		sudo yum install python -y
		sudo yum install git -y
		sudo yum install net-tools -y
		sudo yum install vim-enhanced -y

		# install ELK
		echo "Download and install the public signing key"
		sudo rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
		
		# declare the v6 repo
		sudo bash -c 'echo "[elastic-6.x]" > /etc/yum.repos.d/elastic.repo'
		sudo bash -c 'echo "name=Elastic repository for 6.x packages" >> /etc/yum.repos.d/elastic.repo'
		sudo bash -c 'echo "baseurl=https://artifacts.elastic.co/packages/6.x/yum" >> /etc/yum.repos.d/elastic.repo'
		sudo bash -c 'echo "gpgcheck=1" >> /etc/yum.repos.d/elastic.repo'
		sudo bash -c 'echo "gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch" >> /etc/yum.repos.d/elastic.repo'
		sudo bash -c 'echo "enabled=1" >> /etc/yum.repos.d/elastic.repo'
		sudo bash -c 'echo "autorefresh=1" >> /etc/yum.repos.d/elastic.repo'
		sudo bash -c 'echo "type=rpm-md" >> /etc/yum.repos.d/elastic.repo'

		sudo yum -y install elasticsearch logstash kibana filebeat
		
		echo "Start
		the Elasticsearch service and set it to start automatically on boot"
		sudo systemctl daemon-reload
		sudo systemctl enable elasticsearch
		sudo systemctl start elasticsearch

		echo "Start the Kibana service and set it to start automatically on boot"
		sudo systemctl enable kibana
		sudo systemctl start kibana

		echo "Start the Logstash service and set it to start automatically on boot"
		sudo systemctl restart logstash
		sudo systemctl enable logstash

		echo "Installation complete!"
   SHELL
end
