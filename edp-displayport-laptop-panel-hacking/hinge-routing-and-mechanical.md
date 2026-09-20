# Framework lid↔base cable routing, and the mechanical side of a Glider-in-lid build

Research date: 2026-09-20.

## What Framework already routes across the hinge

Framework's own guides document a **shared cable channel** beside/through the hinge that
already carries multiple cables — this is documented, not inferred:

* **FW13** (`Hinge Replacement Guide`): the Display Cable is freed from the Bottom Cover and
  routing pegs; **silver grounding tape** holds the Display Cable to the hinge; antenna
  cables stick down onto the hinge.
* **FW16** (`eDP Cable` guide, step 18): *"Route the eDP Cable (Display) from the Display
  through the **left hinge** placing it in the **rear guided channel**."*
* **FW16** (iFixit *Hinges Replacement*, step 26): *"Lift the **eDP, webcam, and antenna
  cables** out of their **vertical slot** in the Bottom Cover."* → the slot carries
  **4 cable runs** (eDP + webcam + 2× antenna).
* **FW16** (iFixit *eDP Cable Replacement*, steps 23–25): eDP cable lifted from its vertical
  slot, guided out of **clips along the top left edge** of the Bottom Cover, and is
  **lightly adhered to the Top Cover at its "bend"** near the bottom-left corner.

**Precedent for extra cables: yes, in the sense that the channel is multi-cable and
clip/adhesive-retained rather than a sealed conduit.** No published dimension for free
cross-section was found — see gaps.

Known friction point: the FW13 **webcam cable is too short for the specified over-hinge
routing** on some units, and owners route it *under* the hinge instead, causing a visible
bulge where it is pressed between hinge and cover. That is direct evidence the channel is
**tight and already near capacity**.

## Option A — Glider in the BASE, panel FFC through the hinge

* Moves the 20 W-peak e-paper PSU and the FPGA's heat out of the lid.
* But: the panel-side interface is a **39-pin 0.3 mm FFC** carrying the raw parallel EPD
  interface — many single-ended signals at TCON rates. Routing that through a **moving
  hinge** is the worst case for both flex life and signal integrity, and it is far more
  fragile than a shielded differential cable.
* Framework's own design puts the *differential* link across the hinge and the panel
  electronics in the lid. Inverting that is against the grain.

## Option B — Glider in the LID, USB-C across the hinge ⭐

* The hinge then carries a **USB-C cable** (4 differential pairs + CC + VBUS) instead of a
  39-conductor parallel bus. Electrically much friendlier and mechanically a known quantity.
* Costs one external USB-C port (cable exits the chassis and loops back in, or is routed
  internally to an Expansion Card bay).
* This is exactly what existing Framework display mods do.

### Precedent for USB-C-fed displays on Framework

* **`hanhanhan-kim/framework_tablet`** — 3D-printable tablet case; *"To connect the display
  to the Motherboard, you will need a USB C 3.1"* cable providing power, data and video.
* **Dakota's dual-OLED FW13 build log** (2026-04) — both ZenBook Duo OLED panels driven from
  **USB-C ports** via a custom PCB (CH224K PD sink per port + **RTD2173N DP→eDP bridge** per
  panel + TPS61088 boost). Explicitly rejected the internal eDP connector: *"The FW13's
  internal eDP connector is LCD-only in its pinout."* Power budget: PD gives 15 W + 7.5 W =
  22.5 W across two ports; he considered supplementing from the eDP connector's `3V_EDP`
  pins (4 pins, ~4 W) and `BL_POWER` (**0.3 A × 4 pins = 1.2 A**, i.e. ~12 W at 10 V —
  *connector rating, not an IC limit*).
* **Dual Screen Framework 13** thread frames the correct mental model:
  *"Don't treat it like a second internal display — treat it like an external display that's
  mounted where the keyboard is. Use one of the USB-C ports: DP Alt Mode → video,
  USB → touch input, Power → panel."*

## Cable length is a real constraint

The Framework eDP cable is **short and not extensible**. Measured/estimated at **~162 mm**
for FW13 (community estimate from the marketplace photo, using the 27.55 mm I-PEX connector
as scale). A 2026-08 thread attempting a FW13 tablet conversion concluded *"the cable is
indeed not long enough"* and found **no way to extend or swap it**, forcing a redesign.

⇒ Any lid design that moves the display further from the mainboard **cannot reuse Framework's
eDP cable**. Another argument for the USB-C path, where cable length is a commodity.

## Mechanical resources Framework publishes (CC BY 4.0)

* FW13 `Display/`: `fw_13_5_inch_display.pdf` (2D drawing), `fw_13_5_inch_display_{1,2,3}.dxf`,
  `135_fhd_hads_asm.stp`, **`FW_display_w_cable_bracket.stp`** (display + cable bracket).
* FW16: `Framework Laptop 16 with Expansion Bay Shell CAD.zip`,
  `Framework Laptop 16 with Graphics Module CAD.zip`, `Hinges/framework_laptop_16_hinge_assy_asm.stp`,
  `Case/`, `Touchpad/`.
* FW16 `Display/` contains **only** `README.md` — **no 2D drawing, no DXF, no STEP** for the
  FW16 panel. A FW16 e-ink lid would need the panel envelope measured or derived from the
  whole-system CAD zips. This is a concrete extra cost of choosing FW16 over FW13.

## E-paper lifetime caveat (worth surfacing to any builder)

Community objection (DIY Perks forum): EPD panels have finite per-pixel refresh budget
(~10⁷ order), so a high-refresh e-ink *laptop* used for video/gaming degrades. Modos'
counter-position is in their Teardown session. Counter-anecdote in the same thread: multiple
e-ink devices (incl. Lenovo ThinkBook Plus Gen 4 colour e-ink laptop) showing no degradation
after years. **Unresolved — treat as a risk, not a settled fact.**

## References

* [Framework Guides — eDP Cable (Framework 16), 32 steps](https://guides.frame.work/Guide/eDP+Cable/300)
* [Framework Guides — Hinge Replacement Guide (FW13)](https://guides.frame.work/Guide/Hinge+Replacement+Guide/104)
* [iFixit — Framework Laptop 16 eDP Cable Replacement](https://www.ifixit.com/Guide/Framework+Laptop+16+eDP+Cable+Replacement/199420)
* [iFixit — Framework Laptop 16 Hinges Replacement](https://www.ifixit.com/Guide/Framework+Laptop+16+Hinges+Replacement/199424)
* [iFixit — Framework Laptop 16 Webcam Cable Replacement](https://www.ifixit.com/Guide/Framework+Laptop+16+Webcam+Cable+Replacement/199811)
* [Framework Community — Webcam cable too short for correct (over hinge) routing](https://community.frame.work/t/webcam-cable-too-short-for-correct-over-hinge-routing-solved/19595)
* [Framework Community — Any way to extend eDP cable for the 13 Touchscreen Display?](https://community.frame.work/t/any-way-to-extend-edp-cable-for-the-13-touchscreen-display/84318)
* [Framework Community — Dimensions of the eDP cable (~162 mm estimate)](https://community.frame.work/t/dimensions-of-the-edp-cable/38594)
* [Framework Community — [BUILD LOG] FW13 + Dual ZenBook Duo OLED Panels](https://community.frame.work/t/build-log-fw13-dual-asus-zenbook-duo-oled-panels-custom-chassis-custom-pcb-daily-driver/82046)
* [Framework Community — Dual Screen Framework 13 in our near future?](https://community.frame.work/t/dual-screen-framework-13-in-our-near-future/82173)
* [hanhanhan-kim/framework_tablet](https://github.com/hanhanhan-kim/framework_tablet)
* [DIY Perks forum — Building a e-ink Laptop](https://forum.diyperks.com/general-general/building-a-e-ink-laptop/)
* [Framework-Laptop-16 repo root (CAD zips, Hinges)](https://github.com/FrameworkComputer/Framework-Laptop-16)
