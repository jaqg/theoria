# Theoria

Blender add-ons for scientific visualization, simulation data import, and publication-quality rendering.

> **Monorepo** — each add-on lives in its own directory. Shared testing, shared conventions, shared style system.

---

## Add-ons

### Eidolon — Orbital Renderer (`eidolon/`)

Molecular orbital isosurface rendering from Gaussian cube files and render recipes.

| | |
|---|---|
| **Input** | `render_recipe.json` + `.cube` files (from [orbital-visualizer.py](https://github.com/josemazo/sci-scripts/blob/main/Visualization/orbital-visualizer.py)) |
| **UI** | Two-tab sidebar panel: Orbitals (load recipe, browse MOs, isovalue sliders, material pickers) + Render (camera, lights, engine, resolution) |
| **CLI** | `blender --background --python render_orbital.py -- --recipe r.json --style s.json --output ./renders/` |
| **Style** | JSON style files (like mplstyle) capture camera, lighting, material, and resolution presets for reproducible figures |
| **Blender** | 4.0+ |
| **License** | GPLv3 |

[Full docs →](eidolon/)

#### Quick install

```bash
# Symlink into Blender's add-ons directory
ln -s $(pwd)/eidolon ~/.config/blender/4.X/scripts/addons/eidolon
```

Then enable "Orbital Renderer" in Blender → Edit → Preferences → Add-ons.

#### Quick render (CLI)

```bash
blender --background --python render_orbital.py -- \
    --recipe eidolon/test_recipe.json \
    --output ./renders/
```

---

## Conventions

- **Target:** Blender 4.0+ (bpy API)
- **Dependencies:** numpy only (ships with Blender) — no pip install
- **License:** GPLv3
- **Install:** Symlink add-on directory into `~/.config/blender/4.X/scripts/addons/`
- **Namespacing:** Each add-on uses a unique prefix for class/operator IDs (Eidolon uses `OR_`, `orbital_renderer.*` operator IDs)
- **Imports:** Relative imports within each add-on package

---

## Third-party sources (not in this repo)

This repo incorporates extracted/adapted code from:

- [Molecular Blender](https://github.com/smparker/molecular-blender) (Shane Parker, Joshua Szekely — GPLv3) — marching cubes algorithm, cube file reader, periodic table data
- [Beautiful Atoms / batoms](https://github.com/beautiful-atoms/beautiful-atoms) (Xing Wang, Beautiful Atoms Team — GPLv3) — material creation system, UI patterns, render settings patterns

For development reference, clone these alongside this repo.

---

## Companion tool

**[orbital-visualizer.py](https://github.com/josemazo/sci-scripts/blob/main/Visualization/orbital-visualizer.py)** — PyQt6/vispy desktop application for browsing MOs from GAMESS `.log` files, evaluating volumetric grids (numba-accelerated), and exporting `.cube` files + `render_recipe.json`. Lives in the [sci-scripts](https://github.com/josemazo/sci-scripts) repo.
