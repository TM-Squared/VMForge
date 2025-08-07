# VMForge

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Shell](https://img.shields.io/badge/shell-bash-green.svg)
![Platform](https://img.shields.io/badge/platform-linux-lightgrey.svg)


Quick VM creation tool with KVM/QEMU - Create and launch virtual machines in seconds.



## Author
**TM²** ([github.com/TM-Squared](https://github.com/TM-Squared)) - Gintou Organization


## Description
VMForge is a KISS (Keep It Simple, Stupid) script that automates the creation of virtual machines using virt-builder and QEMU/KVM. It supports both interactive mode and command-line arguments for maximum flexibility.

## Features
- Interactive and CLI modes
- Support for Debian and Ubuntu images
- Automatic SSH key generation and injection
- Custom package installation
- Network configuration with DHCP
- Automatic VM startup
- Essential packages always included (openssh-server, sudo)

## Dependencies

### Required Tools
```bash
# Core virtualization tools
sudo apt install qemu-system-x86 qemu-kvm libvirt-daemon-system

# virt-* tools for image creation and customization
sudo apt install virtinst virt-viewer virt-manager libguestfs-tools

# Specifically needed
- virt-builder    # Create base VM images
- virt-customize  # Customize VM images  
- qemu-system-x86_64  # Run VMs with KVM acceleration
```

### Installation Commands

#### Debian/Ubuntu
```bash
# Install all dependencies
sudo apt update
sudo apt install qemu-system-x86 qemu-kvm libvirt-daemon-system \
                 virtinst virt-viewer virt-manager libguestfs-tools

# Add user to libvirt group
sudo usermod -aG libvirt $USER
sudo usermod -aG kvm $USER

# Restart to apply group changes
# or run: newgrp libvirt && newgrp kvm
```

#### Fedora/RHEL
```bash
sudo dnf install qemu-kvm libvirt virt-install virt-viewer \
                 virt-manager libguestfs-tools

sudo systemctl enable --now libvirtd
sudo usermod -aG libvirt $USER
```

#### Arch Linux
```bash
sudo pacman -S qemu-full libvirt virt-manager virt-viewer \
               libguestfs edk2-ovmf

sudo systemctl enable --now libvirtd
sudo usermod -aG libvirt $USER
```

### Verify Installation
```bash
# Check KVM support
kvm-ok
# or
egrep -c '(vmx|svm)' /proc/cpuinfo

# Verify virt-builder works
virt-builder --list | head -5

# Check QEMU
qemu-system-x86_64 --version

# Test libvirt
virsh list --all
```

## Usage

### Quick Start
```bash
# Make executable
chmod +x vmforge

# Interactive mode (recommended for first use)
./vmforge

# Quick VM with arguments
./vmforge -n myvm -u admin -p secret

# Custom packages
./vmforge -n devvm --packages "git,htop,curl,nodejs"
```

### Command Line Options
```
OPTIONS:
    -o, --os          OS image (ex: debian-12, ubuntu-22.04)
    -n, --name        VM name
    -u, --user        Username
    -p, --password    User password
    -s, --size        Disk size (ex: 8G)
    -m, --memory      RAM in MB (ex: 2048)
    -c, --cpus        Number of CPUs (ex: 2)
    -P, --port        SSH port (ex: 2222)
    -k, --key         SSH key path
    --packages        Additional packages to install
    --create-key      Create SSH key if not exists
    --no-start        Don't start VM after creation
    -i, --interactive Interactive mode
    -h, --help        Show help
```

### Examples
```bash
# Interactive mode
./vmforge -i

# Quick Debian VM
./vmforge -o debian-12 -n webserver -u admin

# Ubuntu development VM
./vmforge -o ubuntu-22.04 -n devbox -u dev --packages "git,nodejs,docker.io"

# With custom SSH key
./vmforge -n testvm --create-key -k ~/.ssh/testvm_key

# Large VM for heavy workloads
./vmforge -n bigvm -m 4096 -c 4 -s 20G
```

## Available Images
```bash
# List Debian images
virt-builder --list | grep debian

# List Ubuntu images  
virt-builder --list | grep ubuntu

# List all available images
virt-builder --list
```

## VM Management

### Connect to VM
```bash
# SSH with password
ssh username@localhost -p 2222

# SSH with key (if configured)
ssh -i ~/.ssh/vm_key username@localhost -p 2222
```

### VM Control
```bash
# Check running VMs
ps aux | grep qemu

# Stop VM
pkill qemu-system-x86_64

# Clean SSH keys (if connection issues)
ssh-keygen -R "[localhost]:2222"
```

## Troubleshooting

### KVM Permission Issues
```bash
# Check KVM permissions
ls -la /dev/kvm

# Add user to kvm group if needed
sudo usermod -aG kvm $USER
newgrp kvm
```

### SSH Connection Issues
```bash
# VM takes time to boot, wait 30-60 seconds then:
ssh username@localhost -p 2222

# If key conflicts:
ssh-keygen -R "[localhost]:2222"

# Force password authentication:
ssh -o PreferredAuthentications=password username@localhost -p 2222
```

### virt-builder Issues
```bash
# Update template cache
virt-builder --cache-all-templates

# Clear cache if corrupted
virt-builder --delete-cache

# Check available templates
virt-builder --list
```

## Default Configuration
- **OS Image**: debian-12
- **Memory**: 2048 MB
- **CPUs**: 2
- **Disk**: 8G
- **SSH Port**: 2222
- **Essential packages**: openssh-server, sudo (always installed)
- **Default packages**: vim

## License
MIT License - See LICENSE file for details.

## Contributing
Issues and pull requests welcome! Keep it simple and KISS compliant.
