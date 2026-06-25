# Planetary Cycloidal Hub Gearbox — Technical Study

A mechanical analysis of the **3D-printed Planetary Cycloidal Hub Gearbox**, an "RV"-style
reducer: its two-stage architecture, measured part geometry, materials, and demonstrated
durability. It is the engineering companion to [print-settings.md](print-settings.md) and
[bill-of-materials.md](bill-of-materials.md).

> **Method note.** All part dimensions, triangle counts, and STL encodings reported below were
> obtained by parsing the supplied surface meshes directly (see [§4 Measured part data](#4-measured-part-data)).
> The reduction ratio is discussed in [§6 Reduction ratio](#6-reduction-ratio).

---

## 1. Overview

The reducer is an **"RV"-style cycloidal gearbox**: a compact, single-body unit integrating a
**planetary (spur-gear) input stage** with a **cycloidal output stage**. The design is released as
open CAD and print files and was modelled in Fusion 360.

In the accompanying build video the abbreviation **RV** is glossed as *"radial vector"*; the term
more conventionally denotes the **Rotary-Vector** cycloidal architecture popularized by Nabtesco's
RV series. In both cases the defining feature is the same — a sun/planet spur-gear set feeding a
cycloidal disc pair — which yields very high reduction, high torque, and near-zero backlash in a
short axial package.

**Native motor frame:** NEMA 23. The unit adapts to a NEMA 17 via a re-bored sun gear and an adapter
plate (see [assembly-guide.md](assembly-guide.md)); no internal components change.

### At a glance

| Property | Value |
|---|---|
| Architecture | 2-stage: planetary (spur) → cycloidal |
| Native motor frame | NEMA 23 (adapts to NEMA 17) |
| Housing envelope | **≈ 100 × 100 × 26 mm** base (full assembly is deeper than the base alone) |
| Cycloidal disc Ø | ≈ **74 mm** × 6.35 mm thick (×2) |
| Eccentric cam Ø × length | **Ø 26 × 35.7 mm** (×3) |
| Printable parts | **13** STL files |
| Total mesh complexity | ≈ **412 800 triangles** across all parts |
| STL coordinate units | **millimetres** (modelled in mm) |
| STL encoding | 1 ASCII, 12 binary |

---

## 2. Operating principle — power flow

Motion passes through **two distinct reduction stages** within one housing:

```
motor shaft ─► sun gear ─► (3) planet spur gears ─► (3) eccentric cams ─► cycloidal discs ─► output hub
        \________ planetary stage ________/   \___________ cycloidal stage ___________/
```

### Stage 1 — planetary (spur-gear) pre-reduction

- A **helical spur sun gear** mounts on the motor shaft
  ([`Sun_Gear_NEMA17.stl`](../STL/Sun_Gear_NEMA17.stl)).
- It meshes with **three spur gears**, one on each **eccentric cam**
  ([`Eccentric_Cam_1.stl`](../STL/Eccentric_Cam_1.stl),
  [`Eccentric_Cam_2.stl`](../STL/Eccentric_Cam_2.stl),
  [`Eccentric_Cam_3.stl`](../STL/Eccentric_Cam_3.stl)).
- This stage performs two functions simultaneously:
  1. **first torque multiplication** (sun → planets), and
  2. **phase-locking the three cams** so they share the cycloidal reaction load evenly (each cam
     carries roughly one-third of it).

### Three-cam architecture

A conventional cycloidal drive uses a **single** eccentric cam. Distributing the drive across
**three** cams — synchronized through the spur gears — lowers the load on each cam and is expected
to extend service life. This is the principal departure from a textbook cycloidal drive.

### Stage 2 — cycloidal reduction

- The rotating eccentric cams cause the two **cycloidal discs**
  ([`Cycloidal_Disk_1.stl`](../STL/Cycloidal_Disk_1.stl),
  [`Cycloidal_Disk_2.stl`](../STL/Cycloidal_Disk_2.stl))
  to **orbit** (nutate) rather than spin.
- Each disc's lobed profile rolls against fixed pins/rollers in the housing; an output hub carrying
  its own pins engages the discs and **converts the orbital motion into slow, high-torque rotation**.

A further distinguishing feature: in this design the cycloidal discs undergo **no net rotation** —
they nutate in place. A conventional cycloidal drive instead rotates its disc slowly. Eliminating
that disc rotation reduces the moving-part count.

### Net effect

Because the planetary stage feeds the cycloidal stage, the **total reduction is the product of the
two**, producing a high-ratio, low-speed, high-torque, backlash-free output.

---

## 3. Construction & materials

| Sub-system | Parts | Fabrication | Material |
|---|---|---|---|
| Eccentric cams (×3) | resin-printed for tight tolerances | SLA | **SirayaTech Nylon Black** resin |
| Cycloidal discs (×2) | laser-cut acrylic or FDM-printed | laser cutter / FDM | acrylic **or** PLA |
| Housing, caps, hub, standoffs | FDM | FDM | **PLA+** (a full-PLA build is verified to run) |
| Sun gear | FDM (gear axis = Z) | FDM | PLA/PETG/ASA; Nylon ideal |
| Fastening | M3/M5 socket screws into **heat-set inserts** | — | steel screws + brass inserts |

- **Heat-set inserts** are used throughout, allowing repeated assembly and disassembly without
  stripping plastic threads.
- **Bearings are deliberately standard, inexpensive sizes** (see [bill-of-materials.md](bill-of-materials.md)).
- The complete unit has been rebuilt **entirely in FDM PLA**, confirming it is not dependent on
  resin or laser tools.

Full slicing guidance: [print-settings.md](print-settings.md).

---

## 4. Measured part data

Dimensions (X × Y × Z in mm), triangle counts, and STL encoding, obtained by parsing the meshes in
[`../STL/`](../STL). The bounding-box Z axis is each part's own modelling axis — orient parts per
print, not per this table.

### Housing & structural

| Part | File | X × Y × Z (mm) | Tris | Enc | Role |
|---|---|---|---:|---|---|
| Sealed base | [`Gearbox_Base_Closed.stl`](../STL/Gearbox_Base_Closed.stl) | 100.0 × 100.0 × 26.0 | 8 976 | bin | Closed housing body |
| End / face cap | [`End_Cap.stl`](../STL/End_Cap.stl) | 67.0 × 67.0 × 19.4 | 12 012 | bin | Faceplate; through-bolts clamp the stack |
| Hub retainer | [`Hub_Retainer.stl`](../STL/Hub_Retainer.stl) | 77.2 × 77.2 × 5.0 | 3 508 | bin | Retains the output hub |
| Hub cap | [`Hub_Cap.stl`](../STL/Hub_Cap.stl) | 49.0 × 49.0 × 10.0 | 4 994 | bin | Output-hub cover |

### Planetary / input stage

| Part | File | X × Y × Z (mm) | Tris | Enc | Role |
|---|---|---|---:|---|---|
| Sun gear (NEMA 17) | [`Sun_Gear_NEMA17.stl`](../STL/Sun_Gear_NEMA17.stl) | 16.0 × 16.0 × 22.0 | 60 492 | bin | Motor pinion, re-bored to 5 mm + M3 grub screw |
| Adapter plate | [`Adapter_NEMA17_to_NEMA23.stl`](../STL/Adapter_NEMA17_to_NEMA23.stl) | 60.0 × 60.0 × 8.0 | 7 802 | **ASCII** | NEMA 23 face ↔ NEMA 17 motor |

### Eccentric cams (Stage-1 → Stage-2 link)

| Part | File | X × Y × Z (mm) | Tris | Enc | Role |
|---|---|---|---:|---|---|
| Cam 1 | [`Eccentric_Cam_1.stl`](../STL/Eccentric_Cam_1.stl) | 26.0 × 26.0 × 35.7 | 83 768 | bin | Wobble driver, phase A |
| Cam 2 | [`Eccentric_Cam_2.stl`](../STL/Eccentric_Cam_2.stl) | 26.0 × 26.0 × 35.7 | 85 074 | bin | Wobble driver, phase B (120°) |
| Cam 3 | [`Eccentric_Cam_3.stl`](../STL/Eccentric_Cam_3.stl) | 26.0 × 26.0 × 35.7 | 86 380 | bin | Wobble driver, phase C (120°) |
| Cam cap | [`Cam_Cap.stl`](../STL/Cam_Cap.stl) | 18.0 × 18.0 × 14.7 | 4 086 | bin | Forms the 2nd half of each eccentric cam (×3), bolted on |

The three cams share **identical geometry**, phased 120° apart by the spur gears; the differing
triangle counts are tessellation noise between the three exported files.

### Cycloidal stage

| Part | File | X × Y × Z (mm) | Tris | Enc | Role |
|---|---|---|---:|---|---|
| Disc 1 | [`Cycloidal_Disk_1.stl`](../STL/Cycloidal_Disk_1.stl) | 73.88 × 73.89 × 6.35 | 20 836 | bin | Cycloidal disc (anti-phase pair A) |
| Disc 2 | [`Cycloidal_Disk_2.stl`](../STL/Cycloidal_Disk_2.stl) | 74.08 × 74.08 × 6.35 | 25 388 | bin | Cycloidal disc (anti-phase pair B) |
| Standoff | [`Cycloidal_Standoff.stl`](../STL/Cycloidal_Standoff.stl) | 67.0 × 67.0 × 24.7 | 9 490 | bin | Spacer stack between disc sets / cam halves |

---

## 5. Bill of materials (non-printed)

The reducer requires its own bearings and fasteners, independent of the motor — the NEMA 17
adaptation removes none of them. Full quantities are in
[bill-of-materials.md](bill-of-materials.md) (structured data:
[../bom/hardware.csv](../bom/hardware.csv)). Summary:

| Class | Items |
|---|---|
| Bearings | 9× **15×24×5**, 3× **20×27×4**, 1× **45×58×7** |
| Screws | M5×12 (12), M5×20 (4), M3×8/10/16/20/25 (4 each) |
| Nuts & inserts | 16× M5 nylock, 16× M3 heat-set insert, 1× M3×6 grub |

The single **45×58×7** bearing is the main output-hub bearing; the **9× 15×24×5** serve as the
cycloidal roller/pin bearings; the **3× 20×27×4** seat the three eccentric cams.

---

## 6. Reduction ratio

The total ratio equals **planetary stage × cycloidal stage** and is high (tens-to-one).

The exact value is set by the **sun/planet tooth counts** and the **cycloidal lobe/pin counts**,
which are defined parametrically in the FreeCAD source
([`../cad/rv-reducer.FCStd`](../cad/rv-reducer.FCStd)). It **cannot be recovered reliably from the
STL surface mesh**: the disc lobes are geometrically subtle and the exported rim tessellation is too
coarse for a clean count, so a mesh-derived ratio should not be trusted. Measure it empirically after
assembly:

```
ratio = (input shaft turns) ÷ (output hub turns)
```

---

## 7. Durability — continuous-run endurance test

A continuous-run endurance test was conducted by fitting a large PLA wheel to the output and
driving it outdoors, 24 h/day, on a slip-ring-fed test arm. Results (from the source build video):

| Metric | Result |
|---|---|
| Continuous run time | **~2 weeks, 24/7** (stopped at ~2 weeks) |
| Distance travelled | **≈ 35 miles** |
| Motor revolutions | **> 3 000 000** input turns |
| Weather | Sub-freezing nights (~5 days below freezing) + rain + snow |
| Lubrication | Gel lubricant added **once** before the test; never re-applied |
| Post-test wear | **Minimal** — cycloidal discs, helical spur gears, and output hub appeared essentially as-new |

**Caveat (per the source):** the test imposed **little output resistance**, so the gearbox operated
**well below its maximum torque**. The result is primarily a **cycle-life / durability** finding
(3 M+ revolutions on a 3D-printed spur-gear train with negligible wear), not a peak-torque rating.

---

## 8. File inventory (source)

Provided alongside this repo as the upstream "RV Reducer Files" bundle. In this repo the layout is:

```
pom360_camera/
├── README.md
├── STL/                      # 13 print-ready meshes (flat, one per part) — used in §4
│   └── plates/               # .3mf slicer project files
├── cad/                      # editable source
│   ├── rv-reducer.FCStd      # FreeCAD parametric model (tooth/lobe counts live here)
│   └── rv-reducer.step
├── bom/
│   └── hardware.csv          # structured buy-list
└── docs/                     # this study + BOM + print/assembly guides
    └── images/
```

---

## 9. See also

- [print-settings.md](print-settings.md) — materials, slicing, tolerances, ratio measurement.
- [bill-of-materials.md](bill-of-materials.md) — full bearing + fastener BOM with quantities.
- [assembly-guide.md](assembly-guide.md) — build order.
- [../README.md](../README.md) — repo landing page.
