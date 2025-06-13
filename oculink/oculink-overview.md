# OCuLink Overview

## What is OCuLink?

**OCuLink (Optical-Copper Link)** is an external PCIe connector standard that provides direct PCIe connectivity over cable, similar to how eSATA extends SATA externally.

## Technical Specifications

- **Version 1.0** (October 2015): Up to 4 PCIe 3.0 lanes (3.9 GB/s)
- **OCuLink-2**: Up to 16 GB/s (PCIe 4.0 ×8) = 128 Gbps
- **Connector**: SFF-8611 (same for both 4-lane and 8-lane variants)
- **Open standard**: No licensing fees required

## Key Advantages

1. **Direct PCIe**: No protocol conversion overhead (unlike Thunderbolt)
2. **Lower latency**: Direct connection without translation layers
3. **Higher bandwidth**: OCuLink-2 delivers 16 GB/s vs Thunderbolt 4's 10 GB/s
4. **Cost-effective**: No licensing fees, simpler implementation

## Linux Support

OCuLink requires **no special drivers** - it appears as standard PCIe to the OS:

### GPU Compatibility
- **AMD**: Best support via built-in `amdgpu` kernel driver
- **NVIDIA**: Improving with open-source kernel modules (R515+)
  - Requires `Option "AllowExternalGpus" "True"` in Xorg
- **Intel Arc**: Mixed reports, some compatibility issues

### Setup Requirements
- Standard PCIe device drivers work as-is
- PRIME for GPU switching with open-source drivers
- Same kernel support as internal PCIe devices

## Common Use Cases

1. **External GPUs (eGPUs)**: Primary use in laptops
2. **SBC Expansion**: Adding PCIe devices to single-board computers
3. **Storage Arrays**: High-speed external NVMe storage
4. **Server Interconnects**: Original design intent

## Comparison with Alternatives

| Interface | Max Bandwidth | Protocol | Licensing |
|-----------|--------------|----------|-----------|
| OCuLink-2 | 16 GB/s | Native PCIe | Open |
| Thunderbolt 4 | 10 GB/s | PCIe tunneled | Proprietary |
| USB4 | 10 GB/s | PCIe tunneled | Open* |

*USB4 includes Thunderbolt 3 compatibility

## Future Outlook

- Growing adoption in SBCs (Radxa, GPD devices)
- Potential for higher PCIe generations
- Increasing use in compact computing solutions