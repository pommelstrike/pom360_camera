# Print Settings

How to fabricate the 13 parts. Part dimensions and roles are in
[technical-description.md](technical-description.md); the buy-list is in
[bill-of-materials.md](bill-of-materials.md).

## Material by part

| Part group | Recommended | Also works |
|---|---|---|
| Eccentric cams (×3) | **SLA resin** — SirayaTech Nylon Black (tight tolerances) | FDM Nylon / PA-CF |
| Cycloidal discs (×2) | **laser-cut acrylic** | FDM (fine layers) |
| Housing, caps, hub, standoffs | **PETG / ASA** | PLA+ (full-PLA build is verified) |
| Sun gear | Nylon ideal | PLA / PETG / ASA |

> A complete all-FDM PLA build has been verified to run, so the gearbox is not locked to resin or
> laser tools.

## Slicing

- **Cams + cycloidal discs:** fine layers (0.12–0.16 mm), calibrated flow/dimensions, **≥4
  perimeters**, **≥40 % infill**. Sloppy prints bind — accuracy matters most here.
- **`Sun_Gear_NEMA17`:** orient **gear axis = Z** (teeth strong); 5 mm bore + M3 set-screw to the
  shaft.
- **Adapter plate:** solid, 4 perimeters.

## Tolerances

The cycloidal discs and eccentric cams must print accurately or the stage binds. Calibrate
flow/dimensions on these parts before printing the final set.

## Reduction ratio — measure it

The exact ratio can't be read from the mesh; measure empirically after assembly:

```
ratio = (input shaft turns) ÷ (output hub turns)
```

See [technical-description.md §6](technical-description.md) for why, and
[assembly-guide.md](assembly-guide.md) for the build order.
