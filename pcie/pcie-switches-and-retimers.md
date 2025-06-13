# PCIe Switches and Retimers - Technical Reference

## PCIe Gen4 Switch - Microchip Switchtec PM40084

### Technical Specifications
- **PCIe Generation**: Gen4 (16 GT/s per lane)
- **Total Lanes**: 84 lanes
- **Total Ports**: 44 ports
- **Virtual Switch Partitions**: 22
- **Non-Transparent Bridges (NTBs)**: 40
- **Theoretical Maximum Bandwidth**: ~168 GB/s bidirectional
- **Per Lane Speed**: 16 GT/s (15.75 Gbps effective after 128b/130b encoding)

### C-Payne Implementation Board
- **Input**: 2× SlimSAS 8i PCIe Gen4 (1 mandatory)
- **Output**: 4× PCIe Gen4 x16 slots (101.6mm spacing)
- **Auxiliary**: 1× PCIe Gen4 x4 SlimSAS for NIC/NVMe
- **Power**: 4 EPS connectors (2 mandatory)
- **Price**: €1,250.00 EUR

### Key Features
- Hot-plug and surprise-plug controllers per port
- Advanced error containment and isolation
- Real-time eye capture for signal integrity analysis
- Integrated MIPS processor for management
- Hardware-based configuration simplicity

### Use Cases
- Multi-GPU systems (4× large consumer GPUs)
- Data center high-density PCIe expansion
- Professional GPU compute clusters
- NVMe JBOF implementations
- PCIe device validation platforms

### Platform Compatibility
- ✓ AMD EPYC, Threadripper, AM5 platforms
- ✗ Intel mainstream platforms (enumeration issues)

## PCIe Gen5 Retimer Technology

### What is a PCIe Retimer?
Mixed-signal device that fully recovers data, extracts embedded clock, and retransmits fresh signal - unlike redrivers that simply amplify.

### Core Components
- **CTLE** (Continuous Time Linear Equalizer) - Initial signal conditioning
- **CDR** (Clock and Data Recovery) - Extracts clock, eliminates jitter
- **DFE** (Decision Feedback Equalizer) - Removes reflections/crosstalk
- **TX FIR Driver** - Adaptive pre-emphasis for downstream channel

### Benefits for PCIe Gen5/4/3/2/1
1. **Signal Regeneration**: Complete eye diagram restoration
2. **Jitter Reset**: Eliminates accumulated jitter (critical for Gen5's 0.15 UI RMS tolerance)
3. **Insertion Loss Reset**: Resets 36dB loss budget, enabling 2x reach
4. **Crosstalk Elimination**: DFE removes crosstalk after CDR sampling
5. **Protocol Awareness**: Participates in PCIe LTSSM, adapts automatically

### Enabling Longer Traces/Cables
- Without retimer: Gen5 limited to 10-15 inches PCB trace
- With retimer: 20-30+ inches possible, 1-2 meter cables feasible
- Each retimer creates new electrical segment with fresh budgets

### Fixing Unstable Links
- Dynamic equalization adaptation to channel conditions
- Temperature/voltage variation compensation
- Manufacturing tolerance accommodation
- Active protocol participation with proper timeouts

## MCIO PCIe Gen5 Host Adapter with Retimer

### MCIO Connector Specifications
- **74-pin variant**: Most common for x8 PCIe lanes
- **Electrical**: Supports Gen1-5 (2.5-32 GT/s), ready for Gen6 PAM-4
- **Based on**: SFF-TA-1009 specification
- **Features**: High-density, robust latching, 85Ω impedance

### C-Payne MCIO Host Adapter
- **Input**: PCIe x16 slot
- **Output**: 2x MCIO 74-pin (8i) connectors
- **Modes**: x16, x8x8, x8x4x4, x4x4x4x4
- **Weight**: 300g
- **Price**: €240.00

### Retimer Options
- **ASTERA**: USB-I2C configuration, CXL support
- **KANDOU**: DIP switch configuration, CXL support

### Key Features
- Re-encodes all PCIe generations (1-5)
- Must match BIOS bifurcation settings
- Includes 3D CAD files for integration

## References

1. Microchip Switchtec PM40084 Product Page
2. C-Payne PCIe Gen4 Switch Implementation: https://c-payne.com/products/pcie-gen4-switch-4x-x16-microchip-switchtec-pm40084
3. C-Payne MCIO PCIe Gen5 Host Adapter: https://c-payne.com/products/mcio-pcie-gen5-host-adapter-x16-retimer
4. SFF-TA-1009 MCIO Specification
5. PCIe 5.0 Base Specification
6. ASTERA Labs Retimer Documentation
7. KANDOU Bus Retimer Technical Brief