# FiveAges W2 Components

This package (`fiveages_w2_common_description`) contains reusable component descriptions for FiveAges W2:

- **Linkhou S2** chassis (`xacro/components/linkhou_s2.xacro`, `linkhou_s2_v2.xacro`)
- **Body** torso (`xacro/components/body.xacro`)
- **Head** neck and cameras (`xacro/components/head.xacro`)

The full robot and arms component xacro remain in `fiveages_w2_description` (M6 arms) and `fiveages_w2r_description` (AR5 arms).

## 1. Build

```bash
cd ~/ros2_ws
colcon build --packages-up-to fiveages_w2_common_description --symlink-install
```

## 2. Visualize Components

`component.launch.py` loads `fiveages_w2_common_description/xacro/component.xacro`.

| Launch 参数 | 作用 |
|-------------|------|
| `type` | 组件：`linkhou_s2`（底盘）、`body`、`head` |
| `chassis` | 底盘型号（仅 `type:=linkhou_s2`）：`linkhou_s2` / `linkhou_s2_v2` |
| `variant` | OEM 外观子款，如 `sinopec`（与整机 `platform.variant` 一致） |
| `chassis_joints_movable` | 底盘关节是否可动（默认 `true`；静态预览可设 `false`） |

### 2.1 Chassis（Linkhou S2）

* S2 v2（默认）
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch component.launch.py robot:=fiveages_w2_common
  ```
  默认 `type:=linkhou_s2`、`chassis:=linkhou_s2_v2`。

  ![linkhou s2 v2](.images/linkhou_s2_v2.png)

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

### 2.4 其它

固定底盘关节（仅底盘组件有意义）：

```bash
ros2 launch robot_common_launch component.launch.py robot:=fiveages_w2_common \
  chassis_joints_movable:=false
```

整机（含双臂）可视化与 `platform.variant` 见 `fiveages_w2_description` README，使用 `humanoid.launch.py robot:=fiveages_w2`。
