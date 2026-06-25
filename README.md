# pom360_camera
nema 17 Stepper Motor with Planetary Cycloidal Hub Gearbox
orginal prints were for nema 23 and drafted to fit nema 17
# Barkstudio 360 Booth

A DIY full-body 360° photo + video booth for 3–4 people. Stationary platform, spinning arm. Two modes: smooth orbit (wedding/event style) and Glam Bot slow-mo pass (red-carpet style). Green screen background, real-time chroma key, private LAN gallery for guests.

---

## Hardware Wishlist

| Component | Choice | Status |
|-----------|--------|--------|
| Camera | iPhone 12 + Blackmagic Camera  | Owned |
| Brain | Raspberry Pi 3 | Owned |
| Compute | Asus Predator laptop | Owned |
| Lighting | Existing LED panels + green screen | Owned |
| Platform | 48" Baltic birch ¾" plywood disc | To build |
| Column | 1½" black iron pipe, 42" | To buy |
| Bearing | 12" heavy-duty lazy Susan | To buy |
| Arm | TBD — repurposing extendable arm if rigid enough | Pending |
| Motor | TBD (worm-gear DC vs. NEMA 23 stepper) | Pending |
| Drive | GT2 timing belt, ~5:1 reduction | Pending |

Target arm radius: **1.8–2.0m** (~6 ft) for full-body framing of 3–4 people in portrait orientation.

---

## System Architecture

```
┌──────────────────────────────────────────────────────────┐
│  ASUS PREDATOR (Laptop)                                  │
│                                                          │
│  ┌──────────────────┐    ┌──────────────────────────┐   │
│  │ Operator Panel   │    │ Guest Gallery (LAN)      │   │
│  │ (localhost:3000) │    │ booth.local/{session}    │   │
│  └────────┬─────────┘    └──────────┬───────────────┘   │
│           │                          │                   │
│           └──────────┬───────────────┘                   │
│                      ▼                                   │
│           ┌──────────────────────┐                       │
│           │  Node.js Server      │                       │
│           │  - Session manager   │                       │
│           │  - File watcher      │                       │
│           │  - OBS WebSocket     │                       │
│           └──────┬───────┬───────┘                       │
│                  │       │                               │
│                  ▼       ▼                               │
│           OBS Studio   Watch folder                      │
│           (chroma key) (chroma'd clips)                  │
└──────────┬───────────────────────────┬───────────────────┘
           │                           │
           │ HTTP                      │ Auto-receive
           │ (LAN)                     │ (AirDrop / Quick Share)
           ▼                           ▼
┌────────────────────┐         ┌────────────────────┐
│  Raspberry Pi 3    │         │  iPhone 12         │
│                    │         │                    │
│  FastAPI server    │         │  Blackmagic Camera │
│  - /spin endpoint  │ BT HID  │  + auto-share to   │
│  - GPIO motor ctl  ├────────►│  laptop watch dir  │
│  - BT shutter      │ shutter │                    │
│  - Limit switch    │         │  (Mounted on arm)  │
└────────┬───────────┘         └────────────────────┘
         │ GPIO
         ▼
   Motor Driver
         │
         ▼
   Motor → Belt → Hub → Arm → iPhone
```

---

## Timeline

- [x] Platform design — 48" Baltic birch, Y-frame underneath
- [x] Column + bearing assembly spec
- [x] Operator panel UI design (`public/operator.html`)
- [x] Guest gallery UI design (`public/gallery.html`)
- [ ] Cut + finish platform
- [ ] Buy + assemble column + bearing
- [ ] Decide arm (repurpose or buy V-slot)
- [ ] Pick + buy motor + driver
- [ ] Wire Pi → driver → motor + e-stop
- [ ] Write Pi FastAPI server (`pi/booth_controller.py`)
- [ ] Write laptop Node.js server (`laptop/src/server.js`)
- [ ] OBS scene + chroma key + WebSocket scripting
- [ ] Bluetooth HID shutter trigger from Pi → iPhone
- [ ] Network setup (Windows hotspot, mDNS)
- [ ] End-to-end test

---

## Project Structure

```
barkstudio-360/
├── README.md                  ← you are here
├── docs/
│   ├── 01-hardware-bom.md     ← full parts list + budget
│   ├── 02-platform-build.md   ← 48" Baltic birch build
│   ├── 03-column-bearing.md   ← pipe + lazy Susan assembly
│   ├── 04-architecture.md     ← detailed system diagram + flows
│   └── 05-network-setup.md    ← LAN, hotspot, mDNS, OBS
├── public/
│   ├── gallery.html           ← guest-facing gallery mockup
│   └── operator.html          ← booth control panel mockup
├── pi/
│   ├── README.md
│   ├── requirements.txt
│   └── booth_controller.py    ← FastAPI motor + BT shutter
└── laptop/
    ├── package.json
    ├── README.md
    └── src/
        └── server.js          ← Express server for ops + gallery
```
