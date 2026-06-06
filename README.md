# Virtual Lab Environment with VirtualBox and Vagrant

## Lab Overview

This Vagrant setup creates two minimal virtual machines for learning and practice:

1. **Debian XFCE** (`debian-gui`) — Debian 12 (Bookworm) with XFCE desktop
   - IP: `192.168.56.10`
   - Memory: 1 GB | CPU: 1 core
   - XFCE desktop environment (lightweight) with LightDM auto-login
   - 3D acceleration, 128 MB VRAM, bidirectional clipboard & drag-and-drop

2. **Ubuntu CLI** (`ubuntu-cli`) — Ubuntu 22.04 LTS, command-line only
   - IP: `192.168.56.11`
   - Memory: 1 GB | CPU: 1 core
   - Headless (no GUI)
   - **2 extra virtual disks (5 GB each)** for storage/partition labs

---

## 💾 Extra Disks — ubuntu-cli

> ⚠️ This requires the **vagrant-disksize** plugin or Vagrant ≥ 2.2.x with VirtualBox disk support.

The `ubuntu-cli` VM is provisioned with two additional virtual disks for hands-on storage practice:

| Disk Name           | Device  | Size  | Purpose                        |
|---------------------|---------|-------|--------------------------------|
| `ubuntu_cli_disk1`  | `/dev/sdc` | 5 GB  | Partition / filesystem labs    |
| `ubuntu_cli_disk2`  | `/dev/sdd` | 5 GB  | LVM / RAID / mount point labs  |

These disks are managed automatically by Vagrant:
- On `vagrant up` — disks are created and attached
- On `vagrant reload` — disks are re-attached
- On `vagrant destroy` — disks are deleted

### Verify disks inside the VM:
```bash
vagrant ssh ubuntu-cli
lsblk
```

Expected output:
```
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
loop0    7:0    0 63.8M  1 loop /snap/core20/2599
loop1    7:1    0 89.4M  1 loop /snap/lxd/31333
loop2    7:2    0 49.3M  1 loop /snap/snapd/24792
sda      8:0    0   40G  0 disk
└─sda1   8:1    0   40G  0 part /
sdb      8:16   0   10M  0 disk
sdc      8:32   0    5G  0 disk          ← ubuntu_cli_disk1
sdd      8:48   0    5G  0 disk          ← ubuntu_cli_disk2
```



---

## System Requirements

### Minimum:
- **RAM**: 3 GB (2 GB+ available for VMs)
- **Storage**: 30 GB free space (+ ~10 GB for the extra disks)
- **CPU**: Multi-core with virtualization support (VT-x / AMD-V)
- **OS**: Windows 10+, macOS 10.14+, or Linux

### Recommended:
- **RAM**: 8 GB or more
- **Storage**: 50 GB+ free space
- **CPU**: 4+ cores with VT-x/AMD-V enabled in BIOS

---

## Prerequisites Installation

### Step 1: Install VirtualBox

#### Windows:
1. Download from: https://www.virtualbox.org/wiki/Downloads
2. Run the `.exe` installer as Administrator
3. Follow the wizard (accept defaults)
4. Restart if prompted

#### macOS:
```bash
brew install --cask virtualbox
```

#### Linux (Ubuntu/Debian):
```bash
wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
echo "deb [arch=amd64] https://download.virtualbox.org/virtualbox/debian $(lsb_release -cs) contrib" | sudo tee /etc/apt/sources.list.d/virtualbox.list
sudo apt update && sudo apt install virtualbox-7.0
```

#### Linux (RHEL/CentOS/Fedora):
```bash
sudo dnf install VirtualBox
```

---

### Step 2: Install Vagrant

#### Windows:
1. Download from: https://www.vagrantup.com/downloads
2. Run the `.msi` installer as Administrator
3. Restart your terminal/PowerShell

#### macOS:
```bash
brew install --cask vagrant
```

#### Linux (Ubuntu/Debian):
```bash
wget https://releases.hashicorp.com/vagrant/2.4.0/vagrant_2.4.0-1_amd64.deb
sudo dpkg -i vagrant_2.4.0-1_amd64.deb
```

#### Linux (RHEL/CentOS/Fedora):
```bash
sudo dnf install vagrant
```

---

### Step 3: Verify Installation

```bash
vboxmanage --version
vagrant --version
```

---

### Step 4: Install Git (Optional)

#### Windows:
```bash
winget install --id Git.Git -e --source winget
```

#### macOS:
```bash
brew install git
```

#### Linux:
```bash
sudo apt install git        # Ubuntu/Debian
sudo dnf install git        # RHEL/CentOS/Fedora
```

---

## Usage Instructions

### Initial Setup

```bash
git clone https://github.com/ahmedjorani/Linux-Virtual-Lab-Environment.git
cd Linux-Virtual-Lab-Environment
vagrant up
```

### Start Individual VMs

```bash
vagrant up debian-gui    # Debian XFCE desktop
vagrant up ubuntu-cli    # Ubuntu CLI + extra disks
```

---

## Common Vagrant Commands

```bash
vagrant status           # Show VM states
vagrant ssh debian-gui   # SSH into Debian VM
vagrant ssh ubuntu-cli   # SSH into Ubuntu VM
vagrant suspend          # Save VM state
vagrant resume           # Resume suspended VMs
vagrant halt             # Graceful shutdown
vagrant reload           # Restart + re-apply config
vagrant provision        # Re-run provisioning scripts
vagrant destroy          # Delete VMs (irreversible)
```

---

## Accessing the VMs

### Debian XFCE GUI
- Opens automatically in a VirtualBox window
- Auto-login as `vagrant` into XFCE desktop
- Credentials: `vagrant` / `vagrant`

### Ubuntu CLI
```bash
vagrant ssh ubuntu-cli

# Or via SSH directly
ssh -i ~/.vagrant.d/insecure_private_keys/vagrant.key.rsa vagrant@192.168.56.11
```

---

## Credentials

| Username | Password |
|----------|----------|
| `vagrant` | `vagrant` |

Both VMs grant passwordless `sudo` to the `vagrant` user.

---

## Shared Folder

The project directory on your host is synced to `/vagrant` inside both VMs:

```bash
ls /vagrant    # run inside either VM
```

---

## Troubleshooting

### VirtualBox
- **Windows**: Disable Hyper-V if VirtualBox fails to start VMs
- **macOS**: Allow VirtualBox in System Settings → Privacy & Security
- **Linux**: Add your user to the `vboxusers` group: `sudo usermod -aG vboxusers $USER`

### Vagrant
```bash
vagrant box update          # Update boxes to latest version
rm -rf ~/.vagrant.d/boxes/  # Clear cached boxes
vagrant reload              # Reload config changes
vagrant up --debug          # Verbose output for debugging
```

### Extra Disks Not Appearing
- Ensure you are using **Vagrant ≥ 2.2.x** with VirtualBox
- Run `vagrant reload` to re-attach disks if they disappear after a restart
- Removing a disk entry from the Vagrantfile and running `vagrant reload` will detach and delete it

### Performance
- Reduce `vb.memory` in the Vagrantfile if the host is low on RAM
- Enable hardware virtualization (VT-x/AMD-V) in BIOS/UEFI
- Close unused applications on the host

---

## File Structure

```
Linux-Virtual-Lab-Environment/
├── Vagrantfile       # VM definitions and provisioning
├── README.md         # This file
├── .gitignore
└── .vagrant/         # Vagrant internal state (do not commit)
```

---

## Customization

Edit `Vagrantfile` to adjust:
- Memory: `vb.memory = "2048"`
- CPUs: `vb.cpus = 2`
- Extra disks: add/remove `vm.disk` entries under `ubuntu-cli`

Install additional packages inside a VM:
```bash
vagrant ssh ubuntu-cli
sudo apt update && sudo apt install <package>
```

---

## Security Notes

- Default credentials: `vagrant` / `vagrant` — **change before any shared use**
- SSH key automatically copied to `~/.ssh/vagrant.key.rsa` inside each VM
- VMs are isolated in a private network (`192.168.56.0/24`)

---

## ⚠️ DISCLAIMER

**THIS LAB IS FOR EDUCATIONAL AND PRACTICE PURPOSES ONLY**

- Do **not** use in production
- Use at your own risk
- You are responsible for your system and data
- First-time setup will download several GB of box images
- Always backup important data before running `vagrant destroy`
