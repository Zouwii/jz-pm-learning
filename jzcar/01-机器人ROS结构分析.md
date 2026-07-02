# 机器人 ROS 结构分析

> **分析时间：** 2026-06-17
> **目标机器人：** 172.19.10.11
> **主机名：** jzrobot-x（x86 工控机）
> **操作系统：** Ubuntu 16.04.1 LTS
> **系统镜像：** ubuntu16041-jz-emma-1-9.img.gz

---

## 一、节点架构（共 50 个 ROS 节点）

### 1.1 导航系统（Navigation）

| 节点 | 功能描述 |
|------|---------|
| `cartographer_node` | Cartographer SLAM 定位与建图 |
| `cartographer_tf_publisher` | Cartographer TF 坐标发布 |
| `nav_manager_node` | 导航管理（全局规划、路径控制） |
| `nav_checker` | 导航安全检查（碰撞检测、路径校验） |
| `nav_net` | 导航网络通信 |
| `scan_localiser` | 基于激光的定位 |
| `tag_localiser` | 基于视觉标签（Apriltag）的定位 |
| `lasermark_node` | 激光标记定位 |
| `fm_odometry` | 轮式里程计 |
| `speed_manager` | 速度仲裁管理（多来源速度融合） |
| `costmap_pub_node` | 代价地图发布 |
| `map_status_pub_node` | 地图状态发布 |

### 1.2 任务调度（Task & Scheduling）

| 节点 | 功能描述 |
|------|---------|
| `agent3` | 任务调度引擎（agent3 架构，核心调度节点） |
| `async_service` | 异步服务处理 |
| `web_service` | Web API 服务（对外接口） |
| `jmther` | 系统母体进程（核心状态管理） |
| `jjobs_daemon` | Job 守护进程 |
| `jjobs_caliber_checker` | Job 标定状态检查 |
| `jlinks_daemon` | 事件链路守护进程 |
| `localagent` | 本地调度代理 |

### 1.3 感知系统（Perception）

| 节点 | 功能描述 |
|------|---------|
| `camera_node_up` | 上相机节点（深度相机） |
| `camera_node_down` | 下相机节点（深度相机） |
| `front/ast_camera` | 前向 AST 相机 |
| `front/image_transport` | 前向图像传输 |
| `iplus_perception_node` | iplus 感知节点（综合感知） |
| `jzhw_detector` | 硬件检测器 |
| `jzhw_node` | 硬件管理节点 |
| `jzhw_tf` | 硬件相关 TF 发布 |

### 1.4 伺服/执行（Servo & Actuation）

| 节点 | 功能描述 |
|------|---------|
| `ibvs_constrained` | 视觉伺服约束（IBVS = Image-Based Visual Servoing） |
| `carly` | 卡尔控制模块（纹理伺服、货架检测、视觉对接） |
| `caliber_server` | 标定服务 |
| `servo_net` | 伺服网络通信 |

### 1.5 底盘硬件（Hardware & Chassis）

| 节点 | 功能描述 |
|------|---------|
| `ioboard` | IO 板管理（GPIO、货叉、举升等） |
| `joystick` | 遥杆输入 |
| `joystick_controller` | 遥杆控制 |

### 1.6 传感器驱动（Sensor Drivers）

| 节点 | 功能描述 |
|------|---------|
| `sick_tim_240_0` | SICK TiM240 导航激光雷达 |
| `laser2d_sick_s300_front` | SICK S300 前向安全避障激光 |
| `jrosth_2243856042` | ROS 话题硬件抽象层 |

### 1.7 系统管理（System）

| 节点 | 功能描述 |
|------|---------|
| `jz_system` / `jz_env` | 迦智系统/环境管理 |
| `jzui` | 迦智用户界面 |
| `dcui` | 调度UI |
| `osmanager` | 操作系统管理 |
| `rosbag` | 数据录包 |
| `scan_filter` | 激光扫描过滤 |
| `precision_check` | 定位精度检查 |
| `soft_lowbattery_off` | 软低电量关机 |
| `emerg_clear_all` | 急停清除 |

---

## 二、话题（Topics）分类统计

### 2.1 导航与定位 (~50 个)

核心话题包括：
- 速度输出：`move_base_vel`、`move_base_vel_oa`、`move_base_vel_cd`
- 安全检查：`nav_checker/*`（碰撞距离、代价地图、预测路径、停止类型等）
- 定位相关：`tag_localiser/*`、`laser_localiser/*`、`scan_emma_nav_*`
- 子图管理：`submap_*`、`submap_cloud`、`submap_local`
- 路由状态：`route/*`
- 地图管理：`map_master/*`
- 路标信息：`landmark_*`

### 2.2 硬件与底盘 (~30 个)

- 硬件状态：`jzhw/*`（电池、电机、刹车、碰撞、急停、GPS、IMU 等）
- IO板：`iobox/*`（GPIO、货叉举升高度、载货重量、灯光控制、复位按钮等）
- 载具控制：`emb/carrier/*`、`carrier/*`
- 速度控制：`sdpx/*`

### 2.3 任务与调度 (~20 个)

- 任务状态：`agent/*`（错误、事件、健康、状态）
- 链路状态：`route/*`
- 任务距离：`task_beginning_dist`、`task_ending_dist`
- 卡尔模块：`carly/*`（货物偏差、装载、视觉伺服置信度、速度限制等）
- Web服务：`web_service/*`、`async/*`

### 2.4 感知与视觉 (~15 个)

- 深度相机：`front/depth/*`（深度图、点云、压缩图像）
- 彩色相机：`front/color/*`
- 对接相机：`cam_*_docking`
- 杆状物：`pole_like_*`（聚类、网格、标记、ROI点云等）

### 2.5 状态监控 (~15 个)

- 系统状态：`robot_state`、`work_state`、`diagnostics`
- 迦智状态：`jstate_*`（错误收集、模块收集、机器人收集、系统收集）
- 人员状态：`people_states`
- 行为树：`btree_monitor/status`

### 2.6 视觉伺服 (~10 个)

- IBVS Action：`ibvs_constrained_action/*`（goal/feedback/result/status）
- 视觉伺服速度：`visual_servo_*`
- VTR：`vtr/*`（全局定位、子图定位、移动指令）

---

## 三、服务（Services）分类统计

### 3.1 视觉伺服示教 (~15个)

`ibvs_constrained/*_teaching_server` 支持多种示教方式：
二维码、纹理、托盘、激光线、反射板、地面标签、3D模型、相对示教、PSD、双相机等

### 3.2 相机参数控制 (~20个)

`front/set_*` / `front/get_*`：曝光、增益、白平衡、深度镜像、激光、风扇

### 3.3 硬件控制 (~15个)

`jzhw/camera/*`（上下相机标定/曝光/增益/LED）、`iobox/*`（GPIO读写、低压模式、旋转标定）、`jzhw/eeprom_*`（读写）、`jzhw/pwrcmd`（电源控制）

### 3.4 导航控制 (~10个)

`move_task/*`（移动任务/原始路由/伺服路径）、`nav/*`（慢速停止）、`map/localization/*`（地图定位）、`load_map`、`start_trajectory`、`finish_trajectory`

### 3.5 Cartographer (~8个)

`cartographer_ros/*`：添加约束、保存离线地图、运行优化触发、删除子图/轨迹、更新世界坐标点

---

## 四、架构特征分析

### 4.1 导航方案

```
Cartographer SLAM（核心建图定位引擎）
    ├── 激光定位（scan_localiser + 导航激光 TiM240）
    ├── 视觉标签定位（tag_localiser → Apriltag / 二维码）
    ├── 激光标记定位（lasermark_node → 反光板）
    └── 轮式里程计（fm_odometry）
         ↓
    nav_manager_node（全局规划 + 路径控制）
         ↓
    speed_manager（多来源速度仲裁融合）
         ↓
    mux_vel → platform_control/cmd_vel（底盘速度指令）
```

- SLAM：Google Cartographer
- 支持**多传感器融合定位**：激光 + 视觉标签 + 激光反光标记
- 有独立的代价地图（costmap）和导航安全检查层

### 4.2 任务架构

```
agent3（核心调度引擎）
    ├── jjobs_daemon（Job 管理）
    │     └── jjobs_caliber_checker（标定检查）
    ├── jlinks_daemon（事件链路）
    ├── async_service（异步任务）
    ├── web_service（对外 API）
    └── localagent（本地代理）
```

- **agent3** 是第三代任务调度架构
- jjobs 管理任务 Job 的生命周期
- jlinks 维护事件链路，协调模块间交互
- jmother 作为"系统母体"统筹各模块

### 4.3 伺服方案

```
ibvs_constrained（主视觉伺服）
    ├── 二维码示教 → 托盘/货架精准对位
    ├── 纹理示教 → 纹理特征匹配
    ├── 托盘示教 → 托盘检测与插取
    ├── 激光线示教 → 激光线特征
    └── 3D模型示教 → 3D物件识别对位

carly（卡尔控制模块）
    ├── 纹理伺服（textureServo）→ 基于纹理的精确伺服
    ├── 货架检测（shelf）→ 货架识别
    └── 视觉对接（docking）→ 视觉引导对接

servo_net（伺服网络）
    ├── 叉车托盘堆叠
    └── 多楼层托盘检测
```

### 4.4 传感器配置

| 传感器 | 型号 | 用途 |
|--------|------|------|
| 导航激光 | SICK TiM240 | 建图、定位、障碍物检测 |
| 安全激光 | SICK S300 | 前向安全避障、安全区域 |
| 上深度相机 | （D435系列） | 视觉伺服、标签识别、货架对接 |
| 下深度相机 | （D435系列） | 托盘检测、地面参考 |
| 前向AST相机 | — | 前向视觉感知 |
| IMU | 内置 | 姿态估计 |
| 轮式编码器 | — | 里程计 |

### 4.5 车型推断

基于以下特征，推断为**叉车（Forklift）**类车型：

- `emb/carrier/*` — 嵌入式载具控制
- `iobox/ifork/*` — 货叉举升高度/载货重量
- `servo_net/forklift/*` — 叉车专用伺服（托盘堆叠）
- `carrier/*`（carrier_box, carrier_rotater）— 载具箱体/旋转器
- 上下双目深度相机配置，用于精确对位和托盘检测

---

## 五、文件记录

后续探索内容将记录在本目录下，按编号递增：

```
jzcar/
├── 01-机器人ROS结构分析.md   ← 本文
├── 02-后续探索.md
└── ...
```
