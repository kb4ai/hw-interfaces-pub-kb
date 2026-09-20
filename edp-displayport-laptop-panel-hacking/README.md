# eDP / DisplayPort laptop-panel hacking — Framework + Modos Glider

Scope: feasibility of driving a **Modos Glider** e-ink controller from a **Framework Laptop**
internal display path, and the FW13 vs FW16 eDP connector question.

Research date: **2026-09-20**.

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
