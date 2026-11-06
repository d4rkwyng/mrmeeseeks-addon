# Mr MeeSeeks Shitty Addon

A #badgelife shitty addon PCB designed for DEF CON 26.

Created by @d4rkwyng and SparX. Thanks to [AND!XOR](https://www.andnxor.com/) for their instructional video on the schematics used in this project.

## Preview

| Front | Back |
|:-:|:-:|
| ![Front](design/example/Front.png) | ![Back](design/example/Back.png) |

## Bill of Materials

Rev E (see `mrmeeseeks_RevE_BOM.csv`). Rev A–D boards carry a ninth LED/resistor pair (D9/R9) and an 0402 C1.

| Ref | Component | Package | Qty |
|-----|-----------|---------|-----|
| U1 | MCP23017 | SOIC-28 (`mrmeeseeks_RevE`) or SSOP-28 (`mrmeeseeks_RevE_ESO`) | 1 |
| D1-D8 | LED | 1206 | 8 |
| R1-R8 | 220 ohm | 1206 | 8 |
| C1 | 0.1uF | 1206 | 1 |
| X1 | Shitty connector | 2x2 | 1 |

## Hardware Notes (Rev E)

- **I2C address `0x20`** — MCP23017 A0/A1/A2 are all tied to GND (`0100 A2 A1 A0`). Bus runs at the badge's 3.3 V.
- **LEDs on GPB0–GPB7** (D1–D8 via R1–R8 to GND), active high: set IODIRB = `0x00`, then write GPIOB/OLATB. Port A (GPA0–GPA7), INTA and INTB are unconnected; RESET is tied to 3.3 V.
- **Connector** follows the original 2x2 badgelife add-on layout (front view: VCC/SDA top row, GND/SCL bottom row). The footprint numbers its pads 1=VCC, 2=GND, 3=SCL, 4=SDA; only the physical positions matter for this 2x2 standard.
- `mrmeeseeks_RevE.net` is a stale 2018 netlist that still lists the Rev D D9/R9 pair — do not re-import it into the Rev E boards.
- On Rev A–D, D9/R9 is driven from GPA0 (pin 21). Both Rev E boards still carry that net name (`Net-(R9-Pad1)`) on U1 pin 21, but nothing else is on the net and it has no copper, so the pin is unconnected.
- `mrmeeseeks_RevE_BOM.csv` lists the SOIC board (`mrmeeseeks_RevE.kicad_pcb`). For the SSOP board, U1 is `MCP23017-ESS_NoSilk` instead; every other line is the same.

## Fabrication Outputs

All gerbers were plotted with KiCad 4.0.7 in 2018.

- `gerbers/mrmeeseeks_RevE-*` (SOIC board; `mrmeeseeks_RevE.zip` holds the same files) were plotted 2018-06-13, before commit `aacb9ba` (2018-06-18) moved U1's `MCP23017` value text from B.SilkS to B.Fab. The only difference from the current board is that text: it is still in `mrmeeseeks_RevE-B.SilkS.gbo`, drawn in 0.05 mm strokes inside U1's outline. Copper, mask, paste, edge cuts and drill match the current board.
- `gerbers/mrmeeseeks_RevE_ESO-*` (SSOP board) were plotted from the board as committed in `7509748`. There is no zip for this set.
- Both Rev E boards use a 0.2 mm pad-to-mask clearance. On the SSOP board this makes each U1 mask opening 0.83 mm wide at a 0.65 mm pin pitch, so there is no solder-mask web between U1 pins. On the SOIC board the web is about 0.11 mm.

## Revisions

- **Rev E** — Final revision, two board variants sharing `mrmeeseeks_RevE.sch`:
  - `mrmeeseeks_RevE.kicad_pcb` — MCP23017 in SOIC-28 (`MCP23017-ESO_NoSilk`, 1.27 mm pitch)
  - `mrmeeseeks_RevE_ESO.kicad_pcb` — MCP23017 in **SSOP-28** (`MCP23017-ESS_NoSilk`, 0.65 mm pitch), despite the "ESO" in the filename
- **Rev D** — PCB only (no schematic in repo); gerbers in `gerbers/mrmeeseeks_RevD.zip`
- **Rev B, Rev C** — PCB only
- **Rev A** — Initial prototype (schematic `mrmeeseeks_RevA.sch` + PCB)

## Project Structure

```
mrmeeseeks-addon/
├── design/          # Artwork, silkscreen, and soldermask KiCad footprints
├── gerbers/         # Manufacturing files (Rev D, Rev E, Rev E SSOP variant)
├── lib/             # KiCad component libraries
└── mrmeeseeks_*     # Schematic, PCB, and netlist files
```

## Tools

- [KiCad](https://www.kicad.org/) — Schematic and PCB design

## License

MIT License. See [LICENSE](LICENSE) file.
