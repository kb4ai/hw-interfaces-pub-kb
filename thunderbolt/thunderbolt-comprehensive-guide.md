# Thunderbolt Comprehensive Guide

## Standards Comparison

### Thunderbolt 3
- **Bandwidth**: 40 Gbps total (32.4 Gbps theoretical PCIe, 22-24 Gbps practical)
- **PCIe**: 4×PCIe 3.0 lanes, hardcoded 22 Gbps cap (18 Gbps DisplayPort reserved)
- **Real-world**: 25.92 Gbps after 8b/10b encoding, ~22-24 Gbps measured
- **DisplayPort**: DP 1.2/1.4, priority over PCIe, scales with resolution
- **Power**: Up to 100W charging, 15W to devices
- **Daisy-chain**: Up to 6 devices

### Thunderbolt 4
- **Bandwidth**: 40 Gbps bidirectional
- **PCIe**: PCIe 3.0 with mandatory 32 Gbps (removed TB3's 22 Gbps cap)
- **DisplayPort**: DP 1.4 with DP 2.0 support
- **Power**: Up to 100W charging, 15W to devices
- **Improvements**: VT-d DMA protection, 2m cables, wake from sleep, hub support

### Thunderbolt 5
- **Bandwidth**: 80 Gbps bidirectional, 120 Gbps with Bandwidth Boost
- **PCIe**: PCIe 4.0 with 64 Gbps bandwidth
- **DisplayPort**: DP 2.1 supporting 80 Gbps video
- **Power**: Up to 240W (USB PD 3.1)
- **Key features**: PAM-3 signaling, asymmetric bandwidth

## Display Bandwidth Requirements

### 4K Displays
| Resolution | Refresh | Bit Depth | Bandwidth | Standard Required |
|------------|---------|-----------|-----------|-------------------|
| 4K | 60Hz | 8-bit | 14.33 Gbps | DP 1.2 |
| 4K | 120Hz | 8-bit | 28.66 Gbps | DP 1.4 + DSC |
| 4K | 144Hz | 8-bit | 34.39 Gbps | DP 2.0 |
| 4K | 144Hz | 10-bit | 42.99 Gbps | DP 2.0 |

### 5K/8K Displays
- **5K @ 60Hz**: 25.46 Gbps (barely fits DP 1.4)
- **8K @ 60Hz**: 57.32 Gbps (requires DP 2.0)
- **8K @ 60Hz 10-bit**: 71.65 Gbps (needs DP 2.0 UHBR 20)

### Bandwidth Impact on PCIe

**TB3 (22 Gbps PCIe cap)**:
- No display: 22 Gbps PCIe available
- 1× 4K@60Hz (14 Gbps): 8 Gbps PCIe remaining
- 1× 5K@60Hz (22 Gbps): 0 Gbps PCIe remaining
- 2× 4K@60Hz: Exceeds TB3 total bandwidth

**TB4 (32 Gbps PCIe)**:
- 1× 4K@60Hz: 18 Gbps PCIe available
- 2× 4K@60Hz: Limited by 40 Gbps total
- 1× 5K@60Hz: 10 Gbps PCIe available

## Linux Compatibility

### Kernel Requirements
- **Minimum**: 4.13+ (basic Thunderbolt security)
- **Recommended**: 5.6+ (better compatibility)
- **Latest**: 6.8+ (may have regressions)

### Essential Packages

**Ubuntu**:
```bash
sudo apt install bolt thunderbolt-tools
sudo apt install linux-modules-extra-$(uname -r)  # Critical!
```

**Arch Linux**:
```bash
sudo pacman -S bolt thunderbolt-tools
# KDE users:
sudo pacman -S plasma-thunderbolt
```

### Working Docks & Hubs

1. **Lenovo ThinkPad TB3 Dock Gen 2**
   - Excellent Arch Linux support
   - Works out of box with kernel 5.6.7+
   - 135W power delivery

2. **CalDigit TS4**
   - 18 ports, 98W charging
   - May need power cycling on Ubuntu 20.04
   - Requires TB authorization

3. **Dell WD19TB/TB16**
   - Good support with updated firmware
   - Enterprise-grade reliability

## eGPU Support

### Performance Impact
- **TB3/4**: ~20-30% performance loss
- **TB5**: ~14% performance loss (RTX 5090)
- **OCuLink**: Better than TB but still ~23% loss

### High-End GPU Compatibility

**NVIDIA RTX 5090 (2025)**
- 575W TDP, 32GB GDDR7
- Requires TB5 for optimal performance
- Linux drivers: 570.86.10+
- Current enclosures underpowered

**NVIDIA RTX 6000 Ada**
- 300W TDP, 48GB ECC
- Ubuntu 24.04 LTS certified
- Professional Linux support

**AMD Options**
- RX 7900 XTX: Excellent open-source drivers
- PRO W7900: 48GB, enterprise certified
- Better plug-and-play on Linux

### Recommended eGPU Enclosures

**Current Generation**:
- Razer Core X (650W) - Best Linux compatibility
- Avoid Core X Chroma - USB/Ethernet issues

**Future (TB5)**:
- ASUS ROG XG Mobile 2025 - First TB5 eGPU
- Supports RTX 5090 properly

### eGPU Configuration

```bash
# Add to /etc/X11/xorg.conf.d/20-nvidia.conf
Section "Device"
    Identifier "NVIDIA Card"
    Driver "nvidia"
    BusID "PCI:XX:XX:X"  # From lspci
    Option "AllowExternalGpus" "True"
EndSection

# Early load thunderbolt (mkinitcpio.conf)
MODULES=(thunderbolt)
```

## Best Practices

### Display Setup
1. High-bandwidth displays first in chain
2. Use DSC when available
3. Consider TB5 for multi-4K setups

### eGPU Usage
1. AMD GPUs for best Linux experience
2. Wait for TB5 enclosures for RTX 5090
3. Consider multiple mid-range GPUs for AI workloads

### Troubleshooting
```bash
# Check devices
boltctl list

# Authorize device
boltctl authorize [UUID]

# For kernel 6.8+ issues
GRUB_CMDLINE_LINUX_DEFAULT="... thunderbolt.host_reset=0"
```

## Key Takeaways

1. **TB3/4 bandwidth** shared between display and data
2. **TB5** doubles PCIe bandwidth, essential for latest GPUs
3. **Linux support** mature but requires configuration
4. **AMD GPUs** offer superior Linux compatibility
5. **Current TB** bottlenecks flagship GPUs significantly