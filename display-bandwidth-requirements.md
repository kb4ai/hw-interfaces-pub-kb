# Display Bandwidth Requirements

## Overview

This document provides detailed bandwidth calculations for various display configurations, including 4K, 5K, and 8K displays at different refresh rates, and their impact on DisplayPort and Thunderbolt interfaces.

## Bandwidth Calculation Formula

Display bandwidth is calculated using:
```
Bandwidth (Gbps) = (Horizontal × Vertical × Refresh Rate × Bit Depth × 3) / 1,000,000,000
```

Where:
- Horizontal × Vertical = Resolution (pixels)
- Refresh Rate = Hz
- Bit Depth = Color depth per channel (typically 8 or 10 bits)
- 3 = RGB channels
- Result includes blanking intervals (typically ~20% overhead)

## 4K Display Bandwidth Requirements

### 4K (3840 × 2160) at Various Refresh Rates

| Configuration | Raw Bandwidth | With Overhead (20%) | DisplayPort Version Required |
|--------------|---------------|---------------------|----------------------------|
| 4K @ 60Hz, 8-bit | 11.94 Gbps | 14.33 Gbps | DP 1.2 (17.28 Gbps) |
| 4K @ 60Hz, 10-bit | 14.93 Gbps | 17.91 Gbps | DP 1.3/1.4 (25.92 Gbps) |
| 4K @ 120Hz, 8-bit | 23.88 Gbps | 28.66 Gbps | DP 1.4 with DSC or DP 2.0 |
| 4K @ 144Hz, 8-bit | 28.66 Gbps | 34.39 Gbps | DP 2.0 (77.4 Gbps) |
| 4K @ 144Hz, 10-bit | 35.82 Gbps | 42.99 Gbps | DP 2.0 |

### With Display Stream Compression (DSC)
DSC typically provides 3:1 compression ratio:
- 4K @ 120Hz, 8-bit with DSC: ~9.55 Gbps
- 4K @ 144Hz, 10-bit with DSC: ~14.33 Gbps

## 5K Display Bandwidth Requirements

### 5K (5120 × 2880) at Various Refresh Rates

| Configuration | Raw Bandwidth | With Overhead (20%) | DisplayPort Version Required |
|--------------|---------------|---------------------|----------------------------|
| 5K @ 60Hz, 8-bit | 21.22 Gbps | 25.46 Gbps | DP 1.4 (25.92 Gbps) |
| 5K @ 60Hz, 10-bit | 26.52 Gbps | 31.83 Gbps | DP 2.0 or DP 1.4 with DSC |
| 5K @ 120Hz, 8-bit | 42.44 Gbps | 50.93 Gbps | DP 2.0 |

## 8K Display Bandwidth Requirements

### 8K (7680 × 4320) at Various Refresh Rates

| Configuration | Raw Bandwidth | With Overhead (20%) | DisplayPort Version Required |
|--------------|---------------|---------------------|----------------------------|
| 8K @ 30Hz, 8-bit | 23.88 Gbps | 28.66 Gbps | DP 1.4 with DSC or DP 2.0 |
| 8K @ 60Hz, 8-bit | 47.77 Gbps | 57.32 Gbps | DP 2.0 |
| 8K @ 60Hz, 10-bit | 59.71 Gbps | 71.65 Gbps | DP 2.0 |

## Multiple Display Configurations

### Dual Display Setups

| Configuration | Total Bandwidth Required | Interface Required |
|--------------|-------------------------|-------------------|
| 2× 4K @ 60Hz, 8-bit | 28.66 Gbps | DP 2.0 or 2× DP 1.2 |
| 2× 4K @ 120Hz, 8-bit | 57.32 Gbps | DP 2.0 |
| 2× 5K @ 60Hz, 8-bit | 50.92 Gbps | DP 2.0 |

### Triple Display Setups

| Configuration | Total Bandwidth Required | Interface Required |
|--------------|-------------------------|-------------------|
| 3× 4K @ 60Hz, 8-bit | 42.99 Gbps | DP 2.0 |
| 3× 1440p @ 144Hz, 8-bit | 35.64 Gbps | DP 2.0 |

## DisplayPort Versions and Bandwidth

| DisplayPort Version | Max Bandwidth | Effective Data Rate |
|-------------------|---------------|-------------------|
| DP 1.2 | 21.6 Gbps | 17.28 Gbps |
| DP 1.3/1.4 | 32.4 Gbps | 25.92 Gbps |
| DP 2.0 (UHBR 10) | 40 Gbps | 38.69 Gbps |
| DP 2.0 (UHBR 13.5) | 54 Gbps | 52.22 Gbps |
| DP 2.0 (UHBR 20) | 80 Gbps | 77.37 Gbps |

Note: Effective data rate accounts for 8b/10b encoding (DP 1.x) or 128b/132b encoding (DP 2.0).

## Thunderbolt and DisplayPort Bandwidth Interaction

### Thunderbolt 3/4 (40 Gbps total)

Bandwidth allocation:
- DisplayPort tunneling: Up to 34.56 Gbps (2× DP 1.2 streams)
- Remaining for PCIe: ~5.44 Gbps minimum

Example allocations:
| Display Configuration | DP Bandwidth Used | PCIe Available |
|---------------------|-------------------|----------------|
| 1× 4K @ 60Hz | 17.28 Gbps | 22.72 Gbps |
| 2× 4K @ 60Hz | 34.56 Gbps | 5.44 Gbps |
| 1× 5K @ 60Hz | 25.92 Gbps | 14.08 Gbps |

### Thunderbolt 5 (80 Gbps bi-directional, 120 Gbps uni-directional)

In video mode (120 Gbps output):
- DisplayPort tunneling: Up to 2× DP 2.0 streams
- Can support: 3× 4K @ 144Hz or 2× 8K @ 60Hz

## Daisy-Chaining Effects on Bandwidth

### DisplayPort MST (Multi-Stream Transport)

In daisy-chain configurations, total bandwidth is shared:

**Example: DP 1.4 daisy-chain (25.92 Gbps total)**
- Display 1: 4K @ 60Hz = 17.28 Gbps
- Display 2: 1080p @ 60Hz = 3.96 Gbps
- Remaining: 4.68 Gbps

**Bandwidth degradation in chain:**
1. First display: Full bandwidth available
2. Second display: Bandwidth - First display usage
3. Third display: Bandwidth - (First + Second display usage)

### Thunderbolt Daisy-Chain

**Thunderbolt 3/4 chain (40 Gbps shared):**
- Device 1: External GPU using 32 Gbps PCIe
- Device 2: 4K display limited to remaining 8 Gbps
- Result: Display may be limited to 4K @ 30Hz

**Best practices for daisy-chaining:**
1. Place high-bandwidth devices first in chain
2. Use displays with DSC support
3. Consider bandwidth requirements when planning chain order

## Practical Recommendations

### For 4K Gaming (120Hz+)
- Use DP 2.0 or HDMI 2.1
- Enable DSC if available
- Direct connection preferred over daisy-chain

### For Professional Multi-Display
- Calculate total bandwidth before setup
- Use DP 2.0 hubs for multiple 4K displays
- Consider Thunderbolt 5 for 5K+ configurations

### For Thunderbolt Docks
- Check dock's DisplayPort version
- Verify total bandwidth allocation
- Plan for PCIe devices' bandwidth needs

## Summary Table: Quick Reference

| Display Type | Minimum Interface | Bandwidth Used |
|-------------|------------------|----------------|
| 4K @ 60Hz | DP 1.2 / HDMI 2.0 | 14.33 Gbps |
| 4K @ 120Hz | DP 1.4 + DSC / HDMI 2.1 | 28.66 Gbps |
| 4K @ 144Hz | DP 2.0 / HDMI 2.1 | 34.39 Gbps |
| 5K @ 60Hz | DP 1.4 | 25.46 Gbps |
| 8K @ 60Hz | DP 2.0 | 57.32 Gbps |
| 2× 4K @ 60Hz | DP 2.0 or 2× DP 1.2 | 28.66 Gbps |
| 3× 4K @ 60Hz | DP 2.0 | 42.99 Gbps |