# eGPU Chassis Selection Guide

## Understanding GPU Compatibility

**Key Insight**: All GPUs are Thunderbolt compatible because all GPUs support downgrading to 4-lane PCIe 3.0. There's no GPU that can't handle this - it's a fundamental PCIe feature. The real compatibility factors are:
- **Chassis wattage**: Must handle GPU power requirements
- **Power supply quality**: Some GPUs have voltage spikes on boot
- **Physical dimensions**: GPU must fit in the enclosure
- **Cooling capacity**: Chassis must dissipate GPU heat

## Critical Selection Criteria

### 1. Power Supply Wattage

The most important specification for chassis selection:

| GPU Class | Minimum PSU | Recommended PSU | Examples |
|-----------|-------------|-----------------|-----------|
| Entry (75W) | 250W | 350W | GTX 1650, RX 6400 |
| Mid-range (150W) | 400W | 500W | RTX 4060, RX 7600 |
| High-end (250W) | 550W | 650W | RTX 4070 Ti, RX 7800 XT |
| Enthusiast (350W) | 650W | 750W | RTX 4080, RX 7900 XTX |
| Flagship (450W+) | 750W | 850W+ | RTX 4090, upcoming 5090 |

**Why overhead matters**:
- PSU efficiency curve (best at 50-80% load)
- Voltage spike handling
- Future upgrade headroom
- System stability under load

### 2. Power Delivery Quirks

Some GPUs have specific power requirements:

**Voltage Spikes on Boot**:
- Older NVIDIA cards (GTX 10-series) could spike 50W+ over TDP during initialization
- AMD Vega cards notorious for transient spikes
- Solution: Chassis with robust PSU and good capacitors

**Multi-Rail vs Single-Rail**:
- High-end GPUs prefer single-rail PSUs
- Avoid chassis with split 12V rails for 300W+ GPUs

### 3. Physical Dimensions

**GPU Length**:
- Compact chassis: Up to 280mm (2-slot cards)
- Standard chassis: Up to 320mm (most cards)
- Large chassis: 350mm+ (flagship cards)

**GPU Height**:
- Standard: 115mm (reference height)
- Tall cards: 140mm+ (custom coolers)
- Check clearance from PCIe slot to chassis wall

**Slot Width**:
- 2-slot: Most common
- 2.5-slot: High-end cards
- 3-slot: Flagship models
- 4-slot: Extreme cooling solutions

### 4. Cooling Considerations

**Airflow Design**:
- **Open chassis**: Better temps, more noise
- **Closed chassis**: Quieter, needs good ventilation
- **Hybrid**: Perforated panels, best of both

**Fan Configuration**:
- Intake fans: Essential for closed designs
- Exhaust assistance: Helps with hot air removal
- GPU fan clearance: Minimum 20mm recommended

**Thermal Capacity**:
- Entry chassis: 150W continuous cooling
- Standard: 250W continuous
- Premium: 350W+ continuous

### 5. Build Quality Indicators

**Frame Material**:
- Steel: Durable, heavy, good EMI shielding
- Aluminum: Light, good thermals, premium feel
- Plastic: Avoid for high-end GPUs

**PCIe Slot Quality**:
- Reinforced slots for heavy GPUs
- Secure mounting mechanism
- Anti-sag brackets for 1kg+ cards

**Cable Quality**:
- Thunderbolt cable included?
- Cable length (0.5m vs 0.7m)
- Active vs passive cables

### 6. Additional Features

**Worth Having**:
- Carrying handle (portability)
- Quick-release mechanism
- Dust filters
- Status LEDs
- Daisy-chain port

**Nice to Have**:
- USB hub (but problematic on Linux)
- Ethernet (often buggy)
- RGB lighting (personal preference)
- SD card reader

### 7. Known Chassis-Specific Issues

**Razer Core X**:
- Excellent GPU compatibility
- PSU fan can be noisy
- No USB/Ethernet (actually a plus for Linux)

**Razer Core X Chroma**:
- USB controller conflicts on Linux
- Ethernet unstable
- RGB software Windows-only

**Akitio Node**:
- Basic but reliable
- PSU barely adequate for high-end
- Excellent Linux compatibility

**Mantiz Venus**:
- Compact design limits GPU size
- Good build quality
- 5-port USB works on Linux

**Sonnet eGFX Breakaway Box**:
- Professional grade
- Quiet operation
- Premium price

## Selection Decision Tree

```
1. What's your GPU TDP?
   ├── Under 200W → 400-500W chassis adequate
   ├── 200-300W → 650W chassis recommended
   └── Over 300W → 750W+ chassis required

2. GPU physical size?
   ├── Reference size → Any chassis works
   ├── Large custom cooler → Check dimensions
   └── Triple-slot → Need spacious chassis

3. Usage pattern?
   ├── Desktop replacement → Prioritize cooling
   ├── Portable setup → Weight and handle important
   └── Quiet operation → Closed design with good fans

4. Linux user?
   ├── Yes → Avoid chassis with integrated USB/Ethernet
   └── No → Additional features may be useful
```

## Recommended Chassis by Use Case

### Best Overall (Linux)
**Razer Core X** (non-Chroma)
- 650W PSU handles most GPUs
- No problematic USB controller
- Good thermal design
- Reasonable price

### Budget Option
**Akitio Node**
- 400W adequate for mid-range
- Barebones but functional
- Proven Linux compatibility
- Upgrade PSU possible

### Premium Choice
**Sonnet eGFX Breakaway Box 650/750**
- Professional build quality
- Quiet operation
- Tool-less installation
- Extended warranty

### Compact Setup
**Akitio Node Lite**
- PCIe slot powered only (75W)
- Extremely portable
- Perfect for entry GPUs
- Silent operation

### Future-Proof (TB5)
**Wait for 2025 releases**
- ASUS ROG XG Mobile 2025
- Razer Core X2 (rumored)
- OWC Mercury Helios 4

## Pre-Purchase Checklist

- [ ] PSU wattage ≥ GPU TDP + 150W
- [ ] Physical dimensions accommodate GPU
- [ ] Cooling adequate for continuous load
- [ ] Linux compatibility verified (if applicable)
- [ ] Thunderbolt cable included and adequate length
- [ ] Warranty and support availability
- [ ] Reviews mention stability with similar GPU
- [ ] Price includes necessary accessories

## Final Recommendations

1. **Don't cheap out on PSU wattage** - It's the most common failure point
2. **Measure your GPU** - Many compatibility issues are simple size mismatches
3. **Read Linux-specific reviews** - Windows compatibility doesn't guarantee Linux success
4. **Consider future GPUs** - Buy chassis for the GPU you'll upgrade to
5. **Verify return policy** - In case of unexpected incompatibilities