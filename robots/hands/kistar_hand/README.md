# KISTAR Hand (Ver2)

4-finger anthropomorphic hand developed at
[Korea Institute of Science and Technology](https://www.kist.re.kr).

| Field            | Value                                       |
| ---------------- | ------------------------------------------- |
| Author           | Jaesung Lee                                 |
| Affiliation      | Korea Institute of Science and Technology   |
| License          | BSD-3-Clause                                |
| Last update      | 2026-04-19                                  |
| URDF             | [`kistar_hand.urdf`](kistar_hand.urdf)      |
| MuJoCo MJCF      | [`kistar_hand.xml`](kistar_hand.xml)        |
| Mesh format      | binary STL (`meshes/kistar/`)               |

---

## Files

```
kistar_hand/
├── kistar_hand.urdf            # original URDF (Made by Jaesung Lee, KIST)
├── kistar_hand.xml             # MuJoCo MJCF auto-derived from the URDF
├── README.md
└── meshes/kistar/
    ├── mount.STL
    ├── Palm.STL
    ├── thp1.STL                # thumb base motor
    ├── thumb_link_0.STL
    ├── thumb_link_1.STL
    ├── thumb_link_2.STL
    ├── finger_basemotor.STL    # shared by index / middle / ring
    ├── finger_link_0.STL
    ├── finger_link_1.STL
    ├── finger_link_2.STL
    ├── finger_halfmotor.STL    # 4-bar linkage drive between link_2 and tip_braket
    ├── Tip_braket.STL
    ├── finger_tip.STL
    └── sensor.STL              # tactile sensor placeholder geom
```

---

## Kinematic Tree

```
mount
└── palm                                  (fixed, +0.071 z)
    ├── thumb_basemotor                   (fixed, rpy ≈ -5°)
    │   └── thumb_link_0                  joint_0  (Y)   0 .. 90°
    │       └── thumb_link_1              joint_1  (Z) -90 .. 90°
    │           └── thumb_link_2          joint_2  (Y)   0 .. 90°
    │               ├── thumb_sensor_0
    │               ├── thumb_sensor_1
    │               └── thumb_tip_braket  joint_3  (Y)   0 .. 90°
    │                   └── thumb_tip     (fixed)
    │
    ├── index_basemotor                   (fixed, rpy ≈ -5°)
    │   └── index_link_0                  joint_0  (X)  ± 15°
    │       └── index_link_1              joint_1  (Y)   0 .. 90°
    │           ├── index_sensor_0
    │           ├── index_sensor_1
    │           └── index_link_2          joint_2  (Y)   0 .. 90°
    │               └── index_finger_motor (fixed, +0.0267 z)
    │                   ├── index_sensor_2
    │                   └── index_tip_braket   joint_3 (Y)  0 .. 90°  ←── coupled to joint_2
    │                       └── index_tip      (fixed)
    │
    ├── middle_basemotor   (same as index, base xyz = -0.02325 0 0)
    └── ring_basemotor     (same as index, rpy ≈ +5°, base xyz = -0.02325 -0.02935 -0.0004)
```

---

## Joint / Actuator Summary

|  Finger | Hinge joints | Master actuators            | Coupled (slave) |
| ------- | ------------ | --------------------------- | --------------- |
| Thumb   | 4            | `thumb_act_{0,1,2,3}`       | —               |
| Index   | 4            | `index_act_{0,1,2}`         | `index_joint_3` |
| Middle  | 4            | `middle_act_{0,1,2}`        | `middle_joint_3` |
| Ring    | 4            | `ring_act_{0,1,2}`          | `ring_joint_3`  |
| **Total** | **16**     | **13**                      | **3**           |

The MJCF expresses the mechanical 4-bar linkage between `*_link_2` and `*_tip_braket`
(physically realized through the `finger_halfmotor` link in CAD) as a 1:1 MuJoCo
`<equality joint>` constraint.

---

## How to run in MuJoCo

```bash
pip install mujoco
python -m mujoco.viewer --mjcf=robots/hands/kistar_hand/kistar_hand.xml
```

Drive joints by moving the **Control** sliders in the left panel.

---

## How to run in any URDF parser

```python
import yourdfpy
robot = yourdfpy.URDF.load("robots/hands/kistar_hand/kistar_hand.urdf")
robot.show()
```

Compatible with: yourdfpy, RViz, Gazebo, IsaacGym, IsaacSim, SAPIEN, PyBullet.

---

## License

BSD-3-Clause © 2026 Korea Institute of Science and Technology.
Author: **Jaesung Lee** &lt;jay.lee@kist.re.kr&gt;.
