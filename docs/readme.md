# Planetary Cycloidal Hub Gearbox — Technical Study

A mechanical analysis of the **3D-printed Planetary Cycloidal Hub Gearbox**, an "RV"-style
reducer: its two-stage architecture and physics, measured part geometry, materials, and a planned
test programme. It is the engineering companion to [02-motor-drive-gears.md](02-motor-drive-gears.md)
(installation on a NEMA 17) and [rv-gearbox-hardware.md](rv-gearbox-hardware.md) (bearing/fastener
bill of materials).

> **Method note.** All part dimensions, triangle counts, and STL encodings reported below were
> obtained by parsing the supplied surface meshes directly (see [§4 Measured part data](#4-measured-part-data)).
> The reduction ratio is discussed in [§6 Reduction ratio](#6-reduction-ratio); validation is a
> forward-looking plan in [§7 Testing](#7-testing--to-be-conducted).

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
plate (see [02-motor-drive-gears.md](02-motor-drive-gears.md)); no internal components change.

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

## 2. Operating principle — physics & mechanics

Motion passes through **two reduction stages** inside one housing. This section walks the kinematics
and force flow stage by stage.

```
motor shaft ─► sun gear ─► (3) planet spur gears ─► (3) eccentric cams ─► cycloidal discs ─► output hub
        \________ planetary stage ________/   \___________ cycloidal stage ___________/
```

### 2.1 Two-stage topology

The reducer chains a **sun/planet spur stage** with a **cycloidal pin stage**. The spur stage both
reduces speed and synchronises the three eccentric cams; the cams then orbit the cycloidal discs,
whose lobed profiles walk around fixed pins and drive the output hub. Because the stages are in
series, the total ratio multiplies (see [§6](#6-reduction-ratio)).

### 2.2 Stage 1 — sun/planet spur stage and cam synchronisation

- The motor carries a **helical spur sun gear** that meshes with **three planet gears**, one on each
  eccentric cam
  ([`Eccentric_Cam_1.stl`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/STL/STL/Eccentric_Cam_1.stl),
  [`_2`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/STL/STL/Eccentric_Cam_2.stl),
  [`_3`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/STL/STL/Eccentric_Cam_3.stl)).
- The planet axes are **fixed in the housing** (there is no rotating carrier), so this is a simple
  sun→planet external spur mesh rather than a full epicyclic. The cam (planet) speed is
  `ω_cam = ω_motor · (Z_sun / Z_planet)`, in the opposite sense to the sun.
- Meshing all three planets with a **single common sun phase-locks them** — the cams are held 120°
  apart with no separate timing element.
- Sharing the input across **three** parallel meshes cuts the tooth load on each to roughly a third
  versus a single-cam cycloidal drive.

### 2.3 The eccentric cam — converting spin into orbit

Each eccentric cam is a gear whose **bore is offset from its pitch centre by an eccentricity *e***.
When the cam spins about its gear centre (the fixed axis above), the offset bore — and the cycloidal
disc pin riding in it — traces a circle of radius *e*. The disc pin therefore undergoes **pure
orbital (translational) motion**: it circles the axis while the cam body simply rotates in place.

- This spin→orbit conversion is the core of any cycloidal drive: a fast rotation becomes a small,
  precise circular "wobble" of the disc.
- With **three cams at 120°**, the three radial force vectors sum to (nearly) zero, so the cycloidal
  stage is **radially balanced** and imposes little net side load on the housing bearings.

### 2.4 Stage 2 — cycloidal disc and pin engagement

- Each **cycloidal disc**
  ([`Cycloidal_Disk_1.stl`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/STL/STL/Cycloidal_Disk_1.stl),
  [`_2`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/STL/STL/Cycloidal_Disk_2.stl))
  has a lobed **trochoidal** profile (a hypotrochoid with an equidistant offset) that engages two
  sets of cylindrical rollers: a **fixed ring** pressed into the housing, and a second set on the
  **output hub**.
- The fixed-ring pin count and the output-hub pin count differ by **exactly one**. As the cams orbit
  the disc once, its profile must climb exactly one pin relative to the fixed ring; the only way to
  satisfy both meshes is for the **output hub to rotate by one pin pitch**.
- Hence the cycloidal-stage reduction is set by the **pin count** (≈ number of output pins, = ring
  pins ∓ 1), giving a large ratio from a compact disc. The exact counts are parametric in the CAD
  ([`RV.FCStd`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/CAD/RV.FCStd)).

### 2.5 Why the discs nutate instead of spinning

In a textbook cycloidal drive the disc both orbits **and** slowly counter-rotates, and the disc
itself is the output. In this design the cycloidal discs are **constrained to pure nutation**
(orbit, no net spin): the rotation a standard cycloidal disc would absorb is instead taken up by the
**output hub**. Removing that disc rotation eliminates a degree of freedom and a moving part — the
design's main simplification relative to a conventional cycloidal drive.

### 2.6 Force and torque flow

- Torque path: motor → sun → 3 planet/cams → 3 disc pins → cycloidal profile → output pins → hub.
- Reaction torque is carried by the fixed ring into the housing. With three cams and **two
  anti-phase discs** sharing the load, contact is spread across many lobes at once — the source of
  the stage's high stiffness and low backlash.
- The **two anti-phase discs** (180° apart) also cancel the first-order **radial unbalance** (shaking
  force) of a single eccentric disc and double the contact area, smoothing the output.

### 2.7 Backlash, stiffness and efficiency

- **Backlash:** near-zero in principle — many lobes mesh simultaneously and the eccentric geometry
  keeps them preloaded against the pins, so there is little play to take up on reversal. That is the
  main reason the topology suits a repeatable motion axis.
- **Stiffness:** high torsional stiffness from the broad, multi-lobe contact.
- **Efficiency:** the rolling pin/roller contacts are low-friction, but the stage runs many bearings
  with local sliding at the lobe roots, so efficiency is moderate (single-stage cycloidal drives are
  typically ~70–90 %). The real figure should be **measured**, not assumed.

### Net effect — reduction

The total reduction is the product of the two stages and is high (tens-to-one); the exact value
comes from the tooth/pin counts in the CAD (see [§6](#6-reduction-ratio)).

---

## 3. Construction & materials

| Sub-system | Parts | Fabrication | Material |
|---|---|---|---|
| Eccentric cams (×3) | resin-printed for tight tolerances | SLA | **SirayaTech Nylon Black** resin |
| Cycloidal discs (×2) | laser-cut acrylic or FDM-printed | laser cutter / FDM | acrylic **or** PLA |
| Housing, caps, hub, standoffs | FDM | FDM | **PLA+** (an all-FDM build is a supported option) |
| Sun gear | FDM (gear axis = Z) | FDM | PLA/PETG/ASA; Nylon ideal |
| Fastening | M3/M5 socket screws into **heat-set inserts** | — | steel screws + brass inserts |

- **Heat-set inserts** are used throughout, allowing repeated assembly and disassembly without
  stripping plastic threads.
- **Bearings are deliberately standard, inexpensive sizes** (see [rv-gearbox-hardware.md](rv-gearbox-hardware.md)).
- An **all-FDM PLA build is a supported option**, so the unit is not locked to resin or laser tools.

Full slicing guidance: [02-motor-drive-gears.md](02-motor-drive-gears.md).

---

## 4. Measured part data

Dimensions (X × Y × Z in mm), triangle counts, and STL encoding, obtained by parsing the meshes in
[`…/STL/STL/`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/STL/STL/).
The bounding-box Z axis is each part's own modelling axis — orient parts per print, not per this
table.

### Housing & structural

| Part | File | X × Y × Z (mm) | Tris | Enc | Role |
|---|---|---|---:|---|---|
| Sealed base | [`Gearbox_Base_Closed.stl`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/STL/STL/Gearbox_Base_Closed.stl) | 100.0 × 100.0 × 26.0 | 8 976 | bin | Closed housing body |
| End / face cap | [`End_Cap.stl`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/STL/STL/End_Cap.stl) | 67.0 × 67.0 × 19.4 | 12 012 | bin | Faceplate; through-bolts clamp the stack |
| Hub retainer | [`Hub_Retainer.stl`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/STL/STL/Hub_Retainer.stl) | 77.2 × 77.2 × 5.0 | 3 508 | bin | Retains the output hub |
| Hub cap | [`Hub_Cap.stl`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/STL/STL/Hub_Cap.stl) | 49.0 × 49.0 × 10.0 | 4 994 | bin | Output-hub cover |

### Planetary / input stage

| Part | File | X × Y × Z (mm) | Tris | Enc | Role |
|---|---|---|---:|---|---|
| Sun gear (NEMA 17) | [`Sun_Gear_NEMA17.stl`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/STL/STL/Sun_Gear_NEMA17.stl) | 16.0 × 16.0 × 22.0 | 60 492 | bin | Motor pinion, re-bored to 5 mm + M3 grub screw |
| Adapter plate | [`Adapter_NEMA17_to_NEMA23.stl`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/STL/STL/Adapter_NEMA17_to_NEMA23.stl) | 60.0 × 60.0 × 8.0 | 7 802 | **ASCII** | NEMA 23 face ↔ NEMA 17 motor |

### Eccentric cams (Stage-1 → Stage-2 link)

| Part | File | X × Y × Z (mm) | Tris | Enc | Role |
|---|---|---|---:|---|---|
| Cam 1 | [`Eccentric_Cam_1.stl`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/STL/STL/Eccentric_Cam_1.stl) | 26.0 × 26.0 × 35.7 | 83 768 | bin | Wobble driver, phase A |
| Cam 2 | [`Eccentric_Cam_2.stl`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/STL/STL/Eccentric_Cam_2.stl) | 26.0 × 26.0 × 35.7 | 85 074 | bin | Wobble driver, phase B (120°) |
| Cam 3 | [`Eccentric_Cam_3.stl`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/STL/STL/Eccentric_Cam_3.stl) | 26.0 × 26.0 × 35.7 | 86 380 | bin | Wobble driver, phase C (120°) |
| Cam cap | [`Cam_Cap.stl`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/STL/STL/Cam_Cap.stl) | 18.0 × 18.0 × 14.7 | 4 086 | bin | Forms the 2nd half of each eccentric cam (×3), bolted on |

The three cams share **identical geometry**, phased 120° apart by the spur gears; the differing
triangle counts are tessellation noise between the three exported files.

### Cycloidal stage

| Part | File | X × Y × Z (mm) | Tris | Enc | Role |
|---|---|---|---:|---|---|
| Disc 1 | [`Cycloidal_Disk_1.stl`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/STL/STL/Cycloidal_Disk_1.stl) | 73.88 × 73.89 × 6.35 | 20 836 | bin | Cycloidal disc (anti-phase pair A) |
| Disc 2 | [`Cycloidal_Disk_2.stl`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/STL/STL/Cycloidal_Disk_2.stl) | 74.08 × 74.08 × 6.35 | 25 388 | bin | Cycloidal disc (anti-phase pair B) |
| Standoff | [`Cycloidal_Standoff.stl`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/STL/STL/Cycloidal_Standoff.stl) | 67.0 × 67.0 × 24.7 | 9 490 | bin | Spacer stack between disc sets / cam halves |

---

## 5. Bill of materials (non-printed)

The reducer requires its own bearings and fasteners, independent of the motor — the NEMA 17
adaptation removes none of them. Full quantities are in
[rv-gearbox-hardware.md](rv-gearbox-hardware.md). Summary:

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
([`RV.FCStd`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/CAD/RV.FCStd)).
It **cannot be recovered reliably from the STL surface mesh**: the disc lobes are geometrically
subtle and the exported rim tessellation is too coarse for a clean count, so a mesh-derived ratio
should not be trusted. Measure it empirically after assembly — the procedure is in
[02-motor-drive-gears.md § "Reduction ratio — measure it"](02-motor-drive-gears.md):

```
ratio = (input shaft turns) ÷ (output hub turns)
```

Then set `gear_ratio` in [code/pi/config.yaml](code/pi/config.yaml).

---

## 7. Testing — to be conducted

This section is reserved for **this project's own validation programme**. No results are reported
here yet; tests and their outcomes will be added as they are performed.

**Test platform:** the **NEMA 17** configuration (re-bored sun gear + adapter plate). All results are
specific to this build and are not transferable from any NEMA 23 reference.

| Test | Method | Status |
|---|---|---|
| Reduction ratio | count input turns ÷ output turns (hand-turned, then powered) | To be conducted |
| Backlash / lost motion | measure hub rotation on torque reversal under light load | To be conducted |
| Torque capacity | incremental load / stall test at the output hub | To be conducted |
| Continuous-run durability | 24/7 run at a representative load; log time, cycles, temperature | To be conducted |
| Wear inspection | disassemble after the run; inspect cams, discs, pins, gears | To be conducted |
| Efficiency | measure input vs output power under load | To be conducted |

Results, conditions (material, lubrication, motor, ambient), and dates will be recorded here when
available.

---

## 8. File inventory (source)

Provided in [`RV Reducer Files-…/`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/):

```
RV Reducer Files/
├── CAD/
│   ├── RV.FCStd                     ← FreeCAD parametric source (tooth/lobe counts live here)
│   ├── update_rv_gearbox.FCStd
│   └── RV_Reducer_Final_Assembly.step
├── STL/                             ← per-part .stl + .3mf project plates
│   └── STL/                         ← canonical per-part meshes (13 .stl files, used in §4)
└── 3D Printed Planetary Cycloidal Hub Gearbox…en-orig.srt   ← source-video captions
```

Notes:
- The `STL/STL/` subfolder holds the **13 individual part meshes** referenced throughout this
  document; the parent `STL/` also contains grouped `.3mf` print-plate files (e.g.
  [`caps.3mf`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/STL/caps.3mf),
  [`cam_cycloco_disk.3mf`](../../RV%20Reducer%20Files-20260616T132445Z-3-001/RV%20Reducer%20Files/STL/cam_cycloco_disk.3mf)).
- **Skip** `Sun_Gear` and `Gearbox_Base_Open` — replaced by the NEMA-17 sun gear and the closed base
  (see [02-motor-drive-gears.md](02-motor-drive-gears.md)).

---

## 9. See also

- [02-motor-drive-gears.md](02-motor-drive-gears.md) — motor/driver choice, NEMA 17 fitting, print
  settings, empirical ratio measurement.
- [rv-gearbox-hardware.md](rv-gearbox-hardware.md) — full bearing + fastener BOM with quantities.
- [shopping-list.md](shopping-list.md) — what to buy, where.
- [assembly-teardown-checklist.md](assembly-teardown-checklist.md) — build / re-build steps.
- [README.md](README.md) — rig overview and how the reducer fits the overhead 360° spinner.
