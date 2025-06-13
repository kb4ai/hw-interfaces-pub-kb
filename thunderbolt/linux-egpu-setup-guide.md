# Linux eGPU Setup Guide

## Prerequisites

### System Requirements
- Thunderbolt 3/4/5 capable system
- Linux kernel 5.6+ (6.x recommended)
- UEFI/BIOS with Thunderbolt support
- 650W+ PSU in eGPU enclosure for high-end cards

### BIOS/UEFI Settings
1. **Thunderbolt Security**: Set to "User Authorization" or "No Security"
2. **Thunderbolt Boot Support**: Enable
3. **PCIe ASPM**: Disable (for stability)
4. **Above 4G Decoding**: Enable
5. **Resizable BAR**: Enable if available

## Installation

### 1. Install Required Packages

**Ubuntu/Debian**:
```bash
# Essential packages
sudo apt update
sudo apt install bolt thunderbolt-tools
sudo apt install linux-modules-extra-$(uname -r)
sudo apt install build-essential dkms

# For NVIDIA
sudo apt install nvidia-driver-545  # or latest

# For AMD
# Drivers included in kernel
```

**Arch Linux**:
```bash
# Essential packages
sudo pacman -S bolt thunderbolt-tools linux-headers

# For NVIDIA
sudo pacman -S nvidia nvidia-utils nvidia-settings

# For AMD
sudo pacman -S mesa vulkan-radeon libva-mesa-driver

# For KDE Plasma
sudo pacman -S plasma-thunderbolt
```

### 2. Configure Thunderbolt

**Check connection**:
```bash
boltctl list
# Should show your eGPU enclosure
```

**Authorize device**:
```bash
# One-time authorization
sudo boltctl authorize [DEVICE_UUID]

# Permanent authorization
sudo boltctl enroll [DEVICE_UUID]
```

**Auto-authorization** (use with caution):
```bash
# /etc/udev/rules.d/99-thunderbolt-auto.rules
ACTION=="add", SUBSYSTEM=="thunderbolt", ATTR{authorized}=="0", ATTR{authorized}="1"
```

### 3. GPU Configuration

#### NVIDIA Setup

**Create Xorg config** `/etc/X11/xorg.conf.d/20-nvidia-egpu.conf`:
```bash
Section "Module"
    Load "modesetting"
EndSection

Section "Device"
    Identifier     "NVIDIA eGPU"
    Driver         "nvidia"
    BusID          "PCI:XX:0:0"  # Get from lspci | grep -i nvidia
    Option         "AllowExternalGpus" "True"
    Option         "AllowEmptyInitialConfiguration"
EndSection
```

**Early module loading** `/etc/mkinitcpio.conf` (Arch) or `/etc/initramfs-tools/modules` (Ubuntu):
```bash
# Add these modules
thunderbolt
nvidia
nvidia_modeset
nvidia_uvm
nvidia_drm
```

**Kernel parameters** `/etc/default/grub`:
```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash nvidia-drm.modeset=1"
```

#### AMD Setup (Simpler)

**Usually works out of box**, but ensure:
```bash
# Check if detected
lspci | grep -i amd

# Verify driver loaded
lsmod | grep amdgpu
```

### 4. Display Configuration

#### Option A: eGPU as Primary (Recommended for Desktop)

**GDM/SDDM Configuration**:
```bash
# Force display manager to use eGPU
sudo cp /usr/share/X11/xorg.conf.d/10-nvidia.conf /etc/X11/xorg.conf.d/
# Edit to add BusID of eGPU
```

#### Option B: Offload Rendering (Laptop with Internal Display)

**For NVIDIA (PRIME)**:
```bash
# Run application on eGPU
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia application_name

# Create wrapper script
echo 'export __NV_PRIME_RENDER_OFFLOAD=1
export __GLX_VENDOR_LIBRARY_NAME=nvidia
exec "$@"' > ~/bin/prime-run
chmod +x ~/bin/prime-run
```

**For AMD (DRI_PRIME)**:
```bash
DRI_PRIME=1 application_name
```

### 5. Performance Optimization

**PCIe Link Speed Check**:
```bash
# Should show 8GT/s for PCIe 3.0 or 16GT/s for PCIe 4.0
sudo lspci -vv | grep -A 20 "VGA\|3D" | grep LnkSta
```

**Power Management**:
```bash
# NVIDIA PowerMizer
nvidia-settings -a "[gpu:0]/GPUPowerMizerMode=1"  # Prefer maximum performance

# AMD
echo "performance" | sudo tee /sys/class/drm/card*/device/power_dpm_force_performance_level
```

## Troubleshooting

### Common Issues

**eGPU not detected**:
```bash
# Check Thunderbolt controller
lspci | grep -i thunder

# Check kernel messages
dmesg | grep -i thunderbolt

# Rescan PCIe bus
echo 1 | sudo tee /sys/bus/pci/rescan
```

**Black screen after boot**:
- Boot with `nomodeset` kernel parameter
- Configure display manager to ignore eGPU initially
- Use SSH to configure after boot

**Performance issues**:
```bash
# Check if running on eGPU
glxinfo | grep "OpenGL renderer"
# or
nvidia-smi

# Check PCIe bandwidth
sudo nvidia-smi -q | grep -i pcie
```

### Kernel 6.8+ Regression Fix
```bash
# Add to kernel parameters
thunderbolt.host_reset=0

# Or downgrade bolt
# Download bolt 0.8 from package archives
```

## Recommended eGPU Enclosures

### Best for Linux
1. **Razer Core X** (not Chroma)
   - 650W PSU
   - No USB controller conflicts
   - ~$300

2. **Sonnet eGFX Breakaway Box 650W**
   - Reliable PCIe implementation
   - Good thermal design
   - ~$400

3. **Akitio Node Titan**
   - 650W PSU
   - Thunderbolt daisy-chain support
   - ~$350

### Avoid on Linux
- Razer Core X Chroma (USB/Ethernet issues)
- Gigabyte AORUS Gaming Box (firmware problems)
- Most all-in-one eGPUs (limited flexibility)

## Multi-GPU Setup for AI/LLM

```bash
# Check all GPUs detected
nvidia-smi -L

# Set CUDA device visibility
export CUDA_VISIBLE_DEVICES=0,1  # Use first two GPUs

# Distribute load
# Most ML frameworks handle multi-GPU automatically
```

### Performance Expectations
- **Single GPU**: 70-80% of native performance
- **Multi-GPU**: Better than single internal GPU for parallel workloads
- **LLM Inference**: Minimal impact from TB bandwidth
- **Training**: 20-30% slower than native PCIe

## Automation Scripts

**eGPU Switch Script** `~/bin/egpu-switch`:
```bash
#!/bin/bash
case "$1" in
  on)
    sudo boltctl authorize $(boltctl list | grep -B1 "Razer" | head -1 | awk '{print $1}')
    sudo systemctl restart display-manager
    ;;
  off)
    sudo systemctl stop display-manager
    echo 1 | sudo tee /sys/bus/pci/devices/0000:XX:00.0/remove
    ;;
  status)
    nvidia-smi || echo "eGPU not active"
    ;;
esac
```

Make executable: `chmod +x ~/bin/egpu-switch`