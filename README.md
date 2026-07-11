# OCV — OpenFOAM Case Viewer

> A free, browser-based pre-flight check for vehicle CFD cases.
> Catch a misnamed boundary or an out-of-band CFL **before** the cluster eats your night.

Live: **[ocv.13studio.co](https://ocv.13studio.co)** · Source: `13studio-sudo/ocv` · License: MIT · Edition 0.0.10

---

## Why this exists

I came to CFD from fine arts, pulled in by a love of motorsport. The math and the software were daunting at first — pages of dictionaries, opaque switches, and the very real prospect of burning a night of cluster time on a typo. Pair-programming with Claude let me refine simulation models and boundary conditions by intuition rather than by incantation, and OCV is what I built along the way.

The hope behind it is simple: every maker — racer, student, hobbyist, indie engineer — should eventually be able to run their own simulations with the same ease.

---

## What it does

Drop a zipped OpenFOAM case onto the page. OCV unpacks it in your browser, parses every dictionary it can find — `controlDict`, `fvSchemes`, `fvSolution`, `blockMeshDict`, `snappyHexMeshDict`, `forceCoeffs`, `momentumTransport`, `transportProperties`, `0/U`, and the rest — renders the geometry in 3D, and lays the parsed values out across twelve dashboard sections. A one-click validator checks Mach, CFL, refinement levels, blockage ratio, BC coverage, and other vehicle-CFD rules informed by Chien & Wang (2024).

What you see, at a glance:

- **Geometry / layers** — every named `triSurface` group, toggleable, with vert and tri counts
- **3D viewport** — orbit / pan / zoom, axes, dimension labels, multi-view cameras, solid + wireframe overlay
- **Workflow pipeline** — blockMesh → snappyHexMesh → solver → forces, with stage status
- **Turbulence · time · solver** — RANS / LES model, scheme orders, corrector counts
- **Governing equations** — Navier-Stokes, S-A DDES, DDES length scale, rendered alongside your case
- **Boundary patches** — every patch from `blockMeshDict` reconciled against `0/U`
- **Parameters · CFL** — Re, U∞, ν, ΔT, CFL_surface, CFL_wake
- **Mesh setup** — base cell, refinement levels, refinement box, predicted cell counts
- **Aerodynamics** — `forceCoeffs` reference area, length, dirs
- **Initial conditions · mesh quality · function objects · parallel decomposition** — optional sections

Nothing leaves your laptop. No upload server, no account, no cookie. A single static HTML file does the work entirely client-side, which makes OCV usable on locked-down clusters, in motorsport teams with proprietary geometry, and in academic groups with sensitive cases.

---

## Scope

OCV is tuned for **vehicle aerodynamics** — bluff-body external flow at Re ≈ 10⁶–10⁷, U∞ up to 100 m/s, incompressible air, snappyHexMesh on a `triSurface` body inside a rectangular wind tunnel. Other regimes still parse and display; the rules just become less informed.

---

## Getting started

### Option 1 — use it online
Open [ocv.13studio.co](https://ocv.13studio.co), click **Demo** for the bundled motorBike LES case, or drag any zipped OpenFOAM case onto the window.

### Option 2 — run it locally
Clone the repo and serve the folder; everything is static.

```bash
git clone https://github.com/13studio-sudo/ocv.git
cd ocv
python3 -m http.server 8080
# open http://localhost:8080
```

No build step, no dependencies to install. The runtime libraries (Three.js, JSZip, pako) come from CDN.

### Option 3 — deploy it
The included `vercel.json` is the only config you need. Push to GitHub, import the repo into Vercel, done.

---

## Tutorial cases

A few reference cases ship with OCV (or are one click away):

| Case | Type | Source |
|---|---|---|
| motorBike LES — bundled demo | S-A DDES · pisoFoam · U=100 m/s | `LES_RL9-10_v100ms.zip` (Chien & Wang 2024 reference) |
| motorBike LES — OpenFOAM official | transient, RANS precursor → DDES | hosted in `/tutorials/` |
| motorBike steady RANS — OpenFOAM official | k-ω SST · simpleFoam | hosted in `/tutorials/` |
| pitzDaily | backward-facing step, k-ω SST | OpenFOAM-12 via `download-directory.github.io` |
| cavity | 2D lid-driven, icoFoam | OpenFOAM-12 via `download-directory.github.io` |
| windAroundBuildings | urban CFD, ABL over a city block | OpenFOAM-12 via `download-directory.github.io` |

Open **Setup → Tutorial Cases** inside the app to load any of them.

---

## Project layout

```
ocv/
├── index.html                    # the entire app — vanilla JS, single file
├── LES_RL9-10_v100ms.zip         # bundled motorBike LES demo case
├── tutorials/
│   ├── motorBike_LES.zip         # OpenFOAM-12 official, repackaged with geometry
│   └── motorBikeSteady_RANS.zip  # OpenFOAM-12 official, repackaged with geometry
├── vercel.json                   # static-host config + redirects
├── CNAME                         # ocv.13studio.co
└── LICENSE                       # MIT
```

---

## Built with, indebted to

- **OpenFOAM** — every dictionary key OCV reads
- **Three.js r128 · JSZip 3.10 · pako 2.1** — viewport, archive, gzip
- **GitHub · Vercel** — source and static hosting
- **Autodesk Fusion 360** — demo motorbike geometry
- **Claude (Anthropic)** — pair-programmer for this codebase
- **Chien & Wang (2024)**, *doi:10.1093/jom/ufae025* — vehicle-CFD validation rules

---

## Contributing

MIT-licensed, free, no paid tier. PRs for additional parsers, validation rules, turbulence models, and translations are all welcome. If OCV mis-parses your case, open an issue with a minimal reproducer — a zipped case folder is the most useful thing you can attach.

---

© Project OCV · SHIBAYAMA RACING · design & development by [13studio](https://13studio.co)
