# PX4 1.17.0 2026 Season Change Notes

This document records the local changes found in this checkout for the `work/px4-v1.17.0` branch. It is intended as a quick handoff note for future cloning, review, and simulation setup.

## Parent Repository Changes

The parent PX4 repository adds references to several external dependencies as Git submodules:

| Path | Source | Purpose |
| --- | --- | --- |
| `boards/modalai/voxl2/src/lib/mpa/libmodal-json` | `https://gitlab.com/voxl-public/voxl-sdk/core-libs/libmodal-json.git` | ModalAI JSON/YAML helper library. |
| `boards/modalai/voxl2/src/lib/mpa/libmodal-pipe` | `https://gitlab.com/voxl-public/voxl-sdk/core-libs/libmodal-pipe.git` | ModalAI pipe IPC helper library. |
| `src/lib/rl_tools/rl_tools` | `https://github.com/rl-tools/rl-tools.git` | Reinforcement-learning utility library. |
| `src/modules/mc_raptor/blob` | `https://github.com/rl-tools/px4-blob` | RAPTOR policy/model blob files. |
| `src/modules/simulation/gz_plugins/optical_flow/PX4-OpticalFlow` | `https://github.com/PX4/PX4-OpticalFlow.git` | PX4 optical-flow plugin sources for simulation. |
| `src/modules/uxrce_dds_client/Micro-XRCE-DDS-Client-v3` | `https://github.com/PX4/Micro-XRCE-DDS-Client.git` | Micro XRCE-DDS Client v3 source pinned to `px4-release/3`. |

The root `.gitignore` also excludes common local artifacts, flight logs, ROS bag/database outputs, Python caches, and nested dependency metadata so generated files are not accidentally committed.

After cloning this branch, initialize the added dependencies with:

```sh
git submodule update --init --recursive
```

## Gazebo Simulation Submodule Changes

`Tools/simulation/gz` is a separate Git submodule (`PX4/PX4-gazebo-models`). The working tree contains local simulation changes there. These changes must be committed and pushed in that submodule, or applied separately, before another clone can receive them through a normal parent-repository submodule update.

Modified files inside `Tools/simulation/gz`:

| File | Summary |
| --- | --- |
| `models/OakD-Lite/model.sdf` | Rotates camera sensors by `1.570796` pitch, changes RGB image size from `1920x1080` to `640x480`, and adjusts the stereo/depth camera pose. |
| `models/x500_base/model.sdf` | Enables wind effects on the base link and four rotor links. |
| `worlds/default.sdf` | Adds land-zone setup, GUI/system plugins, a smaller ground plane, China-based spherical coordinates, and two target areas with blue/yellow landing or boundary visuals and cylinder obstacles. |

New model assets inside `Tools/simulation/gz`:

| Path | Contents |
| --- | --- |
| `models/cylinder_large` | Large cylinder SDF/config and `meshes/big.stl`. |
| `models/cylinder_middle` | Middle cylinder SDF/config and `meshes/middle.stl`. |
| `models/cylinder_small` | Small cylinder SDF/config and `meshes/small.stl`. |
| `models/land_zone` | Landing-zone SDF/config plus `landzone.jpg` and `landzone.png` textures. |

## Notes For Review

- The parent repository branch was synchronized with `origin/work/px4-v1.17.0` before committing these notes.
- The added external dependencies are tracked as submodule pointers, not copied source trees.
- The Gazebo model changes are intentionally documented here because they currently live in the `Tools/simulation/gz` submodule working tree rather than in the parent PX4 commit.
