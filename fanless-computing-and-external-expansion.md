# Fanless Computing and External Expansion

## The Fanless Philosophy

Fanless systems prioritize **zero acoustic noise** and **reliability** through passive cooling, eliminating mechanical failure points while creating distraction-free computing environments.

## Premium Fanless Chassis

### HDPLEX H3
- **TDP**: 85W passive cooling
- **Size**: 302×320×114mm (9.2L)
- **Expansion**: Low-profile PCIe slots
- **Build**: 6063T aluminum, 8× copper heatpipes
- **Price**: ~$300-400

### Streacom DB4
- **TDP**: 65W standard, 100W+ modified
- **Size**: 260×260×80mm (5.4L)
- **Design**: Cube form factor
- **Cooling**: Massive aluminum fins

### Akasa Turing
- **TDP**: 35W (perfect for efficient CPUs)
- **Size**: Ultra-compact
- **Target**: NUC-style builds

## Fanless + External GPU Strategy

### The Perfect Combination
1. **Silent daily driver**: Fanless system for 95% of tasks
2. **On-demand power**: eGPU via TB/OCuLink when needed
3. **Acoustic isolation**: GPU noise in separate enclosure
4. **Thermal separation**: Heat sources physically separated

### Implementation Approaches

**Option 1: Thunderbolt Native**
- Motherboard: ASRock Z790 PG-ITX/TB4
- CPU: Intel i5-13400T (35W TDP)
- Chassis: HDPLEX H3
- eGPU: Via TB4 when needed

**Option 2: OCuLink Integration**
- Board: AsRock X570D4I-2T (OCuLink header)
- CPU: AMD 5700G (65W cTDP)
- Direct OCuLink to external GPU
- 64 Gbps bandwidth, zero fan noise

**Option 3: Hybrid Approach**
- Fanless low-profile GPU (RTX A2000)
- External high-end GPU for demanding tasks
- Best of both worlds

## Fanless GPU Options

### Truly Passive GPUs
- **Palit KalmX GTX 1650**: 75W passive
- **Sapphire Nitro+ RX 6400**: Semi-passive mode
- **RTX A2000**: With aftermarket heatsink

### Semi-Passive Solutions
- **ASUS 0dB GPUs**: Fan stop under 55°C
- **MSI Gaming X**: Zero Frozr technology
- **Gigabyte Windforce**: Fan stop mode

## Thermal Design Considerations

### CPU Selection for Fanless
| TDP Class | Recommended CPUs | Use Case |
|-----------|------------------|----------|
| 15W | Intel U-series, AMD U | Ultra-silent office |
| 35W | Intel T-series, AMD GE | Balanced performance |
| 65W | Ryzen 7000 eco mode | Development work |
| 85W+ | i7/i9 undervolted | Professional workloads |

### Case Airflow Without Fans
- **Convection optimization**: Vertical fin orientation
- **Chimney effect**: Bottom intake, top exhaust
- **Heat spreading**: Large surface area critical
- **Ambient consideration**: Room temperature matters

## Fanless Workstation Builds

### Development Machine
```
Chassis: HDPLEX H3
CPU: Ryzen 7 7700 (65W eco mode)
RAM: 64GB DDR5
Storage: 2× NVMe (heatsinks required)
GPU: None (APU graphics)
Expansion: TB4 card for eGPU
Result: Silent coding environment
```

### Content Creation
```
Chassis: Streacom DB4
CPU: Intel i7-13700T (35W)
GPU: Palit KalmX GTX 1650
Storage: 4× SSD RAID
OCuLink: To RTX 4090 eGPU
Result: Silent edit, powerful render
```

## Fanless + eGPU Best Practices

### Thermal Management
1. **Undervolt aggressively**: Lower voltage = less heat
2. **Quality thermal interface**: Liquid metal for CPU
3. **Ambient control**: AC room helps significantly
4. **Monitoring**: Watch temps during summer

### Expansion Strategy
1. **Keep fanless system minimal**: Don't push TDP limits
2. **Offload heavy compute**: Use eGPU for demanding tasks
3. **Storage over network**: NAS for bulk storage
4. **Modular approach**: Easy to upgrade external components

### Cable Management
- **OCuLink**: Shortest cable possible (heat)
- **Thunderbolt**: Active cables for distance
- **Power**: Separate circuits for eGPU
- **Organization**: Clean routing maintains airflow

## The Ultimate Fanless Setup

### System Architecture
```
Fanless PC (HDPLEX H3)
    ├── TB4/OCuLink ports
    ├── 10GbE to NAS (fanless switch)
    ├── Wireless peripherals
    └── External PSU (fanless)

Connected Devices (isolated)
    ├── eGPU enclosure (in closet/other room)
    ├── NAS (in network cabinet)
    └── Backup drives (passive cooling)
```

### Benefits Achieved
- **Zero noise**: Complete silence at desk
- **Full power**: High-end GPU available
- **Reliability**: No moving parts in main system
- **Flexibility**: Upgrade GPU independently
- **Thermal isolation**: Heat sources separated

## Future of Fanless Computing

### Emerging Technologies
- **3D vapor chambers**: Better heat spreading
- **Graphene TIM**: Superior thermal transfer
- **Chiplet designs**: Distributed heat sources
- **AI power management**: Smarter thermal control

### Integration Opportunities
- **TB5 adoption**: 240W power + data over one cable
- **Wireless displays**: Eliminate more cables
- **Cloud computing**: Offload to remote servers
- **Edge AI**: Local inference without fans

## Recommended Resources

### Communities
- **SFF Forum**: Fanless build section
- **HDPLEX Users**: Build galleries
- **Silent PC Review**: Acoustic measurements

### Suppliers
- **HDPLEX**: Premium fanless chassis
- **Streacom**: Design-focused cases
- **Noctua**: Passive CPU coolers
- **Akasa**: Fanless accessories

The intersection of fanless computing and external expansion represents the pinnacle of flexible, silent computing - maintaining a peaceful work environment while retaining access to unlimited computational power on demand.