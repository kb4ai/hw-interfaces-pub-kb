# OCuLink Implementation Notes

## Radxa Orion O6 Implementation

The Radxa Orion O6 SBC demonstrates modern OCuLink integration:
- **8-lane PCIe v4** via OCuLink adapter
- **Power**: USB-C input (65W cap)
- **Throughput**: 128 Gbps = 16 GB/s
- **Form factor**: Slim 18×18cm case
- **Additional connectivity**: Two M.2 connectors (64 Gbps and 32 Gbps respectively)
- **Reference**: [Announcement PDF](https://dl.radxa.com/orion/o6/radxa_orion_o6_announcement_presentation_2024_12_18.pdf)

Chinese SBCs are rapidly advancing in PCIe expansion capabilities.

## Connector Details: SFF-8611

Interesting discovery: Both 4-lane and 8-lane OCuLink use the **same SFF-8611 connector**.
- Simplifies cable compatibility
- Pin configuration determines lane count
- [Perplexity reference](https://www.perplexity.ai/search/what-is-the-name-of-the-socket-eQS5UtflQYmFLQ1ifonyaA)

## Power Delivery Question

**Open question**: Does OCuLink include 12V power lanes?
- Radxa adapter suggests possible power integration
- Needs further investigation

## GPD G1 eGPU Implementation

The GPD G1 demonstrates mature OCuLink deployment:
- Production-ready eGPU docking station with GPU included
- Full 128 Gbps throughput confirmed
- Compact form factor ("How cute is that")
- Available on mainstream retail ([Amazon listing](https://www.amazon.com/GPD-Docking-Station-Charger-Perfect/dp/B0CHMXSG8Y))
- [Official product page](https://www.gpd.hk/gpdg1graphics)
- [Setup guide](https://gpdstore.net/kb/accessories-support-hub/kb-article/getting-started-with-the-gpd-g1-egpu-docking-station/)

## Key Observations

1. **OCuLink adoption accelerating**: From concept to production deployment
2. **Standardized on high bandwidth**: 128 Gbps becoming common
3. **Compact implementations**: Enabling thin & light eGPU solutions
4. **SBC integration**: No longer limited to laptops

## Technical Advantages Confirmed

"OCuLink FTW" - The standard delivers on its promises:
- True PCIe performance without overhead
- Simpler than Thunderbolt implementations
- Cost-effective for manufacturers