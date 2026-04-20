# Rendering preview figures

The root [`README.md`](../README.md) embeds six rendered previews:

- `doc/kistar_hand.{gif,png}`
- `doc/kistar_son_right.{gif,png}`
- `doc/kistar_son_left.{gif,png}`

All six are generated directly from the `.xml` MJCF files under
[`robots/hands/`](../robots/hands/).  Three workflows are supported.

---

## Option 1 — `tools/render_previews.py` (recommended, default)

Single command regenerates every GIF and PNG using MuJoCo's offscreen renderer.
Works on Windows, macOS, Linux; no DISPLAY required.

```bash
pip install mujoco imageio numpy

python tools/render_previews.py
```

Useful flags:

```bash
# Render only one model
python tools/render_previews.py --only kistar_hand

# Custom resolution / length
python tools/render_previews.py --width 960 --height 720 --frames 180 --fps 30
```

Internally the script:

1. Wraps each MJCF in a temporary file that adds a gradient sky-box and
   a brighter headlight (so the repository models stay unchanged).
2. Sweeps every position actuator through 60 % of its `ctrlrange` on a cosine
   schedule, analytically propagating the `<equality joint>` mimic constraints.
3. Orbits the camera ±25 ° around the palm.
4. Writes a looping GIF plus a mid-cycle still PNG to `doc/`.

---

## Option 2 — MuJoCo viewer screenshot (quickest one-off)

```bash
pip install mujoco

python -m mujoco.viewer --mjcf=robots/hands/kistar_hand/kistar_hand.xml
```

In the viewer, drive the joints with the **Control** sliders, then press **F12**
or use the camera icon in the render tab to save a PNG.  Move the file into
`doc/` and adjust the image path in the root README if necessary.

---

## Option 3 — SAPIEN ray-traced animation (dex-urdf style)

Highest visual quality but heavy dependencies. Matches the render style of
[KIST-PRIME-Lab/dex-urdf](https://github.com/KIST-PRIME-Lab/dex-urdf).

```bash
git clone https://github.com/KIST-PRIME-Lab/dex-urdf
cd dex-urdf
pip install -r requirements.txt

python tools/generate_urdf_animation_sapien.py \
    ../KISTAR_URDF/robots/hands/kistar_hand/kistar_hand.urdf \
    --output-video-path output/kistar_hand.mp4 --headless

python tools/generate_urdf_collision_figure_sapien.py \
    ../KISTAR_URDF/robots/hands/kistar_hand/kistar_hand.urdf \
    --output-image-path output/kistar_hand.png --headless
```

Then copy `output/kistar_hand.{mp4,png}` into this `doc/` folder and update the
image paths in the root README.
