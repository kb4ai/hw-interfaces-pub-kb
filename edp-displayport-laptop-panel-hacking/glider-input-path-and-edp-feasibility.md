# Modos Glider input path, and whether Framework's eDP can feed it

Research date: 2026-09-20.

## Signal direction — the load-bearing fact

| | Framework mainboard eDP | Modos Glider |
|---|---|---|
| Role | **SOURCE** (DP upstream / DFP) — transmits | **SINK** (DP downstream / UFP) — receives |
| Drives | main-link TX pairs, AUX master, reads HPD | terminates lanes, AUX slave, asserts HPD |

These are **complementary**, not opposed. That is the good news and it is routinely
misunderstood: an "eDP→DP adapter" is *not* a protocol converter, because eDP main link and
DP main link are the same electrical/protocol layer. What a source-side adapter does is
re-map pins and terminate the sidebands. Framework's mainboard already AC-couples its
main-link pairs (0.22 µF, `CV1`–`CV8`) and AUX (0.1 µF), so the pins at `JEDP` are already
DP-spec-compliant AC-coupled differential pairs.

So a passive eDP-source → DP-sink adapter is **electrically real**. Two independent
statements of this:

* Paul_Combe, Framework forum: *"eDP can be adapted to full size DisplayPort with a simple
  passive cable, just hooking up the signals for full-size DisplayPort and ignoring the
  power and backlight stuff. But since the Framework uses a custom eDP cable, you'd need a
  little breakout board to adapt the Framework eDP to a standard eDP socket or a customized
  cable."*
* `kaybarkbark` designed and ordered *"a passive eDP to Displayport adapter"* PCB
  (PCBWay) for a Framework mainboard. **Outcome never reported** in the thread — treat as
  an unconfirmed attempt, not a working precedent.

Commercial: `micro-coaxial.com` advertises **custom eDP-to-DP cable assemblies**
(IPEX 20453-330T/340T plug, 20455-030E/040E receptacle) — a made-to-order product, not
off-the-shelf retail.

### Real caveats on the source side (from the one detailed failure report)

Electronics.StackExchange Q416604 — an eDP→DP jumper-wire attempt that got EDID and
detection but **never produced an image**. Answer (Ale..chenski):

* Jumper wires destroy the differential transmission line; **DP link training will fail**
  if equalization cannot hit the BER threshold. Needs controlled-impedance twinax/PCB.
* That laptop's eDP was **2-lane**; *"I don't believe that a standard 4-lane DP monitor can
  auto-detect and auto-negotiate the lane re-sizing."* (Framework is 4-lane, so this
  particular failure mode does not apply, but it shows lane-count matching matters.)
* Possible **firmware/BIOS** refusal on the laptop side, since full-DP-out was not a
  designed option.

Additionally, an eDP source typically expects a fixed panel and may not do full hot-plug /
EDID-driven retraining the way a DP source does.

## ⭐ But Glider will not accept a raw DP feed anyway

This is the decisive finding, and it is at schematic level. Component list extracted from
`pcb/mainboard/dp_in.kicad_sch` in the Glider repo:

| Part | Function |
|---|---|
| `GT-USB-7007A` | USB Type-C receptacle |
| **`FUSB302BMPX`** | USB Type-C **CC-line PD PHY** — runs the USB-PD stack |
| **`CBTL02043A`** | USB-C **DisplayPort Alt-Mode lane crosspoint mux** |
| **`PTN3460`** | DP → LVDS bridge — the actual **DP sink** |
| `5.1K/1%` | CC pulldowns (Rd) → board presents as a **UFP/sink** |
| `USBLC6-2P6`, `RCLAMP0524P` | ESD |

Glider's own README confirms the sequence explicitly:

> *"The onboard USB Type-C port can support video input in addition to powering the board
> using the USB-C DisplayPort Alt Mode. The MCU runs a USB-C PD stack to communicate this
> capability to the video source device over standard USB PD protocol. The MCU also controls
> the Type-C signal mux to remap the lanes to the correct place depending on the cable
> orientation."*
>
> *"...the DP decoder chip also handles AUX P/N line flipping based on the Type-C cable
> orientation."*

**Therefore:** wiring Framework eDP lanes into Glider's USB-C jack does nothing. Without
CC/PD negotiation the MCU never configures `CBTL02043A`, so the lanes never reach `PTN3460`.

### The three ways out, ranked

1. **Feed Glider from a Framework USB-C port.** Framework ports natively do DP Alt Mode.
   Zero electrical design. Costs one port. This is the supported configuration and what
   the devkit expects ("a single USB-C cable to a supported DisplayPort Alt Mode source,
   such as a laptop, is enough" — USAGE.md). **Recommended.**
2. **Build an eDP→USB-C DP-Alt-Mode-*source* board.** Needs a PD controller acting as
   DFP/source that enters DP Alt Mode over CC, plus lane mux. This is the mirror image of
   Arya Voronova's published **USB-C→eDP** board (which uses *the Framework eDP connector*,
   built for "KVM conversion mode"). Non-trivial firmware (VDM) work; she documented getting
   AUX working only after bodging two resistors. **No off-the-shelf product found.**
3. **Bypass Glider's mux in hardware** — feed DP lanes directly to `PTN3460`'s receive port
   and drive HPD/AUX yourself. Glider is open hardware (KiCad sources published), and
   *early Glider revisions used a full-size DisplayPort connector before switching to
   USB-C* — so a DP-direct variant is a documented part of its own lineage. This is the
   thing **to ask Modos for**: a respin/variant of `dp_in` with a direct DP or eDP input.

### Bandwidth sanity check

* `PTN3460` per its datasheet: DP link 1.62 / 2.7 Gbit/s, **1-lane or 2-lane** only.
* Glider README: *"Maximum pixel rate using DisplayPort (with PTN3460): 224 MP/s"*;
  a 7-series FPGA with 6G SerDes would give 720 MP/s.
* Framework eDP is **4-lane**. A 4-lane source feeding a 2-lane sink must train down —
  standard DP behavior, but another reason the USB-C path (where Alt Mode negotiates
  lane count properly) is safer than a hand-wired one.
* 13.3" kit config is `1600x1200 @ 75 Hz cvt-rb2` ≈ 125–180 MP/s — within budget.

## Glider board facts relevant to a lid build

* FPGA Xilinx Spartan-6 LX16 + DDR3-800 framebuffer + STM32H750 MCU.
* Inputs: USB-C DP Alt Mode (PTN3460) **or** DVI via **Mini-HDMI** (ADV7611 decoder).
  DVI mode still needs USB-C for power + USB → **two cables**.
* E-paper PSU: ±15 V rail, up to **1 A peak**; modern high-res panels can draw **>20 W peak**.
  This is a real power-budget item for an in-lid build.
* Panel adapters in-repo: `34p-a/b`, `35p-a`, **`39p-b`, `39p-c`**, `40p-ab`, `50p-b/c`,
  `u133_adapter`, `mega_adapter`. Confirms the 39-pin FFC family the brief mentions.
* The board **cannot autodetect the panel** — timing must be flashed (`setres` / `cfggen`).
* Suspend follows host PC sleep; auto-sleeps on video-signal loss.

## References

* [Modos-Labs/Glider — README](https://github.com/Modos-Labs/Glider/blob/main/README.md)
* [Modos-Labs/Glider — USAGE.md](https://github.com/Modos-Labs/Glider/blob/main/USAGE.md)
* [Glider `pcb/mainboard/dp_in.kicad_sch`](https://github.com/Modos-Labs/Glider/tree/main/pcb/mainboard)
* [Glider `pcb/` adapter list (39p-adapter-b/c, mega_adapter)](https://github.com/Modos-Labs/Glider/tree/main/pcb)
* [Crowd Supply — Modos Paper Monitor](https://www.crowdsupply.com/modos-tech/modos-paper-monitor)
* [Crowd Supply — A Technical Deep Dive Into Glider](https://www.crowdsupply.com/modos-tech/modos-paper-monitor/updates/a-technical-deep-dive-into-glider) — "Switched to a USB Type-C port over full-size DP"
* [PTN3460 datasheet](https://pdf.htelec.com/pdf/1/PTN3460.pdf) — 1.62/2.7 Gbit/s, 1–2 lanes
* [Framework Community — Framework internal display connector to DP port?](https://community.frame.work/t/framework-internal-display-connector-to-dp-port/24034)
* [Electronics.SE — Wiring eDP to a full-size DisplayPort](https://electronics.stackexchange.com/questions/416604/wiring-embedded-display-port-edp-to-a-full-size-display-port)
* [Framework Community — RISCV Mainboard Schematic/Block Diagrams](https://community.frame.work/t/riscv-mainboard-schematic-block-diagrams/64342) — passive eDP→DP adapter attempt
* [Hackaday — DisplayPort: Hacking And Examples](https://hackaday.com/2024/05/16/displayport-hacking-and-examples/) — USB-C→eDP board using the Framework connector
* [micro-coaxial.com — custom eDP to DP cable](https://www.micro-coaxial.com/product-detail/edp-to-dp-cable-embedded-displayport/)
