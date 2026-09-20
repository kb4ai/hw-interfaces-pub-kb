# eDP / DisplayPort laptop-panel hacking — Framework + Modos Glider

Scope: feasibility of driving a **Modos Glider** e-ink controller from a **Framework Laptop**
internal display path, and the FW13 vs FW16 eDP connector question.

Research date: **2026-09-20**.


## ⭐ The two-minute answer (added 2026-09-20 after adversarial review)

**1. The laptop's eDP connector is irrelevant to this build.** eDP is a *source* output; Glider is a
*sink*. FW13 and FW16 share the same mainboard-side I-PEX 20879-040E and their pinouts differ only in
four pins FW16 adds (DDS SCL/SDA, PSR_EN#, OD_EN) — but that similarity buys nothing here, because
neither can feed Glider. No off-the-shelf eDP-source → DP-Alt-Mode-source adapter exists.

**2. ⚠ Do NOT assume USB-C DP Alt Mode is the only input.** An earlier version of this README led with
the Alt-Mode chain (FUSB302B → CBTL02043A mux → PTN3460), which requires USB-PD negotiation. Glider
*also* has a **DVI input over Mini-HDMI via an ADV7611** — a plain TMDS sink with **no PD, no CC
lines, no Alt-Mode mux**. Modos's own USAGE.md says: *"If DVI via the Mini-HDMI connector is needed,
use two cables: USB-C for power and USB, plus a Mini-HDMI video cable for video."* Max pixel rate over
DVI is 165 MP/s — ample for 1600×1200. ⇒ **The HDMI path is the simpler one**, and leading with
Alt-Mode alone was a half-answer.

⚠ **But do not conclude the demo used DVI — it did not.** Corrected 2026-09-20 against the builder's
own repository (`github.com/cittadhammo/omarchy-modos-eink`, MIT): *"USB-C DisplayPort Alt Mode
remains the video path; the HID interface is a separate control path."* ⇒ The working Framework 13
build runs **DP Alt Mode for video**, with **USB HID as a second, independent control channel** for
refresh modes. A first-hand account from the person who built it beats our inference from a schematic.
⇒ Practical consequence for an in-lid build: you need **USB data**, not just video and power, reaching
the board — and non-root control needs a udev rule for the raw HID node (VID `0x1209`, PID `0xae86`).

**3. The two decisive mechanical numbers, both previously missing:**

| | value | source |
|---|---|---|
| Glider mainboard | **90.00 × 80.00 mm**, 1.0044 mm PCB, 3× M2 holes | measured from `Edge.Cuts` in `pcb/mainboard/pcb.kicad_pcb` — Modos states it nowhere in prose |
| ED133UT3 panel **outline** | **285.80 × 213.65 × 0.78 mm** (active 270.40 × 202.80, 4 mm bezel, 96 g) | panel drawing |

⚠ **Use the panel OUTLINE, not its active area, for fit.** Comparing active-area-to-active-area
flatters the result badly:

* **Framework 13** lid opening ≈ 284.93 × 189.96 mm ⇒ the panel is **~23.7 mm too tall and ~0.9 mm
  too wide**. ⛔ It does not go in a 13 lid, full stop.
* **Framework 16** lid opening ≈ 344.6 × 215.4 mm ⇒ fits, but with only **~1.75 mm** of height
  margin (and ~58.8 mm spare width). Not the ~12.7 mm an active-area comparison suggests.

⇒ On the 16 it is *marginal but positive*, and the real question is the lid **cavity depth**, not the
opening — the panel's folded source-driver COFs raise local thickness in a band along the tail edge.

## Contents

* `framework-edp-fw13-vs-fw16-connector.md` — connector part numbers, both-side pinouts,
  the exact FW13↔FW16 differences, mux/graphics-switch implications.
* `glider-input-path-and-edp-feasibility.md` — ⭐ the signal-direction analysis
  (eDP = source, Glider = sink), Glider's schematic-level input chain, and what would
  actually be required to feed it.
* `hinge-routing-and-mechanical.md` — how Framework routes cables lid↔base, what the
  channel already carries, cable-length limits, precedent builds.
* `edp-cable-sourcing.md` — Framework's eDP cable as a purchasable part, I-PEX part
  numbers, Digi-Key single-quantity sourcing.
* `framework-lineup-2026-and-ram-correction.md` — ⚠️ LPCAMM2 is **FW13 Pro**, not FW16;
  full FW13 Pro / FW16 spec comparison and which platform suits an e-ink lid.
* `ask-modos-brief.md` — the precise, answerable request to put to Modos.
* `refs/` — archived primary documents (Framework pinout READMEs, FW16 mainboard
  interface schematic PDF, FW13 display drawing PDF, Glider README/USAGE).

## One-paragraph answer

The FW13 and FW16 internal display interfaces are **the same connector family and
essentially the same pinout** — so a lid/shell redesign is portable between them. But
the connector is **not the hard part and arguably not relevant at all**: eDP on the
mainboard is a *source* output, while Glider is a *sink* whose DisplayPort input sits
behind a USB-C PD/Alt-Mode negotiation (FUSB302B CC PHY + CBTL02043A lane mux + PTN3460
DP receiver). Glider will not accept raw eDP lanes without either a DP-Alt-Mode-source
board or a hardware bypass of its own mux. The path of least resistance is to feed
Glider from a **Framework USB-C port** — which natively does DP Alt Mode — and solve the
*mechanical* problem (board placement, cable through the hinge) rather than the
electrical one. So yes: it is largely "a lid/shell print" problem, **plus** a cable-routing
problem, **plus** losing one USB-C port.
