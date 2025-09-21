# Virtual Lab Environment with VirtualBox and Vagrant

## Lab Overview

This Vagrant setup creates two minimal virtual machines for learning and practice:

1. **Debian XFCE** (debian-gui) - Debian 12 (Bookworm) with XFCE desktop
   - IP: 192.168.56.10
   - Memory: 1GB, CPU: 1 core
   - XFCE desktop environment (lightweight)

2. **Ubuntu CLI** (ubuntu-cli) - Ubuntu 22.04 LTS command-line only
   - IP: 192.168.56.11  
   - Memory: 1GB, CPU: 1 core
   - Minimal installation

## System Requirements

### Minimum Requirements:
- **RAM**: 3GB (2GB+ available for VMs)
- **Storage**: 30GB free space
- **CPU**: Multi-core processor with virtualization support
- **OS**: Windows 10+, macOS 10.14+, or Linux

### Recommended:
- **RAM**: 8GB or more
- **Storage**: 50GB+ free space
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
sudo dnf install VirtualBox
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

### Step 4: Install Git (Optional)

#### Windows:
```bash
# Download from: https://git-scm.com/download/win
# Or use: winget install --id Git.Git -e --source winget
```

#### macOS:
```bash
brew install git
```

#### Linux:
```bash
# Ubuntu/Debian
sudo apt install git

# RHEL/CentOS/Fedora
sudo dnf install git
```

## Usage Instructions

### Initial Setup

1. **Clone or download this repository**
   ```bash
   mkdir vagrant-lab
   cd vagrant-lab
   git clone <repository-url> .
   ```

2. **Start all VMs**
   ```bash
   vagrant up
   ```

3. **Start individual VMs**
   ```bash
   # Debian with XFCE
   vagrant up debian-gui

   # Ubuntu CLI only
   vagrant up ubuntu-cli
   ```

### Basic Vagrant Commands

```bash
# Check status
vagrant status

# SSH into VMs
vagrant ssh debian-gui
vagrant ssh ubuntu-cli

# Suspend VMs
vagrant suspend

# Resume VMs
vagrant resume

# Shutdown VMs
vagrant halt

# Restart VMs
vagrant reload

# Destroy VMs
vagrant destroy
```

### Accessing the VMs

#### Debian XFCE GUI:
- Will open in VirtualBox window automatically
- Login: vagrant / vagrant
- Auto-login enabled
- XFCE desktop with applications menu, file manager, and terminal

#### Ubuntu CLI:
```bash
# SSH access
vagrant ssh ubuntu-cli

# Or use IP
ssh vagrant@192.168.56.11
```

## Troubleshooting

### Common Issues:

#### VirtualBox Issues:
- **Windows**: Disable Hyper-V if VirtualBox fails
- **macOS**: Allow VirtualBox in Security & Privacy settings
- **Linux**: Ensure user is in `vboxusers` group

#### Vagrant Issues:
```bash
# Update boxes
vagrant box update

# Clear cache
rm -rf ~/.vagrant.d/boxes/

# Reload configuration
vagrant reload
```

#### Performance Issues:
- Reduce memory in Vagrantfile
- Close unnecessary applications
- Enable hardware virtualization in BIOS

### Getting Help:
```bash
vagrant --help
vagrant up --debug
```

## File Structure

```
vagrant-lab/
├── Vagrantfile
├── README.md
├── .gitignore
└── .vagrant/
```

## Customization

Edit Vagrantfile to change:
- Memory allocation (`vb.memory`)
- CPU cores (`vb.cpus`)
- Network settings

Install additional software:
```bash
vagrant ssh debian-gui
sudo apt update
sudo apt install <package-name>
```

## Security Notes

- Default credentials: vagrant/vagrant
- SSH keys automatically configured
- VMs isolated in private network
- Change passwords for production use

## IMPORTANT DISCLAIMER

**THIS LAB ENVIRONMENT IS FOR EDUCATIONAL AND PRACTICE PURPOSES ONLY**

- **DO NOT USE IN PRODUCTION**
- **USE AT YOUR OWN RISK**
- You are responsible for your system and data
- VMs will download several GB during setup
- Ensure adequate system resources
- Always backup important data

**This is a practice environment only!**
