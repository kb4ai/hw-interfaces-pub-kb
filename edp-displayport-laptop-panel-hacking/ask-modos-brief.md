# If you ask Modos for a Framework e-ink lid — the brief to send them

Research date: 2026-09-20. Purpose: make the ask precise so it is answerable.

## The ask in one line

*"Can you do a Glider variant whose DisplayPort input bypasses the USB-C PD/Alt-Mode
front end — either a direct eDP/DP sink input, or a documented bypass into `PTN3460` —
so it can be fed from a laptop's internal eDP source rather than an external USB-C port?"*

## Why that is the right question

Glider's `dp_in` stage is **USB-C-first by construction**: `GT-USB-7007A` receptacle →
`FUSB302BMPX` CC/PD PHY → `CBTL02043A` lane mux (MCU-configured after Alt-Mode entry) →
`PTN3460` DP receiver. Raw DP lanes on the USB-C pins are never routed to the bridge
without CC negotiation. So "make a lid" alone does **not** finish the job — the input
stage is the blocker, not the shell.

It is a reasonable ask because **early Glider revisions already used a full-size
DisplayPort connector** before switching to USB-C (their own Crowd Supply deep dive),
and the board is open hardware in KiCad. A DP-direct respin is re-treading their own path.

## Supply them with (all public, CC BY 4.0)

* Framework panel-side eDP pinout, FW13 and FW16 — the two `Display/README.md` files.
  Identical except FW16 adds DDS SCL/SDA (pins 1, 34), PSR_EN# (35), OD_EN (40).
* Framework mainboard-side pinout — FW13 `Mainboard/README.md`; FW16 schematic p.4.
* Mainboard receptacle **I-PEX 20879-040E-01** (Digi-Key 14312094); mating cable
  **I-PEX 81466-100B-02-D** (Digi-Key 14312108). Framework also sells its own eDP cable
  ($19, FW13/FW16/FW12 SKUs).
* Mainboard already AC-couples the link: 0.22 µF main-link, 0.1 µF AUX.
* Framework's published display CAD (FW13 has DXF/STEP/PDF; **FW16 has none**).

## Constraints to state up front

* **Framework eDP is 4-lane**; `PTN3460` does **1–2 lanes at 1.62/2.7 Gbit/s**. Glider's
  own ceiling is 224 MP/s over DP. Confirm the source will train down.
* **Power**: e-paper PSU ±15 V @ 1 A peak; large panels >20 W peak. A lid build must say
  where that comes from — `BL_POWER` on the eDP connector is 5–21 V but only
  **0.3 A × 4 pins ≈ 1.2 A** (connector rating), and `3V_EDP` is 4 pins ≈ 4 W.
* **No EDID autodetect** — panel timing is flashed (`setres` / `cfggen`).
* Backlight/`BLK_PWM_LCD`/`BLK_OFF_N` are meaningless to an EPD; the laptop's EC will still
  drive them. Firmware may need to tolerate a panel that reports no backlight.

## The fallback that needs no Modos work at all

Mount Glider **in the lid**, run a **USB-C cable** from a Framework port through/along the
hinge. Framework ports do DP Alt Mode natively; Glider is happy; you lose one port and
gain an ugly cable run. Everything then reduces to the shell print — which *is* the thing
the original intuition said, just via USB-C rather than eDP.

## References

* [Glider `pcb/mainboard/dp_in.kicad_sch`](https://github.com/Modos-Labs/Glider/tree/main/pcb/mainboard)
* [Glider README — Type-C negotiation section](https://github.com/Modos-Labs/Glider/blob/main/README.md)
* [Crowd Supply — A Technical Deep Dive Into Glider](https://www.crowdsupply.com/modos-tech/modos-paper-monitor/updates/a-technical-deep-dive-into-glider)
* [Modos Paper Laptop — their own stated goal](https://www.modos.tech/blog/modos-paper-laptop)
* [Framework Community — Glider - open source e-ink monitor](https://community.frame.work/t/glider-open-source-e-ink-monitor/51012)
* [Framework Community — Replaceable E-ink Display](https://community.frame.work/t/replaceable-e-ink-display/13327)
* [Framework Community — Hack together eink screen using epdiy?](https://community.frame.work/t/hack-together-eink-screen-using-epdiy/4492)
* [arthomnix/FW16_EPD — touchscreen e-paper *input module* for FW16](https://community.frame.work/t/showcase-touchscreen-e-paper-input-module/62895)
