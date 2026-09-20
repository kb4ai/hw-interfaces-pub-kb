# Framework lineup as of 2026-09-20 — and the LPCAMM2 premise correction

## ⚠️ Correction: "new type of RAM, flat mounted" is **Framework Laptop 13 Pro**, not FW16

**LPCAMM2 is not on the Framework Laptop 16.** FW16 still uses **2× DDR5 SO-DIMM sockets**.
The flat-mounted LPCAMM2 LPDDR5X module arrived on the **Framework Laptop 13 Pro**
(announced 2026-04-21, shipping June 2026) — Framework's *third* memory platform after
DDR4 (11th-gen Intel) and DDR5 SO-DIMM.

Nirav Patel's LPCAMM2 deep dive (2026-06-02) frames it as efficiency + bandwidth via
LPDDR5x **while keeping upgradeability** — the thing soldered LPDDR normally costs you.

## Current lineup

| | **Framework Laptop 13 Pro** | **Framework Laptop 16** | Framework Laptop 13 | Framework Laptop 12 |
|---|---|---|---|---|
| Released | **2026** | 2025 (Ryzen AI 300 refresh) | ongoing | 2025 |
| From | $1,199 DIY / $1,499 built | $1,249 DIY / $1,599 built | — | — |
| CPU | Intel **Core Ultra Series 3** (Panther Lake): Ultra 5 325, Ultra X7 358H, Ultra X9 388H | **AMD Ryzen AI 300**: AI 5 340 / AI 7 350 / AI 9 HX 370 (also older 7040) | Intel Core Ultra / Ryzen AI | — |
| **Memory** | ⭐ **LPCAMM2 LPDDR5X**, up to **64 GB**, 6800 MT/s (U5 325) / **7467 MT/s** (X7/X9) | **2× DDR5 SO-DIMM**, up to **96 GB DDR5-5600** | 2× SO-DIMM | — |
| iGPU | Intel Arc B390 (12 Xe-cores) on X7/X9 | Radeon 860M / 890M | — | — |
| dGPU | none | ⭐ **RTX 5070 Laptop 8/12 GB GDDR7**, 100 W TGP, 798 AI TOPS; or RX 7700S (2nd gen) | none | none |
| Display | 13.5" 3:2 **2880×1920 touch**, 700 nit, **30–120 Hz**, DC dimming, per-panel calibration | 16" 16:10 **2560×1600**, **165 Hz**, VRR/FreeSync (**G-Sync** w/ RTX 5070 + Display Kit 2nd gen), 500 nit, 100% DCI-P3, 1500:1, 9 ms | 13.5" 3:2 | 12.2" |
| Battery | 74.45 Wh, ~20 h claimed | 85 Wh | 61 Wh | — |
| Chassis | **full CNC 6063 aluminum** (top/input/bottom), new hinge | CNC Al top + thixomolded Mg bottom, **155° hinge, 6.1 kg force** | — | 2-in-1 |
| Ports | 4× Expansion Card, all **TB4 / DP 2.1 / PD up to 140 W** | **6× Expansion Card**; slots 1&4 USB4 + DP 2.1 UHBR10; slot 2 USB 3.2 + DP 2.1 UHBR10; slot 5 DP 1.4 HBR3; slots 3&6 USB 3.2 only | 4× | 4× |
| Expansion Bay | — | ⭐ **PCIe x8** | — | — |
| Touchpad | **piezo haptic** (Boréas drivers, 4 elements) | Precision or haptic (one-piece option) | — | — |
| Weight | 1.39–1.44 kg | 2.10 kg (shell) / **2.40 kg** (GPU module) | 1.3 kg | — |

## So — is FW16 "stronger hardware"?

**For GPU and expansion, unambiguously yes:** it is the only Framework with a discrete GPU
(RTX 5070, 100 W TGP), a **PCIe x8 Expansion Bay**, 6 Expansion Card slots, and up to 96 GB
RAM. **For CPU, memory bandwidth, battery life, and portability, FW13 Pro now wins** —
Panther Lake + LPCAMM2 at 7467 MT/s, 20 h claimed, 1.4 kg vs 2.4 kg.

## Relevance to an e-ink lid project

Points **toward FW13 / FW13 Pro**, not FW16:

1. **Published mechanical CAD.** FW13 `Display/` ships a 2D PDF drawing, three DXFs, panel
   STEP, and `FW_display_w_cable_bracket.stp`. FW16 `Display/` contains **only a README** —
   no drawing, no DXF, no STEP for the panel. A FW16 e-ink lid means deriving the envelope
   from whole-system CAD zips.
2. **Simpler display path.** FW16's internal eDP runs through a **PS8461 DisplayPort mux**
   switching between APU and Graphics Module (hence the FW16-only DDS I2C pins). FW13 is a
   direct path. Fewer moving parts for a mod.
3. **A dGPU is dead weight** for an e-ink writing machine, and costs ~1 kg and battery life.
4. **Port budget.** FW16 has 6 Expansion Card slots vs FW13's 4 — so if the Glider must eat
   a USB-C port, FW16 absorbs that more comfortably. This is FW16's one genuine advantage here.
5. Precedent mass is on FW13: the dual-OLED custom-PCB build, the 701C transplant, the
   tablet conversions, the Hackaday USB-C→eDP board all target FW13-family mainboards.

## References

* [Framework — Introducing Framework Laptop 13 Pro (2026-04-21)](https://frame.work/blog/introducing-framework-laptop-13-pro)
* [Framework — Laptop 13 Pro Deep Dive: LPCAMM2 (2026-06-02)](https://frame.work/blog/framework-laptop-13-pro-deep-dive-lpcamm2)
* [Framework Laptop 13 Pro — specs](https://frame.work/laptop13pro?tab=specs)
* [Framework Laptop 16 — specs](https://frame.work/laptop16?tab=specs)
* [Framework — Introducing the new Framework Laptop 16 with NVIDIA](https://frame.work/blog/introducing-the-new-framework-laptop-16-with-nvidia)
* [TechPowerUp — Framework Laptop 13 Pro](https://www.techpowerup.com/348410/framework-laptop-13-pro-brings-core-ultra-series-3-cpus-lpcamm2-memory-and-improved-battery-life)
* [VideoCardz — Framework Laptop 13 Pro launches with LPCAMM2 and Panther Lake](https://videocardz.com/newz/framework-laptop-13-pro-launches-with-lpcamm2-memory-and-core-series-3-panther-lake-series-ultra-x9-already-sold-out)
