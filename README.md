# Virtual Lab Environment with VirtualBox and Vagrant

## 📋 Lab Overview

This Vagrant setup creates three virtual machines for learning and practice:

1. **Ubuntu GUI** (ubuntu-gui) - Ubuntu 22.04 LTS with desktop environment
   - IP: 192.168.56.10
   - Memory: 2GB, CPU: 2 cores
   - Full desktop environment with GUI applications

2. **Ubuntu CLI** (ubuntu-cli) - Ubuntu 22.04 LTS command-line only
   - IP: 192.168.56.11  
   - Memory: 1GB, CPU: 1 core
   - Development tools and utilities pre-installed

3. **RockyLinux CLI** (rocky-cli) - RockyLinux 9 command-line only
   - IP: 192.168.56.12
   - Memory: 1GB, CPU: 1 core
   - Enterprise Linux environment for practice

## 📦 Pre-installed Software

### Ubuntu GUI (ubuntu-gui)
**Desktop Environment:**
- Ubuntu Desktop (minimal installation)
- GDM3 display manager with auto-login

**Applications & Tools:**
- **Web Browser**: Firefox
- **Terminal**: Terminator (advanced terminal emulator)
- **Development**: Git, Vim
- **System Tools**: curl, wget, htop, net-tools
- **File Management**: Built-in file manager
- **Text Editor**: Vim + GUI text editors

**Features:**
- 3D acceleration enabled
- Bidirectional clipboard and drag-and-drop
- 128MB video memory
- Auto-login for vagrant user

### Ubuntu CLI (ubuntu-cli)
**Development Tools:**
- **Languages**: Python3 + pip, Node.js + npm
- **Build Tools**: build-essential (gcc, make, etc.)
- **Version Control**: Git
- **Editor**: Vim

**System Utilities:**
- **Network**: curl, wget, net-tools
- **File Management**: tree, unzip
- **Monitoring**: htop
- **General**: Essential command-line utilities

### RockyLinux CLI (rocky-cli)
**Enterprise Tools:**
- **Repository**: EPEL (Extra Packages for Enterprise Linux)
- **Languages**: Python3 + pip, Node.js + npm
- **Build Tools**: gcc, gcc-c++, make
- **Version Control**: Git
- **Editor**: Vim

**System Utilities:**
- **Network**: curl, wget, net-tools
- **File Management**: tree, unzip
- **Monitoring**: htop
- **Security**: firewalld (enabled and configured)
- **General**: Enterprise Linux utilities

## 💻 System Requirements

### Minimum Requirements:
- **RAM**: 8GB (4GB+ available for VMs)
- **Storage**: 50GB free space
- **CPU**: Multi-core processor with virtualization support
- **OS**: Windows 10+, macOS 10.14+, or Linux

### Recommended:
- **RAM**: 16GB or more
- **Storage**: 100GB+ free space
- **CPU**: 4+ cores with VT-x/AMD-V enabled

## 🔧 Prerequisites Installation

### Step 1: Install VirtualBox

#### Windows:
1. Download VirtualBox from: https://www.virtualbox.org/wiki/Downloads
2. Download the Windows installer (.exe)
3. Run installer as administrator
4. Follow installation wizard (accept defaults)
5. Restart computer if prompted

#### macOS:
```bash
# Using Homebrew (recommended)
brew install --cask virtualbox

# Or download from website
# Visit: https://www.virtualbox.org/wiki/Downloads
# Download macOS installer (.dmg)
```

#### Linux (Ubuntu/Debian):
```bash
# Add VirtualBox repository
wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
echo "deb [arch=amd64] https://download.virtualbox.org/virtualbox/debian $(lsb_release -cs) contrib" | sudo tee /etc/apt/sources.list.d/virtualbox.list

# Install VirtualBox
sudo apt update
sudo apt install virtualbox-7.0
```

#### Linux (RHEL/CentOS/Fedora):
```bash
# Install from repositories
sudo dnf install VirtualBox

# Or download RPM from VirtualBox website
```

### Step 2: Install Vagrant

#### Windows:
1. Download Vagrant from: https://www.vagrantup.com/downloads
2. Download Windows installer (.msi)
3. Run installer as administrator
4. Restart command prompt/PowerShell

#### macOS:
```bash
# Using Homebrew (recommended)
brew install --cask vagrant

# Or download from website
```

#### Linux:
```bash
# Ubuntu/Debian
wget https://releases.hashicorp.com/vagrant/2.4.0/vagrant_2.4.0-1_amd64.deb
sudo dpkg -i vagrant_2.4.0-1_amd64.deb

# RHEL/CentOS/Fedora
sudo dnf install vagrant
```

### Step 3: Verify Installation

```bash
# Check VirtualBox
vboxmanage --version

# Check Vagrant
vagrant --version
```

## 🚀 Usage Instructions

### Initial Setup

1. **Clone or download this repository**
   ```bash
   git clone <repository-url>
   cd vagrant-lab
   ```

2. **Start all VMs**
   ```bash
   vagrant up
   ```

3. **Start individual VMs**
   ```bash
   # Ubuntu with GUI
   vagrant up ubuntu-gui
   
   # Ubuntu CLI only
   vagrant up ubuntu-cli
   
   # RockyLinux CLI only
   vagrant up rocky-cli
   ```

### Basic Vagrant Commands

```bash
# Check status of all VMs
vagrant status

# SSH into a specific VM
vagrant ssh ubuntu-cli
vagrant ssh rocky-cli

# Access Ubuntu GUI VM (will open VirtualBox window)
vagrant up ubuntu-gui

# Suspend VMs (saves state)
vagrant suspend

# Resume suspended VMs
vagrant resume

# Shutdown VMs gracefully
vagrant halt

# Restart VMs
vagrant reload

# Destroy VMs (deletes everything)
vagrant destroy

# Reprovision VMs (run setup scripts again)
vagrant provision
```

### Accessing the VMs

#### Ubuntu GUI:
- Will open in VirtualBox window automatically
- Login: vagrant / vagrant
- Auto-login should be enabled

#### Ubuntu CLI & RockyLinux CLI:
```bash
# SSH access
vagrant ssh ubuntu-cli
vagrant ssh rocky-cli

# Or use IP addresses
ssh vagrant@192.168.56.11  # Ubuntu CLI
ssh vagrant@192.168.56.12  # RockyLinux CLI
```

## 🔧 Troubleshooting

### Common Issues:

#### VirtualBox Installation Issues:
- **Windows**: Disable Hyper-V if VirtualBox fails to start
- **macOS**: Allow VirtualBox in Security & Privacy settings
- **Linux**: Ensure your user is in the `vboxusers` group

#### Vagrant Issues:
```bash
# If boxes fail to download
vagrant box update

# Clear Vagrant cache
rm -rf ~/.vagrant.d/boxes/

# Reload Vagrant configuration
vagrant reload --provision
```

#### Performance Issues:
- Reduce VM memory allocation in Vagrantfile
- Close unnecessary applications
- Enable hardware virtualization in BIOS/UEFI

#### Network Issues:
- Check VirtualBox Host-Only network adapter
- Disable host firewall temporarily for testing
- Verify IP ranges don't conflict with your network

### Getting Help:
```bash
# Vagrant help
vagrant --help
vagrant <command> --help

# Check logs
vagrant up --debug

# VirtualBox logs are in VM settings
```

## 📁 File Structure

```
vagrant-lab/
├── Vagrantfile          # Main configuration file
├── README.md           # This file
└── .vagrant/           # Vagrant metadata (auto-generated)
```

## 🛠️ Customization

### Modifying VM Resources:
Edit the Vagrantfile to change:
- Memory allocation (`vb.memory`)
- CPU cores (`vb.cpus`)
- Network settings
- Provisioning scripts

### Adding Software:
Add packages to the provisioning scripts in the Vagrantfile under the `SHELL` sections.

## 📚 Learning Resources

- [Vagrant Documentation](https://www.vagrantup.com/docs)
- [VirtualBox Manual](https://www.virtualbox.org/manual/)
- [Ubuntu Documentation](https://ubuntu.com/server/docs)
- [RockyLinux Documentation](https://docs.rockylinux.org/)

## 🔒 Security Notes

- Default credentials are vagrant/vagrant
- SSH keys are automatically configured
- Change default passwords in production use
- VMs are isolated in private network by default
- Always keep software updated

## 📄 License

This lab setup is provided for educational purposes. Please respect the licenses of the included operating systems and software packages.

---

## ⚠️ IMPORTANT DISCLAIMER ⚠️

**THIS LAB ENVIRONMENT IS FOR EDUCATIONAL AND PRACTICE PURPOSES ONLY**

- **DO NOT USE IN PRODUCTION ENVIRONMENTS**
- **YOU ARE RESPONSIBLE FOR YOUR OWN SYSTEM AND DATA**
- **USE AT YOUR OWN RISK**
- This setup may consume significant system resources
- Always backup your important data before proceeding
- The maintainers are not responsible for any damage or data loss
- Ensure you have adequate system resources before running all VMs simultaneously
- VMs will download several GB of data during initial setup
- Some antivirus software may flag virtualization activities
- By using this lab, you acknowledge that you understand these risks and accept full responsibility

**Remember: This is a practice environment only. You are responsible for your own system and data. Use at your own risk!**