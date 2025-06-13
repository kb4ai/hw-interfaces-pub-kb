# Thunderbolt 3 Bandwidth Technical Analysis

## Raw Bandwidth Specifications

- **Total**: 40 Gbps bidirectional (physical layer)
- **Theoretical PCIe**: 32.4 Gbps (4×PCIe 3.0 lanes)
- **Post-encoding**: 25.92 Gbps (after 8b/10b encoding overhead)
- **Hardcoded PCIe cap**: 22 Gbps (Intel 2016 Technical Brief)
- **Real-world measured**: 22-24 Gbps peak (eGPU community data)

## Bandwidth Allocation Architecture

### DisplayPort Reservation System
- **Always reserved**: ~18 Gbps for DisplayPort protocol
- **Available for PCIe**: 22 Gbps maximum
- **Priority**: DisplayPort takes precedence over PCIe data
- **Dynamic scaling**: Display bandwidth scales with resolution

### Resolution Impact on PCIe Bandwidth

| Display Config | DP Bandwidth | PCIe Available | Total Used |
|----------------|--------------|----------------|------------|
| No display | 0 Gbps | 22 Gbps | 22 Gbps |
| 2.5K@60Hz | ~8 Gbps | 22 Gbps | 30 Gbps |
| 4K@60Hz | ~14 Gbps | 22 Gbps | 36 Gbps |
| 5K@60Hz | ~22 Gbps | 18 Gbps* | 40 Gbps |
| 6K@60Hz | ~31 Gbps | 9 Gbps* | 40 Gbps |

*PCIe bandwidth reduced when total exceeds 40 Gbps

## Technical Implementation Details

### Controller Variations
- **JHL6340/6240**: Standard TB3 controllers
- **JHL7440**: Slightly improved performance (23-24 Gbps measured)
- **Integrated controllers**: May achieve closer to theoretical limits

### Protocol Overhead
- **8b/10b encoding**: 20% overhead (40 Gbps → 32 Gbps usable)
- **DisplayPort 8b/10b**: 25.92 Gbps effective (32.4 Gbps raw)
- **PCIe protocol**: Additional ~2-3 Gbps overhead
- **Flow control**: Bidirectional coordination reduces peak throughput

### Two-Lane vs Four-Lane Designs
- **Four-lane**: Full 22 Gbps PCIe capability
- **Two-lane**: Limited to ~20 Gbps PCIe
- **Implementation**: Host controller dependent

## Real-World Performance Data

### eGPU Measurements
- **RTX 4090**: ~22 Gbps utilized (75-80% native performance)
- **RTX 4070**: ~18 Gbps utilized (85-90% native performance)
- **Gaming workload**: 15-25% performance penalty vs native PCIe

### Storage Benchmarks
- **Single NVMe**: 2.6-2.8 GB/s peak (≈22 Gbps)
- **RAID arrays**: Limited by TB3 bandwidth, not storage
- **Sequential vs random**: Minimal difference in TB3 limitation

### Professional Workloads
- **10GbE networking**: Saturates at ~10 Gbps (within TB3 limits)
- **Audio interfaces**: <100 Mbps (negligible TB3 impact)
- **Video capture**: 4K60 ≈ 12 Gbps (fits within remaining bandwidth)

## Comparison with Competing Standards

| Standard | Raw BW | Usable BW | Display BW | Data BW | Encoding |
|----------|--------|-----------|------------|---------|----------|
| TB3 | 40 Gbps | 32.4 Gbps | 25.92 Gbps | 22 Gbps | 8b/10b |
| TB4 | 40 Gbps | 32.4 Gbps | 25.92 Gbps | 32 Gbps | 8b/10b |
| USB4 | 40 Gbps | 32.4 Gbps | 25.92 Gbps | Variable | 8b/10b |
| TB5 | 80 Gbps | 64 Gbps | 80 Gbps | 64 Gbps | PAM-3 |
| OCuLink | 64 Gbps | 64 Gbps | N/A | 64 Gbps | None |

## Engineering Limitations

### Intel Architecture Decisions
- **18 Gbps DP reservation**: Conservative allocation for 8K future-proofing
- **22 Gbps PCIe cap**: Possibly thermal/power constraints
- **Hardcoded limits**: Prevent protocol conflicts, ensure stability

### Signal Integrity Constraints
- **Cable length**: 0.5m passive (full speed), 2m (reduced performance)
- **Active cables**: Required for >0.5m at full bandwidth
- **Connector quality**: Significant impact on achievable bandwidth

### Thermal Considerations
- **Controller heat**: Higher bandwidth = more heat generation
- **Active cable heat**: Signal conditioning circuits generate heat
- **System throttling**: Thermal limits can reduce practical bandwidth

## Practical Implications

### Workload Optimization
1. **Bandwidth-critical**: Use OCuLink or wait for TB5
2. **Display + eGPU**: TB3 insufficient for 5K+ displays
3. **Multi-device**: Prioritize device connection order
4. **Storage arrays**: Consider direct connection alternatives

### Future-Proofing Considerations
- **TB4 adoption**: Removes 22 Gbps cap but maintains 40 Gbps total
- **TB5 availability**: Limited until 2025-2026
- **USB4 compatibility**: Variable implementation quality
- **OCuLink growth**: Emerging alternative for high-bandwidth needs