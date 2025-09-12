# Virtual Lab Environment with VirtualBox and Vagrant

## Lab Overview

This Vagrant setup creates three virtual machines for learning and practice:

1. **Debian Lightweight GUI** (debian-gui) - Debian 12 (Bookworm) with XFCE desktop
   - IP: 192.168.56.10
   - Memory: 1GB, CPU: 1 core
   - Ultra-lightweight XFCE desktop environment with VirtualBox Guest Additions

2. **Ubuntu CLI** (ubuntu-cli) - Ubuntu 22.04 LTS command-line only
   - IP: 192.168.56.11  
   - Memory: 1GB, CPU: 1 core
   - Development tools and utilities pre-installed

3. **RockyLinux CLI** (rocky-cli) - RockyLinux 9 command-line only
   - IP: 192.168.56.12
   - Memory: 1GB, CPU: 1 core
   - Enterprise Linux environment for practice

## Pre-installed Software

### Debian Lightweight GUI (debian-gui)
**Desktop Environment:**
- **XFCE Desktop** (ultra-lightweight, uses only ~400MB RAM)
- **LightDM display manager** with auto-login configured
- **Lightning-fast boot times** (typically 10-20 seconds)
- **VirtualBox Guest Additions** (full screen mode, seamless integration)

**Applications & Tools:**
- **Web Browser**: Firefox ESR (Extended Support Release - Debian's stable browser)
- **Terminal**: XFCE4 Terminal (fast, lightweight terminal emulator)
- **File Manager**: Thunar (XFCE's efficient file manager)
- **Text Editors**: Mousepad (GUI text editor) + Vim (command-line editor)
- **Image Viewer**: Ristretto (lightweight image viewer)
- **Development Tools**: Git, Vim
- **System Utilities**: curl, wget, htop, net-tools

**Guest Additions Features:**
- **Full screen mode** support (Host+F or View menu)
- **Bidirectional clipboard** (copy/paste between host and guest)
- **Drag and drop** file sharing
- **Mouse integration** (seamless cursor movement)
- **Shared folder access** (vagrant user in vboxsf group)

**Performance Benefits:**
- **Minimal resource usage**: Only 1GB RAM, 1 CPU core required
- **Fast and responsive**: Optimized for virtual machine environments
- **Stable Debian base**: Rock-solid foundation for development work
- **Perfect for learning**: Full GUI functionality with minimal overhead

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

## Desktop Environment Options

The setup uses **Debian 12 with XFCE** for optimal performance in VirtualBox.

### Current Setup (Recommended):
- **Debian XFCE Desktop**: Ultra-lightweight, fast boot (~10-20 seconds), uses only ~400MB RAM
- **Perfect for VMs**: Optimized for virtual environments with minimal resources

### Current Setup Features:
- **Ultra-efficient**: Only 1GB RAM and 1 CPU core needed
- **Debian 12 (Bookworm)**: Rock-solid stable base
- **XFCE Desktop**: Professional yet lightweight interface
- **VirtualBox Guest Additions**: Full screen, clipboard sharing, drag & drop
- **Complete development environment**: Git, Vim, Firefox ESR, terminal

### Alternative Lightweight Options:
To use a different desktop environment, modify the Debian GUI VM provisioning script:

#### Even Lighter Options:
```bash
# After VM is running, you can install alternatives:

# LXDE (lighter than XFCE)
sudo apt install lxde-core

# LXQt (Qt-based minimal)
sudo apt install lxqt-core

# i3 Window Manager (for advanced users)
sudo apt install i3
```

### Performance Comparison:
| Desktop Environment | RAM Usage | Boot Time | CPU Cores | Best For |
|-------------------|-----------|-----------|-----------|----------|
| **Debian XFCE (Current)** | ~400MB | 10-20s | 1 core | **Ultra-lightweight, perfect balance** |
| Ubuntu LXDE | ~300MB | 8-15s | 1 core | Minimal resources |
| Ubuntu GNOME | ~1.5GB | 45-60s | 2+ cores | Full features |
| Ubuntu MATE | ~600MB | 20-35s | 1-2 cores | Traditional interface |
| i3 Window Manager | ~200MB | 5-10s | 1 core | Advanced users |

## System Requirements

### Minimum Requirements:
- **RAM**: 6GB (3GB+ available for VMs) - Very lightweight setup!
- **Storage**: 40GB free space
- **CPU**: Multi-core processor with virtualization support
- **OS**: Windows 10+, macOS 10.14+, or Linux

### Recommended:
- **RAM**: 16GB or more
- **Storage**: 100GB+ free space
- **CPU**: 4+ cores with VT-x/AMD-V enabled

## Prerequisites Installation

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

### Step 4: Install Git (Required for Repository Management)

#### Windows:
```bash
# Option 1: Download from official website
# Visit: https://git-scm.com/download/win
# Download and run the installer

# Option 2: Using Chocolatey (if installed)
choco install git

# Option 3: Using winget (Windows Package Manager)
winget install --id Git.Git -e --source winget
```

#### macOS:
```bash
# Option 1: Using Homebrew (recommended)
brew install git

# Option 2: Using Xcode Command Line Tools
xcode-select --install

# Option 3: Download from official website
# Visit: https://git-scm.com/download/mac
```

#### Linux (Ubuntu/Debian):
```bash
sudo apt update
sudo apt install git
```

#### Linux (RHEL/CentOS/Fedora):
```bash
# RHEL/CentOS
sudo yum install git

# Fedora
sudo dnf install git
```

#### Verify Git Installation:
```bash
git --version
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

## Usage Instructions

### Initial Setup

1. **Clone or download this repository**
   ```bash
   # Create a lab directory with a name of your choice
   mkdir vagrant-lab
   # Change into that directory
   cd vagrant-lab
   # To clone the content of a Git repository directly into your current directory, rather than creating a new subdirectory with the repository name
   git clone <repository-url> .
   ```

2. **Start all VMs**
   ```bash
   vagrant up
   ```

3. **Start individual VMs**
   ```bash
   # Debian with GUI
   vagrant up debian-gui

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

# Access Debian with GUI (will open VirtualBox window)
vagrant up debian-gui

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

#### Debian Lightweight GUI:
- Will open in VirtualBox window automatically
- Login: vagrant / vagrant
- Auto-login should be enabled
- **Ultra-fast boot time**: 10-20 seconds with only 1GB RAM
- **Full Screen Mode**: Use `Host+F` (usually Right Ctrl+F) or View menu
- **XFCE Features**: Right-click desktop for menu, bottom panel for apps
- **Performance**: Extremely responsive even with minimal resources

#### Ubuntu CLI & RockyLinux CLI:
```bash
# SSH access
vagrant ssh ubuntu-cli
vagrant ssh rocky-cli

# Or use IP addresses
ssh vagrant@192.168.56.11  # Ubuntu CLI
ssh vagrant@192.168.56.12  # RockyLinux CLI
```

## Troubleshooting

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

#### Guest Additions Issues (Debian GUI):
```bash
# If full screen mode doesn't work, reinstall Guest Additions
vagrant ssh debian-gui
sudo apt-get update
sudo apt-get install --reinstall virtualbox-guest-additions-iso
sudo reboot

# Or rebuild Guest Additions
sudo /opt/VBoxGuestAdditions-*/init/vboxadd setup
```

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

## File Structure

```
vagrant-lab/
├── Vagrantfile          # Main configuration file
├── README.md           # This documentation file
├── .gitignore          # Git ignore rules (optimized for macOS)
└── .vagrant/           # Vagrant metadata (auto-generated, ignored by git)
```

## Customization

### Modifying VM Resources:
Edit the Vagrantfile to change:
- Memory allocation (`vb.memory`)
- CPU cores (`vb.cpus`)
- Network settings
- Provisioning scripts

### Adding Software:
Add packages to the provisioning scripts in the Vagrantfile under the `SHELL` sections.

## Learning Resources

- [Vagrant Documentation](https://www.vagrantup.com/docs)
- [VirtualBox Manual](https://www.virtualbox.org/manual/)
- [Debian Documentation](https://www.debian.org/doc/)
- [Ubuntu Documentation](https://ubuntu.com/server/docs)
- [RockyLinux Documentation](https://docs.rockylinux.org/)

## Security Notes

- Default credentials are vagrant/vagrant
- SSH keys are automatically configured
- Change default passwords in production use
- VMs are isolated in private network by default
- Always keep software updated

## License

This lab setup is provided for educational purposes. Please respect the licenses of the included operating systems and software packages.

---

## IMPORTANT DISCLAIMER

**THIS LAB ENVIRONMENT IS FOR EDUCATIONAL AND PRACTICE PURPOSES ONLY**

- **DO NOT USE IN PRODUCTION ENVIRONMENTS**
- **YOU ARE RESPONSIBLE FOR YOUR OWN SYSTEM AND DATA**
- **USE AT YOUR OWN RISK**
- This setup may consume significant system resources (optimized for 6GB+ RAM)
- Always backup your important data before proceeding
- The maintainers are not responsible for any damage or data loss
- Ensure you have adequate system resources before running all VMs simultaneously
- VMs will download several GB of data during initial setup
- Some antivirus software may flag virtualization activities
- By using this lab, you acknowledge that you understand these risks and accept full responsibility

**Remember: This is a practice environment only. You are responsible for your own system and data. Use at your own risk!**
