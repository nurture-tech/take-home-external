# Gate Rally — external data bundle

This folder contains **static assets** and **authored game JSON** for **Gate Rally** (see `data/game/game-copy.json` for the canonical name and one-line purpose). Copy or merge these paths into a **Next.js** app that uses **Babylon.js** so URLs and imports match the layout described below.



https://github.com/user-attachments/assets/242d9bc4-ec7d-4593-a0d4-21a2ade83f62



## Expected stack

The source game is built with:

- **Next.js** 16.x and **React** 19.x — `public/` files are served from the site root (`/…`).
- **Babylon.js** (`@babylonjs/core`, `@babylonjs/loaders`, optional `@babylonjs/inspector`) — meshes are loaded with `SceneLoader` and paths like `/assets/…`.
- **@babylonjs/havok** — physics in the full game (optional if you only reuse assets or a simpler rig).

Typical companions in the reference app: **Leva** (debug/tuning UI), **Zod** (JSON validation), **Tailwind CSS** 4.

## Directory layout

```
take-home-external/
├── public/              # Next.js static files → URLs from /
│   └── assets/          # e.g. /assets/ground.glb, /assets/cars/<id>.glb
└── data/
    └── game/            # Course, placements, UI/chat copy (JSON)
```

## How to use the data

### `public/` (meshes, textures, manifests)

1. Place the `public` directory at the **root of your Next.js project** (merge with an existing `public/` if needed).
2. Files under `public/assets/` are available at **`/assets/…`** in the browser (no `public` prefix in the URL).
3. Car and prop manifests (`public/assets/**/*.json`, e.g. `public/assets/cars/*.json`) include **`model_path`** and **`image_path`** as site-root paths (e.g. `/assets/cars/foo.glb`). If you move files, update those fields **or** change your loader base URL and path construction in code so they still resolve.

The reference implementation loads GLBs with Babylon `SceneLoader` using a base of `/assets/` or `/assets/cars/` and filenames taken from those manifests.

### `data/game/` (course and copy)

| File | Role |
|------|------|
| `gates-course.json` | Gate positions/scales, default active gate count, per–gate-count lap order. |
| `ramp-placements.json` | Ramp instance transforms. |
| `ground-edges-placements.json` | Ground edge strip placements (visual rim). |
| `game-copy.json` | Game name, purpose, chat welcome/placeholder strings, UI strings (schema versioned). |

Import these JSON files from your app (for example `import course from '@/data/game/gates-course.json'`) **or** fetch them at runtime, as long as your **schemas** match what your code expects. In the reference app, **yaw** fields in course JSON are in **degrees**; the app converts to **radians** for Babylon and internal math.

If you keep the same relative paths (`data/game/...`), you can align TypeScript path aliases (`@/data/game/...`) with your consumer project.

## Syncing from the main repo

To refresh this bundle from the canonical Gate Rally app repository, copy:

- `public/` → `public/`
- `data/game/` → `data/game/`

Preserve directory structure so `/assets/...` links in JSON stay valid.
