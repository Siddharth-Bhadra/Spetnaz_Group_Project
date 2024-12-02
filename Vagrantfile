Vagrant.configure("2") do |config|
    # Base image
    config.vm.box = "ubuntu/focal64"

    # Setting up private network
    config.vm.network "private_network", ip: "192.168.56.101"

    # Allocating resources
    config.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
    end

    config.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2

        # Installing rsyslog for logging
        sudo apt-get install -y rsyslog
        # Enabling and starting rsyslog service
        sudo systemctl enable rsyslog
        sudo systemctl start rsyslog

        # configuring UFW for Apache
        yes | sudo apt-get install -y ufw
        echo "y" | sudo ufw allow 80/tcp   # Allow HTTP traffic
        echo "y" | sudo ufw allow 443/tcp  # Allow HTTPS traffic 
        echo "y" | sudo ufw allow 22/tcp  # Allow SSH access
        echo "y" | sudo ufw enable
        sudo systemctl enable apache2
        sudo systemctl start apache2

        # Install and configure Apache SSL
        sudo apt-get install -y openssl
        sudo a2enmod ssl
        sudo systemctl restart apache2

        # Export RNG file to prevent warnings
        export RANDFILE=/tmp/.rnd
      
        # Generate and configure self-signed SSL certificate
        sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
     	    -keyout /etc/ssl/private/apache-selfsigned.key \
     	    -out /etc/ssl/certs/apache-selfsigned.crt \
     	    -subj "/C=CA/ST=ON/L=Hamilton/O=Organization/OU=Department/CN=sidbhadra4@gmail.com"

        # Install Lynis
        sudo apt-get update
        sudo apt-get install -y lynis

        # Run Lynis Audit and Save Output
        sudo lynis audit system --quiet > /vagrant/lynis-web-audit.log

        # Extract Warnings and Suggestions
        grep "warning" /var/log/lynis-report.dat > /vagrant/lynis-web-warnings.log
        grep "suggestion" /var/log/lynis-report.dat > /vagrant/lynis-web-suggestions.log

     


    SHELL
end
