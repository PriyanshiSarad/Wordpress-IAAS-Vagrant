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
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/focal64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
   config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
   config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
   config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
     vb.memory = "2000"
   end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
   config.vm.provision "shell", inline: <<-SHELL
     sudo apt update && sudo apt upgrade -y 
	 sudo apt install apache2 \
					  php \
					  mysql-server \
					  libapache2-mod-php \
					  php-mysql \
					  php-bcmath \
					  php-curl \
					  php-mbstring \
					  php-imagick \
					  php-common \
					  php-intl \
					  php-json \
					  php-xml \
					  php-zip -y 
	 sudo systemctl enable apache2 
     
	 sudo chown -R www-data:www-data /etc/apache2
	 sudo chown -R www-data: /var/www/html
     curl https://wordpress.org/latest.tar.gz | sudo -u www-data tar zx -C /var/www/html	 
	 
	 sudo cp /vagrant/wordpress.conf /etc/apache2/sites-available/wordpress.conf
     sudo a2dissite 000-default.conf
     sudo a2ensite wordpress.conf
     sudo a2enmod rewrite
     sudo systemctl restart apache2
     
	 sudo mysql -u root -e 'create database wordpress;'
     sudo mysql -u root -e 'create user john@localhost identified by "john@12345";'
     sudo mysql -u root -e 'grant all on wordpress.* to john@localhost;'	 
	 sudo mysql -u root -e 'flush privileges;'
	 
	 sudo -u www-data cp /var/www/html/wordpress/wp-config-sample.php /var/www/html/wordpress/wp-config.php
	 sudo -u www-data sed -i 's/database_name_here/wordpress/' /var/www/html/wordpress/wp-config.php
     sudo -u www-data sed -i 's/username_here/john/' /var/www/html/wordpress/wp-config.php
     sudo -u www-data sed -i 's/password_here/john@12345/' /var/www/html/wordpress/wp-config.php
	 
	 sudo systemctl restart apache2
   SHELL
end
