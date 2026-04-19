# Rendering preview figures

The README of this repository embeds three placeholder SVG previews:

- `doc/kistar_hand_placeholder.svg`
- `doc/kistar_son_right_placeholder.svg`
- `doc/kistar_son_left_placeholder.svg`

To replace them with **real renders** (à la
[KIST-PRIME-Lab/dex-urdf](https://github.com/KIST-PRIME-Lab/dex-urdf)),
follow one of the two workflows below.

---

## Option 1 — MuJoCo viewer screenshot (fastest)

```bash
pip install mujoco

python -m mujoco.viewer --mjcf=robots/hands/kistar_hand/kistar_hand.xml
```

In the viewer, press **F12** (or use the screenshot icon in the rendering tab)
to save a PNG. Move the file into `doc/` and update the README image paths,
e.g. replace `kistar_hand_placeholder.svg` → `kistar_hand.png`.

## Option 2 — SAPIEN ray-traced animation (same as dex-urdf)

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

Then copy `output/kistar_hand.{mp4,png}` into this `doc/` directory and update
the table in the root [`README.md`](../README.md).

---

## Option 3 — yourdfpy (lightweight Python)

```python
import yourdfpy
import numpy as np

robot = yourdfpy.URDF.load("robots/hands/kistar_hand/kistar_hand.urdf")
robot.show()  # opens an interactive viewer; use the export button for PNG
```
