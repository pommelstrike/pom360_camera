# Assembly Guide

Build order for the reducer, derived from the source build sequence. Part roles and measured
dimensions are in [technical-description.md](technical-description.md); hardware in
[bill-of-materials.md](bill-of-materials.md); slicing in [print-settings.md](print-settings.md).

## 1 — Prepare parts & hardware

- Print/fabricate all 13 parts (see [print-settings.md](print-settings.md)).
- Press **heat-set inserts** into the housing and hub parts (16× M3) so the unit is re-buildable.

## 2 — Build the three eccentric-cam sub-assemblies

- Seat a **20×27×4 bearing** on each eccentric cam (3 total).
- Fit each cam into its bore; the **`Cam_Cap`** forms the second half of each eccentric cam and is
  bolted on.

## 3 — Cycloidal stage

- Install the first **cycloidal disc** with its **15×24×5 roller bearings**; adding the first disc
  links all three eccentric cams together.
- Add the second disc: install another set of bearings, then the second `Cam_Cap` halves, bolted on.
- Use the **`Cycloidal_Standoff`** spacers to set the disc/cam stack height.

## 4 — Output hub

- Fit the **45×58×7** main bearing and assemble the **output hub** + `Hub_Retainer` + `Hub_Cap`.

## 5 — Motor input

- Slide the helical-spur **`Sun_Gear_NEMA17`** onto the motor shaft (5 mm bore + M3 grub screw).
- Mount the motor to the gearbox via the **`Adapter_NEMA17_to_NEMA23`** plate and heat-set inserts.

## 6 — Close up

- Fit the **`End_Cap`** faceplate; run through-bolts through the full stack to clamp everything.
- Add gel lubricant to the cycloidal cavity before the first long run.

## 7 — Verify

- Hand-turn the input and confirm the output hub rotates smoothly with no binding.
- Measure the reduction ratio empirically (see [print-settings.md](print-settings.md)).
