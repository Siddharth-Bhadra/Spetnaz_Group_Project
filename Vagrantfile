Vagrant.configure("2") do |config|
    # Base image
    config.vm.box = "ubuntu/focal64"

    # Setting up private network
    config.vm.network "private_network", ip: "192.168.56.101"
    #config.vm.network "forwarded_port", guest: 9100, host: 1234 # Prometheus port
    #config.vm.network "private_network", type: "dhcp"


    # Allocating resources
    config.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
    end

    config.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt install -y wget
        wget https://github.com/prometheus/node_exporter/releases/download/v1.6.0/node_exporter-1.6.0.linux-amd64.tar.gz
        tar xvfz node_exporter-1.6.0.linux-amd64.tar.gz
        sudo mv node_exporter-1.6.0.linux-amd64/node_exporter /usr/local/bin/
        sudo useradd -rs /bin/false node_exporter
        rm -rf node_exporter-1.6.0.linux-amd64*
        nohup /usr/local/bin/node_exporter &

        # Create a systemd service for Node Exporter
        echo '[Unit]
        Description=Prometheus Node Exporter
        After=network.target

        User=node_exporter
        Group=node_exporter
        Type=simple
        ExecStart=/usr/local/bin/node_exporter

        [Install]
        WantedBy=multi-user.target' | sudo tee /etc/systemd/system/node_exporter.service

        # Start Node Exporter
        sudo systemctl daemon-reload
        sudo systemctl start node_exporter
        sudo systemctl enable node_exporter

    
        sudo apt-get install -y apache2
        sudo apt install nagios4 nagios-plugins nagios-nrpe-plugin -y
        sudo systemctl enable nagios
        sudo systemctl start nagios

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
