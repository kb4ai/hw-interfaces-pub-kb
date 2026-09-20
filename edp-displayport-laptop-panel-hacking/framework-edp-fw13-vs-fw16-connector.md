# Framework Laptop internal display interface — FW13 vs FW16

Research date: 2026-09-20. All pin data from Framework's own CC BY 4.0 repos.

## Two connectors, not one — the distinction most write-ups miss

The eDP link crosses **two** connectors, with **different part numbers and different
pin orders**. The Framework eDP cable is a *pin-remapping* cable, not a straight-through one.

| End | FW13 | FW16 | Source |
|-----|------|------|--------|
| Mainboard receptacle | **I-PEX 20879-040E** | **I-PEX 20879-040E-01** | FW13 `Mainboard/README.md`; FW16 schematic netlist (`I-PEX_20879-040E-01`, ref `JEDP`) |
| Panel (display) side | **I-PEX 20455-040E** | **I-PEX 20682-040E-02** | FW13 `Display/README.md`; community panel table for BOE NE160QDM-NZ6 |
| Pitch | 0.4 mm | 0.4 mm | community (Gmanny): "Framework uses 0.4mm on both sides" |
| Pin count | 40 | 40 | both |

Both are I-PEX **Cabline-UM** family. Proof that the two ends differ in pin order:
mainboard pins 1–3 are `BL_POWER`, while display-side pins 36–39 are `BL_POWER`.

## Panel-side pinout diff (the published `Display/README.md` tables)

FW16 is **identical to FW13 on every signal that carries video or power**. Only the four
pins that FW13 leaves `NC` are populated:

| Pin | FW13 | FW16 | Note |
|-----|------|------|------|
| 1 | NC | **DDS SCL** | Nvidia DDS I2C clock |
| 34 | NC | **DDS SDA** | Nvidia DDS I2C data |
| 35 | NC | **PSR_EN#** | Panel Self Refresh enable |
| 40 | NC | **OD_EN** | Overdrive enable |

Identical on both: pins 3/4, 6/7, 9/10, 12/13 = `EDP_TXN/TXP` lanes 3,2,1,0 (**4 lanes**);
15/16 = `AUXP`/`AUXN`; 27 = `EDP_HPD`; 18–21 = `3V_EDP` (3.3 V typ.); 22 = `SELF_TEST`;
32 = `BLK_OFF_N`; 33 = `BLK_PWM_LCD` (200 Hz–2 kHz, 3.3 V); 36–39 = `BL_POWER` (5–21 V);
2/5/8/11/14/17 signal GND, 23–26 digital GND, 28–31 backlight GND.

**Conclusion (a):** same connector *family*, same 40-pin 0.4 mm form factor, same mainboard
receptacle part, **identical eDP electrical core**. The FW16 additions are all
sideband/housekeeping: graphics-mux I2C and panel power-saving hints. A carrier/adapter
designed against the FW13 pinout is electrically valid on FW16 for video; only the display-side
mating part number differs (20455 vs 20682).

## Mainboard-side pinout (FW13, `Mainboard/README.md`)

Note the completely different ordering, and that Framework's mainboard connector also
carries a **touchscreen** interface — which is why 40 pins are needed for a 4-lane link:

```
 1–3  BL_POWER (12–17 V)      21    EDP_HPD          31–32 USB_DP / USB_DN
 4    NC                      22    BLK_PWM_LCD      33    TS_EN
 5    GND                     23    BLK_OFF_N        34    TS_RST
 6–7  AUXP / AUXN             24–25 NC               35    TS_INT_N
 8    GND                     26–28 3V_EDP (3VS)     36–37 TS_SDA / TS_SCL
 9–19 EDP_TXP/TXN 0..3 + GND  29–30 3V_TS            38    NC
 20   GND                                            39–40 GND
```

FW16's mainboard side "essentially matches" this; FW16 populates pins 4, 24, 25, 38 that
FW13 lists as NC (community analysis of FW16 schematic p.4 vs FW13 Mainboard README).
FW16 schematic net names confirmed present: `SW_EDP_TXP/N0..3`, `SW_EDP_AUXP/N`, `EDP_HPD`,
`PSR_EN`, `OD_EN`, `TS_EN`, `TS_RST_Q`, `TS_INT#`, `TS_SDA`, `TS_SCL`, `+3V_EDP`, `+3V_TS`.

## FW16 mainboard specifics worth knowing

* The FW16 eDP path runs through a **PS8461EQFN66GTR-A3 DisplayPort mux** that switches
  the internal panel between the APU (`AMD Phoenix / FP7r2`) and the **Graphics Module**
  (dGPU). Net prefix `SW_EDP_*` = *switched* eDP. This is what the DDS I2C pins are for.
  Implication: anything hanging off FW16's internal eDP is downstream of a hybrid-graphics
  mux, with more firmware surface than FW13's direct path.
* **AC coupling is already on the mainboard**: `0.22 µF` series caps on each main-link pair
  (`CV1`–`CV8`), `0.1 µF` on AUX (`CV454`/`CV455`). Per DP 1.2 spec, main-link caps are
  required at the *transmitter* end — so Framework's eDP pins are already DP-spec-compliant
  AC-coupled differential pairs at the connector.
* HPD has a `1M` bleed and a `100K` divider network on the mainboard side.

## Panels

| | FW13 | FW16 |
|---|---|---|
| Stock panel | BOE NE135FBM-N41 (1920×1280), NE135A1M-NY1 (2.8K) | BOE **NE160QDM-NZ6** (2560×1600) |
| Lanes / version | 4-lane | **4-lane, eDP 1.4b** |
| Backlight | — | 11S8P, ~12 V typ., 6.95 W |
| Panel-side conn | I-PEX 20455-040E | I-PEX 20682-040E-02 |

FW16 panel datasheet is **not public** — community has repeatedly failed to obtain the
display-side FW16 pinout; Framework has published only the mainboard side plus the
`Display/README.md` table.

## References

* [Framework-Laptop-13/Display — pinout + IPEX 20455-040E](https://github.com/FrameworkComputer/Framework-Laptop-13/tree/main/Display)
* [Framework-Laptop-16/Display — pinout + "Diff vs Laptop 13" column](https://github.com/FrameworkComputer/Framework-Laptop-16/tree/main/Display)
* [Framework-Laptop-13/Mainboard — Display Interface, IPEX 20879-040E](https://github.com/FrameworkComputer/Framework-Laptop-13/tree/main/Mainboard#display-interface)
* [FW16 Mainboard Interfaces Schematic (PDF)](https://github.com/FrameworkComputer/Framework-Laptop-16/blob/main/Mainboard/Mainboard_Interfaces_Schematic_Framework_Laptop_16_7040_Series.pdf) — p.4 eDP, block diagram p.1 (PS8461 mux)
* [Framework Community — Swapping 13in Display Cable For 16in Display Cable](https://community.frame.work/t/swapping-13in-display-cable-for-16in-display-cable/73781)
* [Framework Community — Framework 16 Screen Compatability](https://community.frame.work/t/framework-16-screen-compatability/69385) — panel table incl. I-PEX 20682-040E-02
* [Hackaday — DisplayPort: Hacking And Examples](https://hackaday.com/2024/05/16/displayport-hacking-and-examples/) — AC-coupling values, AUX biasing, HPD pulldown
