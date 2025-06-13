# Thunderbolt 3, 4, and 5 Technical Specifications Comparison

## Thunderbolt 3

### Raw Bandwidth/Throughput
- **Total bandwidth**: 40 Gbps bidirectional
- **Data throughput**: ~22 Gbps peak after overhead
- **Video reservation**: ~8 Gbps reserved for video
- **Non-video data**: 32 Gbps available

### PCIe Lanes and Generation
- **PCIe Generation**: PCIe 3.0
- **Lane configurations**:
  - 4-lane design: Up to 4 lanes of PCIe 3.0 (32.4 Gbps)
  - 2-lane design: 2 lanes of PCIe 3.0 (16 Gbps)
- **Note**: Systems can be configured with either 2 or 4 PCIe lanes

### DisplayPort Bandwidth
- **DisplayPort version**: DP 1.2 (mandatory), DP 1.4 (optional)
- **DisplayPort lanes**: 4 lanes of DisplayPort 1.4 HBR3 (25.92 Gbps after 8/10 encoding)
- **Display support**: Up to two 4K displays at 60Hz

### Power Delivery
- **To devices**: Up to 15W for bus-powered accessories
- **Laptop charging**: Up to 100W (with USB Power Delivery support)

### Daisy-Chaining
- **Maximum devices**: Up to 6 Thunderbolt devices per port
- **Limitations**: 
  - Display should be at the end of the chain if it doesn't support daisy-chaining
  - Chain terminates if USB or DisplayPort device is connected directly

### Cable Requirements
- **Connector**: USB-C
- **Active cables**: Required for maximum performance over 0.5 meters

## Thunderbolt 4

### Raw Bandwidth/Throughput
- **Total bandwidth**: 40 Gbps bidirectional (same as TB3)
- **Minimum PCIe requirement**: 32 Gbps (doubled from TB3's 16 Gbps minimum)
- **Data throughput**: ~22 Gbps peak after overhead

### PCIe Lanes and Generation
- **PCIe Generation**: PCIe 3.0
- **PCIe bandwidth**: 32 Gbps minimum (4 lanes × 8 Gbps)
- **Key improvement**: Mandatory 32 Gbps PCIe vs optional in TB3

### DisplayPort Bandwidth
- **DisplayPort support**: DP 1.4 (up to DP 2.0 supported)
- **Display capabilities**: 
  - Two 4K displays at 60Hz
  - One 8K display at 30Hz

### Power Delivery
- **To devices**: Up to 15W for bus-powered accessories
- **Laptop charging**: Up to 100W

### Daisy-Chaining
- **Maximum devices**: Up to 6 Thunderbolt devices per port
- **New feature**: Support for Thunderbolt hubs (Multi-port Accessory Architecture)
- **Display support**: Better multi-display support than TB3

### Additional Features
- **Security**: Intel VT-d-based DMA protection
- **Wake from sleep**: Supported
- **Cable length**: Up to 2 meters (6.5 feet) with full performance

## Thunderbolt 5

### Raw Bandwidth/Throughput
- **Bidirectional bandwidth**: 80 Gbps (double TB4)
- **Bandwidth Boost mode**: Up to 120 Gbps for display-intensive tasks
  - 120 Gbps transmit / 40 Gbps receive (asymmetric mode)
- **Lane configuration**: 4 lanes at 40 Gbps each

### PCIe Lanes and Generation
- **PCIe Generation**: PCIe 4.0
- **PCIe bandwidth**: 64 Gbps (double TB4)
- **Interface**: PCIe Gen 4 x4

### DisplayPort Bandwidth
- **DisplayPort version**: DP 2.1 (UHBR20)
- **Video bandwidth**: 80 Gbps for video
- **Display capabilities**:
  - Three 4K displays at 144Hz
  - Two 8K displays at 60Hz
  - One 8K display at 120Hz
  - Support for 4K at 540Hz

### Power Delivery
- **Maximum power**: Up to 240W (USB PD 3.1)
- **Significant upgrade**: More than double TB3/TB4's 100W limit

### Daisy-Chaining
- **Compatibility**: Maintains support for up to 6 devices
- **Backward compatibility**: Works with TB3, TB4, and USB4 devices

### Technical Implementation
- **Signaling**: PAM-3 (Pulse Amplitude Modulation with 3 levels)
- **Cable compatibility**: Works with existing TB4 passive cables up to 1 meter
- **Connector**: USB-C (same as previous generations)

## Summary Comparison Table

| Specification | Thunderbolt 3 | Thunderbolt 4 | Thunderbolt 5 |
|--------------|---------------|---------------|---------------|
| Total Bandwidth | 40 Gbps | 40 Gbps | 80 Gbps (120 Gbps boost) |
| PCIe Generation | PCIe 3.0 | PCIe 3.0 | PCIe 4.0 |
| PCIe Bandwidth | 16-32 Gbps | 32 Gbps (min) | 64 Gbps |
| DisplayPort | DP 1.2/1.4 | DP 1.4/2.0 | DP 2.1 |
| Power Delivery | 100W | 100W | 240W |
| Max Displays | 2× 4K@60Hz | 2× 4K@60Hz | 3× 4K@144Hz or 2× 8K@60Hz |
| Daisy Chain | 6 devices | 6 devices + hubs | 6 devices |
| Security | Basic | VT-d DMA protection | VT-d DMA protection |