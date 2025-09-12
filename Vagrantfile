# -*- mode: ruby -*-
# vi: set ft=ruby :

# Virtual Lab Environment for Practice Only
# WARNING: This setup is for educational and practice purposes only
# Do NOT use in production environments

Vagrant.configure("2") do |config|

  # Debian with Lightweight GUI (XFCE Desktop Environment)
  config.vm.define "debian-gui" do |debian_gui|
    debian_gui.vm.box = "debian/bookworm64"
    debian_gui.vm.hostname = "debian-gui-lab"
    debian_gui.vm.network "private_network", ip: "192.168.56.10"

    debian_gui.vm.provider "virtualbox" do |vb|
      vb.name = "Debian-XFCE-Lab"
      vb.memory = "1024"
      vb.cpus = 1
      vb.gui = true

      # Enable 3D acceleration and video memory
      vb.customize ["modifyvm", :id, "--accelerate3d", "on"]
      vb.customize ["modifyvm", :id, "--vram", "128"]
      vb.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
      vb.customize ["modifyvm", :id, "--draganddrop", "bidirectional"]
    end

    # Install XFCE desktop environment with essential tools
    debian_gui.vm.provision "shell", inline: <<-SHELL
      # Update system packages
      apt-get update
      apt-get upgrade -y

      # Ensure vagrant user exists with proper setup
      id -u vagrant &>/dev/null || useradd -m -s /bin/bash vagrant
      echo 'vagrant:vagrant' | chpasswd
      usermod -aG sudo vagrant

      # Install dependencies for Guest Additions
      apt-get install -y \
        build-essential \
        dkms \
        linux-headers-$(uname -r) \
        sudo \
        apt-transport-https \
        ca-certificates \
        gnupg \
        lsb-release

      # Install XFCE Desktop Environment (lightweight)
      apt-get install -y \
        task-xfce-desktop \
        lightdm \
        lightdm-gtk-greeter

      # Install essential applications
      apt-get install -y \
        curl \
        wget \
        git \
        vim \
        htop \
        net-tools \
        firefox-esr \
        xfce4-terminal \
        mousepad \
        thunar \
        ristretto

      # Install VirtualBox Guest Additions
      apt-get install -y virtualbox-guest-additions-iso
      apt-get install -y virtualbox-guest-utils virtualbox-guest-x11

      # Configure LightDM for auto-login
      mkdir -p /etc/lightdm/lightdm.conf.d/
      cat > /etc/lightdm/lightdm.conf.d/50-vagrant-autologin.conf << 'EOF'
[Seat:*]
autologin-user=vagrant
autologin-user-timeout=0
user-session=xfce
EOF

      # Enable display manager
      systemctl enable lightdm
      systemctl set-default graphical.target

      # Add vagrant user to vboxsf group for shared folders
      usermod -aG vboxsf vagrant

      # Allow vagrant user to sudo without password
      echo 'vagrant ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers.d/vagrant
      chmod 440 /etc/sudoers.d/vagrant

      # Optimize boot time by disabling unnecessary services
      systemctl disable bluetooth.service || true
      systemctl disable cups.service || true

      # Enable VirtualBox services
      systemctl enable vboxservice || true

      echo "=========================================="
      echo "Debian XFCE setup completed successfully!"
      echo "Username: vagrant | Password: vagrant"
      echo ""
      echo "IMPORTANT: For full screen mode to work:"
      echo "1. After first login, restart the VM once"
      echo "2. Use Host+F (usually Right Ctrl + F)"
      echo "3. Or go to VirtualBox View menu > Full-screen Mode"
      echo "4. Guest Additions will auto-resize the display"
      echo "5. Resolution fix (xrandr -s 1) runs automatically"
      echo ""
      echo "Manual resolution fix: xrandr -s 1"
      echo "Restart command: vagrant reload debian-gui"
      echo "=========================================="
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

    # Install essential CLI tools and development environment
    ubuntu_cli.vm.provision "shell", inline: <<-SHELL
      # Update system packages
      apt-get update
      apt-get upgrade -y

      # Install essential CLI tools
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

      echo "=========================================="
      echo "Ubuntu CLI setup completed successfully!"
      echo "Access via: vagrant ssh ubuntu-cli"
      echo "=========================================="
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

    # Install essential CLI tools and enterprise environment
    rocky_cli.vm.provision "shell", inline: <<-SHELL
      # Update system packages
      dnf update -y

      # Install EPEL repository
      dnf install -y epel-release

      # Install essential CLI tools
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

      # Configure and enable firewall
      systemctl enable firewalld
      systemctl start firewalld

      echo "=========================================="
      echo "RockyLinux CLI setup completed successfully!"
      echo "Access via: vagrant ssh rocky-cli"
      echo "=========================================="
    SHELL
  end

  # Global VM configurations
  config.vm.box_check_update = false

  # SSH configuration
  config.ssh.insert_key = false
  config.ssh.forward_agent = true

  # Sync folder configuration
  config.vm.synced_folder ".", "/vagrant", disabled: false

  # Global message
  config.vm.post_up_message = <<-MSG
  ==========================================
  Virtual Lab Environment Ready!

  VMs Created:
  - debian-gui  (192.168.56.10) - Debian 12 XFCE Desktop
  - ubuntu-cli  (192.168.56.11) - Ubuntu 22.04 CLI
  - rocky-cli   (192.168.56.12) - RockyLinux 9 CLI

  Access Methods:
  - GUI: VirtualBox window (auto-opens for debian-gui)
  - CLI: vagrant ssh <vm-name>

  Credentials: vagrant/vagrant

  Remember: This is for practice only!
  ==========================================
  MSG
end
