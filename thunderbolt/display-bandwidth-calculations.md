# Display Bandwidth Calculations

## Formula

**Bandwidth (Gbps) = Width × Height × Refresh Rate × Bit Depth × 1.25 (timing overhead) / 10⁹**

## Common Display Configurations

### Single Displays

| Display Type | Resolution | Refresh | Color | Raw Bandwidth | With DSC (3:1) |
|--------------|------------|---------|-------|---------------|----------------|
| 1080p | 1920×1080 | 60Hz | 8-bit | 3.20 Gbps | 1.07 Gbps |
| 1440p | 2560×1440 | 60Hz | 8-bit | 5.68 Gbps | 1.89 Gbps |
| 4K | 3840×2160 | 60Hz | 8-bit | 12.80 Gbps | 4.27 Gbps |
| 4K | 3840×2160 | 120Hz | 8-bit | 25.60 Gbps | 8.53 Gbps |
| 4K | 3840×2160 | 144Hz | 10-bit | 38.40 Gbps | 12.80 Gbps |
| 5K | 5120×2880 | 60Hz | 8-bit | 22.73 Gbps | 7.58 Gbps |
| 8K | 7680×4320 | 30Hz | 8-bit | 25.60 Gbps | 8.53 Gbps |
| 8K | 7680×4320 | 60Hz | 10-bit | 64.00 Gbps | 21.33 Gbps |

### Multi-Display Configurations

#### Thunderbolt 3/4 (40 Gbps total)
| Configuration | Total Bandwidth | PCIe Available |
|---------------|-----------------|----------------|
| 1× 4K@60Hz | 12.80 Gbps | 27.20 Gbps |
| 2× 4K@60Hz | 25.60 Gbps | 14.40 Gbps |
| 1× 4K@120Hz | 25.60 Gbps | 14.40 Gbps |
| 1× 5K@60Hz | 22.73 Gbps | 17.27 Gbps |
| 3× 1440p@60Hz | 17.04 Gbps | 22.96 Gbps |

#### Thunderbolt 5 (80/120 Gbps)
| Configuration | Total Bandwidth | Mode | PCIe Available |
|---------------|-----------------|------|----------------|
| 2× 4K@144Hz | 51.20 Gbps | Normal | 28.80 Gbps |
| 3× 4K@120Hz | 76.80 Gbps | Normal | 3.20 Gbps |
| 2× 8K@60Hz | 102.40 Gbps | Boost | 17.60 Gbps |
| 3× 4K@144Hz 10-bit | 115.20 Gbps | Boost | 4.80 Gbps |

## Daisy-Chain Bandwidth Distribution

### DisplayPort MST Example (DP 1.4 = 25.92 Gbps)
```
Display 1 (4K@60Hz) → Uses 12.80 Gbps
├── Remaining: 13.12 Gbps
Display 2 (1440p@60Hz) → Uses 5.68 Gbps  
├── Remaining: 7.44 Gbps
Display 3 (1080p@60Hz) → Uses 3.20 Gbps
└── Remaining: 4.24 Gbps
```

### Thunderbolt Daisy-Chain (Mixed Devices)
```
TB3 Port (40 Gbps)
├── eGPU → Allocates 32 Gbps PCIe
├── Remaining: 8 Gbps
└── Display (1080p@60Hz) → Uses 3.20 Gbps
    └── Final: 4.80 Gbps for other devices
```

## Practical Recommendations

### For Productivity (Office Work)
- **TB3/4**: Up to 2× 4K@60Hz or 1× 5K@60Hz
- **TB5**: Up to 3× 4K@60Hz or 2× 5K@60Hz

### For Gaming/Content Creation  
- **TB3/4**: 1× 4K@120Hz maximum
- **TB5**: 2× 4K@144Hz or 1× 8K@60Hz

### For eGPU + Displays
- **TB3/4**: 1× 4K@60Hz + eGPU (limited performance)
- **TB5**: 2× 4K@60Hz + eGPU (good performance)

## Bandwidth Optimization Tips

1. **Use DSC** when available (3:1 compression ratio)
2. **Reduce color depth** from 10-bit to 8-bit saves 25%
3. **Lower refresh rates** for secondary displays
4. **Direct connections** for primary display when possible
5. **USB-C alternate mode** can bypass TB bandwidth sharing