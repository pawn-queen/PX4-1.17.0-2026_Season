# PX4-1.17.0-2026_Season

## 仓库说明

本仓库是 2026 赛季仿真运行仓库，基于 **PX4 1.17.0** 版本构建。

本仓库采用最新的 PX4 1.17.0 版本作为仿真运行主程序，针对赛季仿真的主要修改集中在 Gazebo 仿真子仓库：

```text
Tools/simulation/gz
```

感谢上一赛季仓库对本项目的支持与参考：

```text
https://github.com/kongpincheng1/PX4-1.15.4-2025_Season.git
```

---

## 如何获取完整内容

由于本仓库包含 Git 子模块，克隆时请使用 `--recursive` 参数：

```bash
git clone --recursive git@github.com:pawn-queen/PX4-1.17.0-2026_Season.git
cd PX4-1.17.0-2026_Season
git checkout work/px4-v1.17.0
git submodule update --init --recursive
```

其余编译、仿真和运行步骤可参考 PX4 官方文档，以及上一赛季仓库：

```text
https://github.com/kongpincheng1/PX4-1.15.4-2025_Season.git
```

---

# Gazebo 仿真子模块修改说明

## 1. 背景说明

`Tools/simulation/gz` 是一个独立的 Git 子模块，对应仓库为：

```text
PX4/PX4-gazebo-models
```

因此，位于 `Tools/simulation/gz` 目录下的 Gazebo 仿真修改，并不直接属于父仓库 `PX4-1.17.0-2026_Season` 的普通文件修改。

如果希望另一台电脑或另一个克隆仓库能够通过正常的父仓库子模块更新获得这些修改，需要先在该子模块内部完成提交和推送：

```bash
cd Tools/simulation/gz
git add .
git commit -m "Add 2026 season Gazebo models"
git push
```

然后再回到父仓库提交子模块指针变化：

```bash
cd ../../..
git add Tools/simulation/gz
git commit -m "Update Gazebo simulation submodule pointer"
git push
```

否则，其他克隆仓库只能看到父仓库中记录的旧子模块版本，无法自动获得当前本地的 Gazebo 模型和世界文件修改。

---

## 2. 修改文件说明

当前 `Tools/simulation/gz` 子模块中存在以下本地修改文件：

| 文件路径                         | 修改说明                                                                                        |
| ---------------------------- | ------------------------------------------------------------------------------------------- |
| `models/OakD-Lite/model.sdf` | 调整相机传感器姿态，将相机 pitch 方向旋转 `1.570796`；将 RGB 图像尺寸由 `1920x1080` 修改为 `640x480`；同时调整双目/深度相机的位姿参数。 |
| `models/x500_base/model.sdf` | 为无人机基础机体 link 以及四个旋翼 link 启用风场影响，使仿真中机体和桨叶能够受到风力作用。                                         |
| `worlds/default.sdf`         | 新增赛季仿真场景配置，包括降落区、GUI/system 插件、小尺寸地面平面、中国区域球面坐标，以及蓝色/黄色目标区域、边界可视化和圆柱障碍物。                    |

---

## 3. 新增模型资源

当前子模块中新增了以下 Gazebo 模型资源：

| 路径                       | 内容说明                                                            |
| ------------------------ | --------------------------------------------------------------- |
| `models/cylinder_large`  | 大型圆柱障碍物模型，包含 SDF、config 配置文件以及 `meshes/big.stl` 网格文件。           |
| `models/cylinder_middle` | 中型圆柱障碍物模型，包含 SDF、config 配置文件以及 `meshes/middle.stl` 网格文件。        |
| `models/cylinder_small`  | 小型圆柱障碍物模型，包含 SDF、config 配置文件以及 `meshes/small.stl` 网格文件。         |
| `models/land_zone`       | 降落区模型，包含 SDF、config 配置文件，以及 `landzone.jpg`、`landzone.png` 贴图资源。 |

---

## 4. 重要说明

1. 父仓库分支在记录这些说明之前，已经与远程分支 `origin/work/px4-v1.17.0` 同步。

2. 当前新增的外部依赖以 Git 子模块指针的方式管理，而不是直接复制完整源码树到父仓库中。

3. Gazebo 模型和世界文件的修改目前位于：

   ```text
   Tools/simulation/gz
   ```

   也就是 PX4 的 Gazebo 仿真子模块工作区中。

4. 这些修改必须在子模块仓库内部单独提交并推送，否则父仓库无法完整记录这些文件内容。

5. 父仓库中最终记录的是子模块的 commit 指针。只有当子模块本身已经提交并推送后，父仓库再提交新的子模块指针，其他开发者才能通过以下命令正确获取对应的仿真资源：

   ```bash
   git submodule update --init --recursive
   ```

---

## 5. 推荐提交流程

### 第一步：进入 Gazebo 子模块

```bash
cd Tools/simulation/gz
```

### 第二步：检查子模块修改状态

```bash
git status
```

确认修改文件和新增模型资源是否正确。

### 第三步：提交子模块修改

```bash
git add .
git commit -m "Add 2026 season Gazebo models"
```

### 第四步：推送子模块分支

```bash
git push
```

如果当前子模块分支还没有设置远程追踪分支，可以使用：

```bash
git push -u origin 当前分支名
```

### 第五步：回到父 PX4 仓库

```bash
cd ../../..
```

### 第六步：提交子模块指针变化

```bash
git status
git add Tools/simulation/gz
git commit -m "Update Gazebo simulation submodule pointer"
git push
```

---

## 6. 克隆后获取方式

其他开发者在克隆父仓库后，应执行：

```bash
git submodule update --init --recursive
```

如果已经克隆过仓库，但需要更新子模块到父仓库记录的新版本，应执行：

```bash
git pull
git submodule update --init --recursive
```

这样才能确保 `Tools/simulation/gz` 中的 Gazebo 模型、世界文件和新增资源与当前父仓库记录的子模块版本保持一致。

---
# PX4 Drone Autopilot

[![Releases](https://img.shields.io/github/release/PX4/PX4-Autopilot.svg)](https://github.com/PX4/PX4-Autopilot/releases) [![DOI](https://zenodo.org/badge/22634/PX4/PX4-Autopilot.svg)](https://zenodo.org/badge/latestdoi/22634/PX4/PX4-Autopilot)

[![Build Targets](https://github.com/PX4/PX4-Autopilot/actions/workflows/build_all_targets.yml/badge.svg?branch=main)](https://github.com/PX4/PX4-Autopilot/actions/workflows/build_all_targets.yml) [![SITL Tests](https://github.com/PX4/PX4-Autopilot/workflows/SITL%20Tests/badge.svg?branch=master)](https://github.com/PX4/PX4-Autopilot/actions?query=workflow%3A%22SITL+Tests%22)

[![Discord Shield](https://discordapp.com/api/guilds/1022170275984457759/widget.png?style=shield)](https://discord.gg/dronecode)

This repository holds the [PX4](http://px4.io) flight control solution for drones, with the main applications located in the [src/modules](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules) directory. It also contains the PX4 Drone Middleware Platform, which provides drivers and middleware to run drones.

PX4 is highly portable, OS-independent and supports Linux, NuttX and MacOS out of the box.

* Official Website: http://px4.io (License: BSD 3-clause, [LICENSE](https://github.com/PX4/PX4-Autopilot/blob/main/LICENSE))
* [Supported airframes](https://docs.px4.io/main/en/airframes/airframe_reference.html) ([portfolio](https://px4.io/ecosystem/commercial-systems/)):
  * [Multicopters](https://docs.px4.io/main/en/frames_multicopter/)
  * [Fixed wing](https://docs.px4.io/main/en/frames_plane/)
  * [VTOL](https://docs.px4.io/main/en/frames_vtol/)
  * [Autogyro](https://docs.px4.io/main/en/frames_autogyro/)
  * [Rover](https://docs.px4.io/main/en/frames_rover/)
  * many more experimental types (Blimps, Boats, Submarines, High Altitude Balloons, Spacecraft, etc)
* Releases: [Downloads](https://github.com/PX4/PX4-Autopilot/releases)

## Releases

Release notes and supporting information for PX4 releases can be found on the [Developer Guide](https://docs.px4.io/main/en/releases/).

## Building a PX4 based drone, rover, boat or robot

The [PX4 User Guide](https://docs.px4.io/main/en/) explains how to assemble [supported vehicles](https://docs.px4.io/main/en/airframes/airframe_reference.html) and fly drones with PX4. See the [forum and chat](https://docs.px4.io/main/en/#getting-help) if you need help!


## Changing Code and Contributing

This [Developer Guide](https://docs.px4.io/main/en/development/development.html) is for software developers who want to modify the flight stack and middleware (e.g. to add new flight modes), hardware integrators who want to support new flight controller boards and peripherals, and anyone who wants to get PX4 working on a new (unsupported) airframe/vehicle.

Developers should read the [Guide for Contributions](https://docs.px4.io/main/en/contribute/).
See the [forum and chat](https://docs.px4.io/main/en/#getting-help) if you need help!


## Weekly Dev Call

The PX4 Dev Team syncs up on a [weekly dev call](https://docs.px4.io/main/en/contribute/).

> **Note** The dev call is open to all interested developers (not just the core dev team). This is a great opportunity to meet the team and contribute to the ongoing development of the platform. It includes a QA session for newcomers. All regular calls are listed in the [Dronecode calendar](https://www.dronecode.org/calendar/).


## Maintenance Team

See the latest list of maintainers on [MAINTAINERS](MAINTAINERS.md) file at the root of the project.

For the latest stats on contributors please see the latest stats for the Dronecode ecosystem in our project dashboard under [LFX Insights](https://insights.lfx.linuxfoundation.org/foundation/dronecode). For information on how to update your profile and affiliations please see the following support link on how to [Complete Your LFX Profile](https://docs.linuxfoundation.org/lfx/my-profile/complete-your-lfx-profile). Dronecode publishes a yearly snapshot of contributions and achievements on its [website under the Reports section](https://dronecode.org).

## Supported Hardware

For the most up to date information, please visit [PX4 User Guide > Autopilot Hardware](https://docs.px4.io/main/en/flight_controller/).

## Project Governance

The PX4 Autopilot project including all of its trademarks is hosted under [Dronecode](https://www.dronecode.org/), part of the Linux Foundation.

<a href="https://www.dronecode.org/" style="padding:20px" ><img src="https://dronecode.org/wp-content/uploads/sites/24/2020/08/dronecode_logo_default-1.png" alt="Dronecode Logo" width="110px"/></a>
<div style="padding:10px">&nbsp;</div>
