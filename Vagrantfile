# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.define "debian-gui" do |debian_gui|
    debian_gui.vm.box = "debian/bookworm64"
    debian_gui.vm.hostname = "debian-gui-lab"
    debian_gui.vm.network "private_network", ip: "192.168.56.10"

    debian_gui.vm.provider "virtualbox" do |vb|
      vb.name = "Debian-GUI-Lab"
      vb.memory = "1024"
      vb.cpus = 1
      vb.gui = true
      vb.customize ["modifyvm", :id, "--accelerate3d", "on"]
      vb.customize ["modifyvm", :id, "--vram", "128"]
      vb.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
      vb.customize ["modifyvm", :id, "--draganddrop", "bidirectional"]
    end

    debian_gui.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get upgrade -y
      
      # Ensure vagrant user exists and set password
      id -u vagrant &>/dev/null || useradd -m -s /bin/bash vagrant
      echo 'vagrant:vagrant' | chpasswd
      usermod -aG sudo vagrant
      echo 'vagrant ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers.d/vagrant
      chmod 440 /etc/sudoers.d/vagrant
      
      apt-get install -y task-xfce-desktop lightdm lightdm-gtk-greeter
      mkdir -p /etc/lightdm/lightdm.conf.d/
      cat > /etc/lightdm/lightdm.conf.d/50-vagrant-autologin.conf << 'EOF'
[Seat:*]
autologin-user=vagrant
autologin-user-timeout=0
user-session=xfce
EOF
      systemctl enable lightdm
      systemctl set-default graphical.target
    SHELL
  end

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
  end

  config.vm.box_check_update = false
  config.ssh.insert_key = false
  config.ssh.forward_agent = true
  config.vm.synced_folder ".", "/vagrant", disabled: false
end