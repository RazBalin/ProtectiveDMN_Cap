# ProtectiveDMN_Cap

A custom 3D-printable protective cap for the re-sealable chronic craniotomy used in the murine Default Mode Network (DMN) electrophysiology protocol described in the accompanying *Journal of Visualized Experiments* (JoVE) manuscript. The cap shields the head-mount cavity from mechanical impact, dust, bedding debris, and cage-mate contact between recording sessions, while preserving animal welfare (light, streamlined design, minimally restrictive).

This repository is the open hardware companion to the JoVE protocol and is released so that any laboratory can reproduce the cap verbatim.

---

## Contents

| File | Purpose |
|---|---|
| `tinker.obj` | Wavefront OBJ mesh of the cap, exported from Autodesk Tinkercad. Units are millimetres, 1:1 scale, no scaling required on import. Bounding box ≈ 106 × 89 × 20 mm (X × Y × Z). |
| `obj.mtl` | Material/colour metadata referenced by `tinker.obj`. Required only for colour-accurate visualisation in a mesh viewer; slicers ignore it. |
| `README.md` | This file — includes the recommended print settings. |
| `LICENSE` | Terms of use (Creative Commons Attribution 4.0). |

---

## Target hardware and material

- **Printer:** Anycubic Photon Mono 4K (6.23″ 4K monochrome LCD, 3840 × 2400 px, ≈ 35 µm XY pixel pitch, 405 nm parallel-matrix UV source, build volume 132 × 80 × 165 mm).
- **Resin:** Anycubic High Clear UV Resin, SKU **STMGCL-102B**. 405 nm DLP/LCD compatible.
- **Slicer:** Anycubic Photon Workshop (native `.pwma`), Lychee Slicer, or ChiTuBox. Any slicer with a Mono 4K profile will work.
- **Post-processing:** Anycubic Wash & Cure 2.0/3.0 or equivalent 95 %+ IPA bath + 405 nm post-cure station.

> **Biocompatibility note.** The cap sits on top of the already-cured dental-acrylic head mount and does not contact skin, mucosa, or the surgical site. Standard high-clear resin is appropriate here. Do **not** substitute this cap for any component that contacts tissue — those must be printed in certified biocompatible resin.

---

## Why clear resin?

Clear (un-pigmented) photopolymer was chosen for three practical reasons:

1. **Visual access.** During routine cage checks the experimenter can inspect the connector and cavity through the cap without removing it.
2. **Lower shrinkage anisotropy.** Pigment-free formulations shrink more uniformly on post-cure, which preserves the snap-on tolerance against the head mount base ring.
3. **Availability and cost.** STMGCL-102B is Anycubic's reference transparent resin and is sold alongside the Mono 4K, minimising supply-chain friction for replicating labs.

The trade-off is that clear resin has no opaque pigment to attenuate 405 nm light, so the cure depth per flash is larger than for grey/opaque formulations. Settings below are tuned to stop cure sharply at the layer boundary despite this.

---

## Reasoned print settings

Rationale is given per parameter so you can adapt intelligently if your local batch of resin or room temperature differs.

### Orientation and supports

| Parameter | Value | Rationale |
|---|---|---|
| Build-plate orientation | Tilt **25–35° around the X axis**, long axis aligned with printer X | A flat-on-plate 106 × 89 mm pancake generates very large FEP peel forces and risks layer shift. Tilting also reduces continuous cross-section per layer, improving peel behaviour and fine-feature fidelity. |
| Rotation around Z | Align the 106 mm extent with printer X (longer axis) | Build volume is 132 × 80 mm in XY; the 89 mm raw Y extent only fits once the model is tilted (tilt shortens Y footprint). Verify that the sliced bounding box fits inside 132 × 80 mm before printing. |
| Supports | Light-to-medium auto-generated, contact-tip **0.25 mm**, density medium | Enough to hold the tilted cap without marking the mating surface. **Manually delete any auto-support that lands on the inner fit surface** — support scars there will ruin the fit onto the head mount. |
| Raft | Enabled, Rhombic or Grid, 0.5 mm thickness | Improves first-layer adhesion for the tilted geometry. |

### Slicing / layer settings

| Parameter | Recommended value | Rationale |
|---|---|---|
| Layer height | **0.05 mm** | Balanced detail vs. time. Mm-scale structural features don't benefit from 0.03 mm; 0.10 mm would visibly step the curved rim. |
| Anti-aliasing | **On, 8×** (or 4× if slicer limited) | Smooths the 35 µm pixel stair-step on the curved outer shell. |
| Image blur | **3** | Works with AA; reduces visible pixel edges. |
| Grey level | **0** | Not needed with AA+blur; keep at 0 to avoid edge-softening on thin features. |

### Exposure — the critical settings

| Parameter | Recommended value | Rationale |
|---|---|---|
| **Normal exposure time** | **2.6 s** | Anycubic's reference range for clear on Mono 4K is 2.5–3.0 s. 2.6 s sits safely inside and minimises over-cure bleed that would close the cap's ventilation holes. Validate on first print; increase in 0.2 s steps if layers delaminate, decrease if fine features bloat. |
| **Bottom exposure time** | **35 s** | Adequate adhesion without the over-cured flared base that longer times (45–50 s) produce on clear resin (pigment-free resin cures deeper per flash, so bottom exposure does not need to be as long as for opaque grey). |
| Bottom layers | **6** | Enough to lock to the raft; more adds over-cure risk at the raft interface. |
| Transition layers | **8** | Smooth exposure ramp-down; avoids a visible seam between bottom and normal layers on the tilted geometry. |

### Motion and rest-time settings

Clear resin is especially sensitive to layer adhesion and FEP peel artefacts; rest times matter more than for opaque formulations.

| Parameter | Recommended value | Rationale |
|---|---|---|
| Lift distance | **6 mm** | Conservative for the wide cross-section; shorter lifts risk FEP stick. |
| Lift speed | **60 mm/min** | Slow enough to avoid tearing a large peel area; faster speeds cause suction failures on clear resin. |
| Retract speed | **150 mm/min** | Standard; no benefit to going slower on retract. |
| Rest time **before** lift | **1.0 s** | Lets the freshly cured layer relax before peel — reduces micro-cracks in thin features. |
| Rest time **after** lift | **0 s** | Not needed. |
| Rest time **after** retract | **1.5 s** | Lets resin re-level under the build plate before the next exposure; clear resin benefits noticeably from this (reduces horizontal banding on tilted flat surfaces). |
| Light-off delay | 0 s (Mono 4K firmware uses rest times instead) | Redundant with the rest times above. |

### Expected print time

At 0.05 mm layer height with 25–30° tilt, the sliced Z extent is ≈ 60–70 mm → ≈ 1200–1400 layers. At 2.6 s exposure plus ≈ 4 s motion overhead per layer, **expect roughly 1.5–1.8 h total print time**, dominated by motion rather than cure.

### Post-processing

1. **Drain** the print on the build plate for 2–3 minutes.
2. **First IPA wash** (95 % or higher): 3 minutes with mechanical agitation. Do **not** exceed 5 minutes total IPA exposure — prolonged IPA contact leaches plasticiser from clear resin, causing cloudiness and brittleness over weeks.
3. **Second IPA rinse**: 60 s in a cleaner bath to flush any resin residue from cavities and around supports.
4. **Compressed-air dry** the cavities; do not towel-wipe the uncured surface.
5. **Remove supports** while the part is still "green" (not yet fully post-cured) — supports snap off cleanly with minimal scarring. Light-sand (400 grit) any scars on cosmetic surfaces only, never on the inner fit surface.
6. **Post-cure**: 3–4 minutes in an Anycubic Wash & Cure station (or equivalent 405 nm UV chamber), **flipping the part halfway through**. Double-sided cure is preferable to longer one-sided cure for clear resin — it minimises optical yellowing and surface brittleness.
7. **Final inspection**: confirm the cap snaps cleanly onto a sample head-mount base ring before using on an animal.

---

## Complete settings summary

Copy-paste reference block for slicer configuration:

```
Printer:                Anycubic Photon Mono 4K
Resin:                  Anycubic High Clear UV Resin (STMGCL-102B)

Layer height:           0.05 mm
Anti-aliasing:          8x
Image blur:             3
Grey level:             0

Bottom layers:          6
Bottom exposure:        35 s
Transition layers:      8
Normal exposure:        2.6 s

Lift distance:          6 mm
Lift speed:             60 mm/min
Retract speed:          150 mm/min
Rest before lift:       1.0 s
Rest after lift:        0 s
Rest after retract:     1.5 s

Orientation:            tilt 25–35° around X, long axis along printer X
Supports:               light-to-medium, 0.25 mm tip, manual clear of mating surface
Raft:                   enabled, 0.5 mm thick
```

---

## Slicing workflow (Photon Workshop)

1. `File → Open` → select `tinker.obj`. Confirm units are millimetres and scale is 1.00. Do not rescale.
2. Rotate so the long axis (106 mm extent) aligns with the printer's X axis.
3. Apply a 30° tilt around the X axis.
4. Centre on the build plate and confirm the sliced XY footprint is within 132 × 80 mm.
5. Auto-support (medium), then manually delete supports contacting the inner fit surface.
6. Apply the exposure / motion settings from the table above.
7. Slice and export `.pwma`.
8. Transfer to the printer via USB and print.

---

## Troubleshooting

| Symptom | Likely cause | Fix |
|---|---|---|
| Layer delamination mid-print | Normal exposure too low, or lift speed too high | +0.2 s exposure, or drop lift speed to 50 mm/min |
| Loss of fine detail, closed vent holes | Over-cure — exposure too long or too much light bleed | −0.2 s exposure; confirm AA and blur are enabled |
| Print detaches from raft | Bottom exposure too short or raft too thin | +5 s bottom exposure, or raft 0.8 mm |
| Cap does not snap onto head mount | Over-cure expansion, or support scars on mating surface | Reduce normal exposure by 0.2 s; manually remove inner-surface supports before printing |
| Yellowing within days | Over post-cure or UV exposure in storage | Limit post-cure to 4 min total, store in opaque box |
| Cloudy finish | IPA exposure too long | Cap total IPA contact at ≤ 5 min |

---

## Citation

If you use this cap (or a derivative) in a publication, please cite the accompanying JoVE protocol (manuscript in revision, citation to be updated on publication) and, for the underlying re-sealable chronic head-mount architecture, Kisso *et al.* (2025), *bioRxiv* [10.1101/2025.11.13.688186](https://www.biorxiv.org/content/10.1101/2025.11.13.688186v1).

---

## License

This hardware design is released under the **Creative Commons Attribution 4.0 International License (CC BY 4.0)**. You are free to share and adapt the mesh for any purpose, including commercially, provided appropriate credit is given. See `LICENSE` for the full text.

---

## Contact

Issues, improvements, and pull requests are welcome via the GitHub issue tracker on this repository. For scientific questions about the surrounding protocol, please contact the corresponding author of the JoVE manuscript.
