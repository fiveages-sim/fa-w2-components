# FiveAges W2 Components

This package (`fiveages_w2_common_description`) contains reusable component descriptions for FiveAges W2:

- **Linkhou S2** chassis (`xacro/components/linkhou_s2.xacro`, `linkhou_s2_v2.xacro`)
- **Stand** 立柱 (`xacro/components/stand.xacro`)
- **Body** 移动腰段 (`xacro/components/body.xacro`)
- **Torso** 胸部平台 (`xacro/components/torso.xacro`)
- **Head** neck and cameras (`xacro/components/head.xacro`)

轮式整机见 `fiveages_w2_description`（M6）/ `fiveages_w2r_description`（AR5）；立柱整机见 `fiveages_s2_description`（M6）。

## 1. Build

```bash
cd ~/ros2_ws
colcon build --packages-up-to fiveages_w2_common_description --symlink-install
```

## 2. Visualize Components

`component.launch.py` loads `fiveages_w2_common_description/xacro/component.xacro`.

| Launch 参数 | 作用 |
|-------------|------|
| `type` | 组件：`linkhou_s2`（底盘）、`body`、`head`、`stand`、`torso` |
| `chassis` | 底盘型号（仅 `type:=linkhou_s2`）：`linkhou_s2` / `linkhou_s2_v2` |
| `variant` | OEM 外观子款：`sinopec`（v1 底盘 / head / body / torso）、`white`（v2 底盘 / head / body / torso）；`stand`/`v1.5` 仅扩展 **head** 关节限位（立柱整机用 `robot:=fiveages_s2`）；`v0` 锁定立柱 `lift_joint`（无升降） |
| `collider` | 碰撞体：`simple`（盒体/圆柱 primitive）/ `convex`（mesh）；底盘轮子始终为圆柱 |
| `chassis_joints_movable` | 底盘关节是否可动（默认 `true`；静态预览可设 `false`） |

### 2.1 Chassis（Linkhou S2）

* S2 v2（默认）
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch component.launch.py robot:=fiveages_w2_common
  ```
  默认 `type:=linkhou_s2`、`chassis:=linkhou_s2_v2`。

  ![linkhou s2 v2](.images/linkhou_s2_v2.png)

* S2 v2 + 白色
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch component.launch.py robot:=fiveages_w2_common \
    variant:=white
  ```

* S2 v1
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch component.launch.py robot:=fiveages_w2_common \
    type:=linkhou_s2 chassis:=linkhou_s2
  ```
  ![linkhou s2 v1](.images/linkhou_s2.png)

* S2 v1 + 中石化外观
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch component.launch.py robot:=fiveages_w2_common \
    type:=linkhou_s2 chassis:=linkhou_s2 variant:=sinopec
  ```
  ![linkhou s2 v1_sinopec](.images/linkhou_s2_sinopec.png)

### 2.2 Body

* 默认
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch component.launch.py robot:=fiveages_w2_common type:=body
  ```
  ![w2_body](.images/w2_body.png)

* 中石化
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch component.launch.py robot:=fiveages_w2_common \
    type:=body variant:=sinopec
  ```
  ![w2_body_sinopec](.images/w2_body_sinopec.png)

* 白色
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch component.launch.py robot:=fiveages_w2_common \
    type:=body variant:=white
  ```

### 2.3 Head

* 默认
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch component.launch.py robot:=fiveages_w2_common type:=head
  ```
  ![w2_head](.images/w2_head.png)

* 中石化
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch component.launch.py robot:=fiveages_w2_common \
    type:=head variant:=sinopec
  ```
  ![w2_head_sinopec](.images/w2_head_sinopec.png)

* 白色
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch component.launch.py robot:=fiveages_w2_common \
    type:=head variant:=white
  ```

* Stand / v1.5（扩展关节限位，来自 `LRAA0000000_0528_08.SLDASM.urdf`）
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch component.launch.py robot:=fiveages_w2_common \
    type:=head variant:=stand
  ```
  `variant:=v1.5` 与 `stand` 等效。偏航 ±1.3963 rad，俯仰 -0.4014 ~ 0.5236 rad。

### 2.4 Stand / Torso（立柱）

* 立柱（3 关节）
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch component.launch.py robot:=fiveages_w2_common type:=stand
  ```
  可升降时 `lift_joint` 为棱柱 `[0, 0.29]` m，轴 `+Z`（增大往上）；`variant:=v0` 锁定为 fixed。

* 胸部平台（torso）
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch component.launch.py robot:=fiveages_w2_common type:=torso
  ```

* 白色
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch component.launch.py robot:=fiveages_w2_common \
    type:=torso variant:=white
  ```

立柱整机见 `fiveages_s2_description` README：`humanoid.launch.py robot:=fiveages_s2`。

### 2.5 其它

固定底盘关节（仅底盘组件有意义）：

```bash
ros2 launch robot_common_launch component.launch.py robot:=fiveages_w2_common \
  chassis_joints_movable:=false
```

整机（含双臂）可视化与 `platform.variant` 见 `fiveages_w2_description` README，使用 `humanoid.launch.py robot:=fiveages_w2`。

## 3. 导航地图与 Nav2 配置

本包提供 W2 导航静态资源（与底盘/机身组件同属组件层）：

| 目录 | 内容 |
|------|------|
| `config/maps` | 场景地图（`siemens`、`siemens_task2` 等；子目录内优先 `map.yaml`） |
| `config/nav2` | `nav2_params_isaac_gt.yaml`、`nav2_params_isaac_gt_map_only.yaml`、行为树 xml |

### 3.1 Launch 示例

`robot:=fiveages_w2` 时由 `fiveages_w2_description/config/navigation.yaml` 指向本包（无需每次手写包名）：

```bash
source ~/ros2_ws/install/setup.bash
ros2 launch robot_common_launch navigation_isaac_gt.launch.py \
  robot:=fiveages_w2 map:=siemens_task2
```

仅静态地图（不处理点云）：

```bash
ros2 launch robot_common_launch navigation_isaac_gt.launch.py \
  robot:=fiveages_w2 map:=siemens nav2_profile:=map_only
```

通用仿真地图（warehouse 等）在 `robot_common_launch/config/maps/`，会与 W2 地图一并出现在 `./quick_start.sh` 导航菜单中。

`map` 参数可写文件夹名、绝对路径，或子路径如 `german_poc/map`（自动补 `.yaml`）。
