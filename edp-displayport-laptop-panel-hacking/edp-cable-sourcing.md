# Framework eDP cable — what it is, and how to obtain the mating parts

Research date: 2026-09-20.

## Yes, Framework sells it separately

Framework lists the eDP cable as a standalone Marketplace part for **all three** laptop
lines. It was added specifically because the community asked: CEO Nirav Patel (`nrp`),
2023-09-01: *"This cable is now available in the Framework Marketplace... Thanks everyone
for letting us know that this would be useful to list individually."*

| Part | URL | Price | Stock (2026-09-20) |
|---|---|---|---|
| Framework Laptop 13 eDP Cable | `frame.work/products/edp-cable` | — | **Out of stock** |
| Framework Laptop 16 eDP Cable | `frame.work/products/16-edp-cable` | **$19.00** | **Out of stock** |
| Framework Laptop 12 eDP Cable | `frame.work/products/laptop12-edp-cable` | **$19.00** | **Out of stock** |

FW13 product copy: *"The eDP cable connects the display on Framework Laptop 13 to the
Mainboard. It comes with the Display Kit, so you probably only need this if you're
**building your own projects**."* — Framework explicitly blesses the modding use case.

All three were out of stock at time of research; stock has historically cycled. Before the
2023 listing, Framework **support refused** to sell the cable ad-hoc: *"These cables are not
for sale, if you modify the mainboard it might break your warranty."*

## Replaceability

Fully replaceable, documented, user-serviceable with the bundled T5 screwdriver:

* Framework Guides: `eDP Cable` (FW16) — 32 steps, Moderate, 15–45 min.
* iFixit: FW16 eDP Cable Replacement (#199420), FW12 eDP Display Cable Replacement (#186700).
* FW12 guide notes the cable *"threads through the right hinge"*; FW16 guide routes it
  *"through the left hinge... in the rear guided channel"*.

Both ends are latched, not soldered: display end has a flip-up **metal locking bar** under a
sticker and slides **parallel** to the panel; mainboard end is a **press connector with a
pull tab**, lifted straight up.

## Sourcing the mating parts yourself (the actual hard part)

Historically the #1 blocker for Framework eDP breakout projects was not the pinout — which
Framework publishes — but **sourcing the mainboard-side mating connector**. Exact parts,
from Karl_Buchka on the Framework forum:

| Role | Part | Distributor |
|---|---|---|
| Cable assembly that mates to the mainboard | **I-PEX 81466-100B-02-D** | Digi-Key **14312108** |
| Board-mounted receptacle (mainboard connector) | **I-PEX 20879-040E-01** | Digi-Key **14312094** |

His notes, verbatim in substance:

* The cable *"has an identical IPEX Cabline-UM connector on both ends (with **flipped
  pinouts**!), so you'll need the board-mounted receptacle **and a custom PCBA** to do
  anything useful with it."*
* *"This is the **only mating cable** I've been able to find that is available in single
  quantity."*
* *"Soldering the receptacle by hand can be challenging, but is very doable. I used a paste
  stencil and a hotplate to reflow it."*

⚠️ Note the discrepancy to resolve on the bench: Karl describes the generic I-PEX cable as
having the *same* connector both ends with flipped pin order, whereas Framework documents
**different** part numbers per end (mainboard `20879-040E` vs FW13 panel `20455-040E`) and
genuinely different signal orders (mainboard pin 1 = `BL_POWER`; panel pin 36–39 = `BL_POWER`).
Framework's own cable is a **remapping** cable. Do not assume a generic Cabline-UM cable is
a drop-in for Framework's.

Alternative: **custom eDP-to-DP cable assemblies** are made to order by micro-coaxial
manufacturers (IPEX 20453-330T/340T plug ↔ 20455-030E/040E receptacle ↔ DP).

## Precedent boards using the Framework eDP connector

* **Arya Voronova (Hackaday)** — published a **USB-C → eDP** converter board that uses *the
  Framework laptop eDP connector*, built for a "KVM conversion mode". Her assessment:
  *"The Framework's eDP connector is overall pretty capable — four lanes **and** touchscreen
  connection, all within just 40 pins!"* Note also: system76's **Virgo** open laptop appears
  to use the same connector, but *"confused the polarity because of Framework's initially
  confusing data."*
* **`basketofkittens/framework701c`** — Framework mainboard in a ThinkPad 701C chassis;
  repo contains `PCB/eDP Adapter/` with `eDP Adapter Schematic.pdf`, `eDP Adapter Board.pdf`,
  Altium `.PcbDoc`/`.PrjPCB`.
* **`kaybarkbark`** — passive eDP→DP adapter PCB ordered from PCBWay for a Framework
  mainboard; **outcome unreported**.

## References

* [Framework Marketplace — FW13 eDP Cable](https://frame.work/products/edp-cable)
* [Framework Marketplace — FW16 eDP Cable](https://frame.work/products/16-edp-cable)
* [Framework Marketplace — FW12 eDP Cable](https://frame.work/products/laptop12-edp-cable)
* [Framework Guides — eDP Cable (FW16)](https://guides.frame.work/Guide/eDP+Cable/300)
* [iFixit — FW16 eDP Cable Replacement](https://www.ifixit.com/Guide/Framework+Laptop+16+eDP+Cable+Replacement/199420)
* [iFixit — FW12 eDP Display Cable Replacement](https://www.ifixit.com/Guide/Framework+Laptop+12+eDP+Display+Cable+Replacement/186700)
* [Framework Community — Mainbord screen port to eDP cable](https://community.frame.work/t/mainbord-screen-port-to-edp-cable/31230) — I-PEX part numbers, nrp's marketplace announcement
* [Digi-Key I-PEX 81466-100B-02-D](https://www.digikey.com/en/products/detail/i-pex/81466-100B-02-D/14312108)
* [Digi-Key I-PEX 20879-040E-01](https://www.digikey.com/en/products/detail/i-pex/20879-040E-01/14312094)
* [Hackaday — DisplayPort: Hacking And Examples](https://hackaday.com/2024/05/16/displayport-hacking-and-examples/)
* [basketofkittens/framework701c — PCB/eDP Adapter](https://github.com/basketofkittens/framework701c/tree/main/PCB/eDP%20Adapter)
