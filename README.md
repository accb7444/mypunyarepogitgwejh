# Three-Body Problem Simulator

A browser-based N-body gravitational simulation built on HTML5 Canvas with a Velocity-Verlet integrator. Drag bodies, edit masses, toggle visualization layers, and explore four presets ranging from the stable Figure-8 choreography to chaotic three-body motion.

![Three-Body Problem Simulator screenshot](media/screenshot.png)

*(Add a screenshot to `media/screenshot.png` and the badge above will render automatically.)*
wtfguhnjmok,l./
---

## What it is

The three-body problem has no general closed-form solution — unlike the two-body Kepler problem, three mutually gravitating masses produce chaotic, non-repeating orbits except for a handful of specially chosen initial conditions. This simulator lets you play with those special cases and watch chaos unfold in real time.

It is a single-file demo: open `3bodya.html` in any modern browser. No bundler, no server, no dependencies.

## Quick start

```bash
# Clone and open
git clone https://github.com/accb7444/mypunyarepogitgwejh.git
cd mypunyarepogitgwejh
open 3bodya.html            # macOS
xdg-open 3bodya.html        # Linux
start 3bodya.html           # Windows
```

Or just drag `3bodya.html` into your browser. Everything runs client-side.

## Features

- **Physics**
  - Velocity-Verlet (symplectic, second-order) N-body integration
  - Softened gravity to avoid the 1/r² singularity
  - Gravity toggle (turn it off to watch inertial motion)
- **Bodies**
  - Three labelled masses: α Alpha, β Beta, γ Gamma
  - Per-body mass sliders (20–400, where 100 ≈ 1 M☉ in display units)
  - Per-body visibility toggle
  - Drag any body to reposition it mid-simulation
- **Presets**
  - **Figure-8** — the Chenciner–Montgomery choreography; three equal masses trace the same figure-8 path 120° out of phase
  - **Lagrange △** — three equal masses at the vertices of an equilateral triangle, rotating rigidly about their centre of mass
  - **Binary + Planet** — two heavy stars in a tight binary orbited by a lighter third body
  - **Chaotic** — an asymmetric, visually busy configuration
- **Visualization**
  - Orbital trails with gradient-fade bands
  - Velocity vectors (green arrows, draggable tips to edit velocity directly)
  - Gravity force vectors (blue arrows, scaled by net acceleration)
  - Centre-of-mass crosshair
  - Starfield background, optional grid
  - Real-time energy panel: kinetic, potential, total, linear momentum (toggle with the ⚡ checkbox)
- **Playback**
  - Play / pause / step-through / reset
  - Three speed tiers (slow / normal / fast)
  - Timer display in "Earth Days" (calibrated so the Figure-8 orbit ≈ 365 days)
  - Zoom slider, +/- buttons, and scroll-wheel zoom
  - Click-drag on empty canvas to pan
- **Keyboard**
  - `Space` — play / pause
  - `R` — reset to the active preset
  - `G` — grid toggle
  - `V` — velocity toggle
  - `F` — force toggle

## How it works

### Units

The simulation uses its own internal units, calibrated so the visually familiar quantities line up:

| Quantity | Symbol | Value |
|---|---|---|
| Gravitational constant | G | 50 px³ τ⁻² mass⁻¹ |
| Softening ε² | — | 25 px² (ε = 5 px) |
| Base mass ("1 M☉") | BASE_MASS | 100 sim-mass units |
| Velocity-arrow scale | VEL_SCALE | 18 px per px/τ |
| Time → Earth Days | DAYS_PER_TAU | 1.442 days/τ |

Orbital speed for a circular orbit at radius r around mass M: v_circ = √(G·M / r). At r = 200 px, M = 100, that gives v = 5 px/τ, which yields an orbital period of about 251 τ — and the Figure-8 choreography at ≈ 253 τ maps to roughly 365 Earth Days.

### Preset scales

Each preset factory converts from natural/analytic units into simulation units:

- **Figure-8**: the original Chenciner–Montgomery initial conditions use G = 1, m = 1, positions ≈ ±1. Scaling factors: L × 200 px, V × 5 px/τ.
- **Lagrange**: computed from the force-balance condition ω² = 3 G m / s³ where s is the triangle side length.
- **Binary + Planet**: binary angular speed ω_b² = G M_star / (4 d³), planet speed v_p = √(G · 2 M_star / r_p) × 0.97 (slightly eccentric).
- **Chaotic**: hand-tuned empirical positions and velocities for visual interest.

### Integration

`verletStep(dt)` runs one Velocity-Verlet step:

1. x(t+dt) = x(t) + v(t)·dt + ½ a(t)·dt²
2. Recompute accelerations a(t+dt) from the new positions
3. v(t+dt) = v(t) + ½ [a(t) + a(t+dt)]·dt

Because Velocity-Verlet is symplectic, it conserves a shadow Hamiltonian — energy drifts far less than with naive Euler integration, which is why the energy panel stays flat over long runs.

### Rendering

Every animation frame:

1. Clear canvas, draw starfield
2. Optionally draw grid and trails
3. Optionally draw force and velocity arrows
4. Optionally draw the centre-of-mass crosshair
5. Draw each body as a radial-gradient sphere with a soft glow and a label
6. Update the HUD timer and, if enabled, the energy readout

The loop is capped at ~111 fps (`t - lastT < 9`) to keep the physics deterministic across machines and to avoid burning CPU when the tab is backgrounded.

## File structure

```
mypunyarepogitgwejh/
├── 3bodya.html     # Main page — layout, controls, canvas
├── 3bodya.css      # Dark glass-panel theme, layout, controls
├── 3bodya.js       # Physics engine, renderer, interaction, presets
├── README.md       # This file
├── LICENSE         # MIT license
├── .gitignore      # Ignores OS and editor junk
└── media/          # Screenshots and assets (gitignored)
    └── screenshot.png  # (Add your own)
```

## Screenshots

| Preset | What you see |
|---|---|
| Figure-8 | Three equal yellow/blue/orange bodies chasing each other around a figure-8 loop |
| Lagrange △ | Three bodies at the corners of a rotating equilateral triangle |
| Binary + Planet | Two bright stars orbiting each other with a smaller planet looping wider out |
| Chaotic | Three bodies on wild, non-repeating trajectories |

## Browser compatibility

Works in any browser that supports `CanvasRenderingContext2D`, `requestAnimationFrame`, and CSS `backdrop-filter`. Tested on recent Chrome, Firefox, and Edge. Mobile touch drag is supported for panning and body dragging.

## License

MIT — see [LICENSE](LICENSE).

## Author

Created with Claude (Anthropic). Maintained by [accb7444](https://github.com/accb7444).

---

*If you add a screenshot, drop it in `media/screenshot.png` and the badge at the top of this README will render automatically.*
