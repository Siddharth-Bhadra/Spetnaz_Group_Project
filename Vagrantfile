Vagrant.configure("2") do |config|
    # Base image
    config.vm.box = "ubuntu/focal64"

    # Setting up private network
    config.vm.network "private_network", ip: "192.168.56.101"

    # Allocating resources
    config.vm.provider "virtualbox" do |vb|
        vb.memory = "2048"
        vb.cpus = 2
    end

    config.vm.provision "shell", inline: <<-SHELL
        sudo apt update
        sudo apt install -y apache2
        sudo systemctl enable apache2
        sudo systemctl start apache2
    SHELL
end
