# -*- mode: ruby -*-
# vi: set ft=ruby :

# Virtual Lab Environment for Practice Only
# WARNING: This setup is for educational and practice purposes only
# Do NOT use in production environments

Vagrant.configure("2") do |config|

  # Ubuntu with GUI (Desktop Environment)
  config.vm.define "ubuntu-gui" do |ubuntu_gui|
    ubuntu_gui.vm.box = "ubuntu/jammy64"
    ubuntu_gui.vm.hostname = "ubuntu-gui-lab"
    ubuntu_gui.vm.network "private_network", ip: "192.168.56.10"

    ubuntu_gui.vm.provider "virtualbox" do |vb|
      vb.name = "Ubuntu-GUI-Lab"
      vb.memory = "2048"
      vb.cpus = 2
      vb.gui = true

      # Enable 3D acceleration and increase video memory
      vb.customize ["modifyvm", :id, "--accelerate3d", "on"]
      vb.customize ["modifyvm", :id, "--vram", "128"]
      vb.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
      vb.customize ["modifyvm", :id, "--draganddrop", "bidirectional"]
    end

    # Install desktop environment and essential tools
    ubuntu_gui.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get upgrade -y

      # Install Ubuntu Desktop (minimal)
      apt-get install -y ubuntu-desktop-minimal

      # Install additional useful tools
      apt-get install -y \
        curl \
        wget \
        git \
        vim \
        htop \
        net-tools \
        firefox \
        terminator

      # Enable automatic login for vagrant user
      sed -i 's/#  AutomaticLoginEnable = true/AutomaticLoginEnable = true/' /etc/gdm3/custom.conf
      sed -i 's/#  AutomaticLogin = user1/AutomaticLogin = vagrant/' /etc/gdm3/custom.conf

      # Start display manager
      systemctl enable gdm3
      systemctl set-default graphical.target

      echo "Ubuntu GUI setup completed!"
    SHELL
  end

  # Ubuntu CLI Only
  config.vm.define "ubuntu-cli" do |ubuntu_cli|
    ubuntu_cli.vm.box = "ubuntu/jammy64"
    ubuntu_cli.vm.hostname = "ubuntu-cli-lab"
    ubuntu_cli.vm.network "private_network", ip: "192.168.56.11"

    ubuntu_cli.vm.provider "virtualbox" do |vb|
      vb.name = "Ubuntu-CLI-Lab"
      vb.memory = "1024"
      vb.cpus = 1
      vb.gui = false
    end

    # Install essential CLI tools
    ubuntu_cli.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get upgrade -y

      # Install useful CLI tools
      apt-get install -y \
        curl \
        wget \
        git \
        vim \
        htop \
        tree \
        unzip \
        net-tools \
        build-essential \
        python3 \
        python3-pip \
        nodejs \
        npm

      echo "Ubuntu CLI setup completed!"
    SHELL
  end

  # RockyLinux CLI Only
  config.vm.define "rocky-cli" do |rocky_cli|
    rocky_cli.vm.box = "rockylinux/9"
    rocky_cli.vm.hostname = "rocky-cli-lab"
    rocky_cli.vm.network "private_network", ip: "192.168.56.12"

    rocky_cli.vm.provider "virtualbox" do |vb|
      vb.name = "RockyLinux-CLI-Lab"
      vb.memory = "1024"
      vb.cpus = 1
      vb.gui = false
    end

    # Install essential CLI tools
    rocky_cli.vm.provision "shell", inline: <<-SHELL
      dnf update -y

      # Install EPEL repository
      dnf install -y epel-release

      # Install useful CLI tools
      dnf install -y \
        curl \
        wget \
        git \
        vim \
        htop \
        tree \
        unzip \
        net-tools \
        gcc \
        gcc-c++ \
        make \
        python3 \
        python3-pip \
        nodejs \
        npm

      # Enable and start firewall
      systemctl enable firewalld
      systemctl start firewalld

      echo "RockyLinux CLI setup completed!"
    SHELL
  end

  # Global configurations
  config.vm.box_check_update = false

  # SSH configuration
  config.ssh.insert_key = false
  config.ssh.forward_agent = true

  # Sync folder configuration
  config.vm.synced_folder ".", "/vagrant", disabled: false
end
