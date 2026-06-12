# theoria — project memory

Monorepo for Blender add-ons targeting scientific visualization and rendering.

## Layout

```
theoria/
├── README.md                  ← project overview
├── CLAUDE.md                  ← this file (project memory)
├── .gitignore
├── eidolon/                   ← Blender Orbital Renderer add-on
│   ├── __init__.py            ← registration, panels, property groups
│   ├── recipe_loader.py       ← render_recipe.json + .cube reader
│   ├── marching_cubes.py      ← pure Python marching cubes
│   ├── isosurface.py          ← grid → Blender mesh
│   ├── molecule.py            ← atom spheres + bond cylinders
│   ├── materials.py           ← CPK colors + material presets
│   ├── render_setup.py        ← camera + lights auto-placement
│   ├── operators.py           ← LoadRecipe, UpdateIsovalue, Render, etc.
│   ├── style_loader.py        ← style file load/save/apply
│   ├── preferences.py         ← add-on preferences panel
│   ├── test_recipe.json       ← test recipe (points to tests/water.cube)
│   └── example_style.json     ← example publication style
├── render_orbital.py          ← CLI headless batch renderer
├── tests/
│   └── water.cube             ← test fixture (H₂O, MO 5, 32³ grid)
└── docs/
    └── adr/
        ├── 0001-grid-evaluation-boundary.md
        └── 0002-marching-cubes-implementation.md
```

## Add-ons

### Eidolon (Orbital Renderer)

Blender add-on for publication-quality molecular orbital isosurface rendering.

- Consumes `render_recipe.json` + Gaussian `.cube` files
- Produced by companion [orbital-visualizer.py](https://github.com/josemazo/sci-scripts/blob/main/Visualization/orbital-visualizer.py) (in separate repo: `sci-scripts`)
- Marching cubes runs in Blender on pre-computed grid (no numba/SciPy needed)
- Two-tab UI: Orbitals (load recipe, browse MOs, adjust isovalues) + Render (camera, lights, engine, output)
- CLI headless mode: `blender --background --python render_orbital.py -- --recipe ... --style ... --output ...`
- Style files capture render presets for reproducible figures

## Conventions

- **Blender target:** 4.0+ (bpy API)
- **Dependencies:** numpy only (ships with Blender)
- **License:** GPLv3 (incorporates code from Molecular Blender and batoms)
- **Packaging:** Each add-on is a self-contained directory installable via symlink or zip into Blender's `scripts/addons/`
- **Namespacing:** Classes/operators prefixed `OR_` for Eidolon; future add-ons use their own prefix
- **Imports:** Use relative imports (`from . import module`) within add-on packages
- **Style:** Type hints encouraged, Blender add-on conventions (register/unregister, PropertyGroup, operators, panels)

## Third-party reference code (not in this repo)

These GPLv3 projects provided extracted code — they are NOT part of this repo:

- [Molecular Blender](https://github.com/smparker/molecular-blender) — marching cubes, cube reader, periodic table
- [Beautiful Atoms / batoms](https://github.com/beautiful-atoms/beautiful-atoms) — material system, UI patterns, render settings

For development reference, clone them alongside this repo.

## Companion tools (external)

- [orbital-visualizer.py](https://github.com/josemazo/sci-scripts/blob/main/Visualization/orbital-visualizer.py) — PyQt6/vispy app for browsing MOs, evaluating grids, exporting `.cube` files + `render_recipe.json`. Lives in `sci-scripts` repo.
