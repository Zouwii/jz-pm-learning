# jz-pnc 仿真运行指南

## 1. 前置准备

### 安装依赖
```bash
cd /home/zhr/zhr_ws/src/jz-pnc
./tools/configure.sh
```

### 编译（仅编译 jz-pnc 相关包）
```bash
cd /home/zhr/zhr_ws
rm -rf build/ devel/
catkin_make -DCATKIN_WHITELIST_PACKAGES="jz_pnc_core;jz_pnc_kinematics;jz_pnc_path_planner;jz_pnc_trajectory_planner;jz_pnc_predictor;jz_pnc_motion_controller;jz_pnc_revolve_controller;jz_pnc_rotator_controller;jz_pnc_sim_test"
```

### 加载环境
```bash
source /opt/ros/kinetic/setup.bash
source /home/zhr/zhr_ws/devel/setup.bash
```

---

## 2. 6 种底盘仿真

| 底盘类型 | Launch 命令 | 说明 |
|---------|------------|------|
| 差速底盘 | `roslaunch jz_pnc_sim_test motion_differ_sim.launch` | 左右轮独立驱动，通过轮速差实现转向 |
| 双舵轮底盘 | `roslaunch jz_pnc_sim_test motion_bisteer_sim.launch` | 前后轮均可转向，提高机动性 |
| 双舵轮（侧向运动） | `roslaunch jz_pnc_sim_test motion_bisteer_lat_sim.launch` | 支持侧向运动的双舵轮底盘 |
| 麦克纳姆轮 | `roslaunch jz_pnc_sim_test motion_mecanum_sim.launch` | 使用麦克纳姆轮，实现全向移动 |
| 麦克纳姆轮（头锁定） | `roslaunch jz_pnc_sim_test motion_mecanum_headlock_sim.launch` | 全向移动但保持车头朝向固定 |
| 叉车底盘 | `roslaunch jz_pnc_sim_test motion_forklift_sim.launch` | 叉车类型底盘，具有特殊运动学特性 |

---

## 3. 仿真环境

- **仿真器**：Stage（2D 轻量级仿真器）
- **定位**：AMCL 自适应蒙特卡洛定位
- **地图**：迷宫地图（maze.yaml）、大空间地图（maze_large_space.yaml）
- **可视化**：rqt_plot（速度命令曲线）

---

## 4. 仿真启动后会发生什么

以差速底盘为例，launch 文件同时启动：
1. **Stage 仿真器**（stageros） - 加载 `maze_diff.world` 迷宫世界，包含机器人模型
2. **地图服务器**（map_server） - 加载 `maze.yaml` 地图
3. **AMCL 定位**（amcl） - 初始位姿在 (2, 2, 0)
4. **jz_pnc_sim_test_node** - 规划控制仿真节点，运行完整 PNC 算法闭环
5. **rqt_plot** - 显示 /cmd_vel 速度命令曲线

---

## 5. 监控与调试命令

```bash
# 监控机器人里程计
rostopic echo /odom

# 监控速度控制命令
rostopic echo /cmd_vel

# 监控 AMCL 定位位姿
rostopic echo /amcl_pose

# 查看节点列表
rosnode list

# 查看话题列表
rostopic list
```

---

## 6. 修改仿真配置

配置文件位置：`jz_pnc/motion_test/appendix/config/`

| 底盘 | 配置文件 |
|------|--------|
| 差速 | `sim_config_differ.json` |
| 双舵轮 | `sim_config_bisteer.json` |
| 双舵轮侧向 | `sim_config_bisteer_lat.json` |
| 麦克纳姆 | `sim_config_mecanum.json` |
| 麦克纳姆头锁定 | `sim_config_mecanum_headlock.json` |
| 叉车 | `sim_config_forklift.json` |

可修改内容：底盘参数（轴距、轮距、轮径）、速度/加速度约束、控制器参数等。

---

## 7. PNC 算法数据链路

```
参考路径
  → path_planner（Frenet 采样生成几何路径）
  → trajectory_planner（速度规划生成带时间的 CentroidTrajectory）
  → motion_controller（根据当前状态输出质心速度命令）
  → predictor（基于历史命令补偿执行延迟）
  → kinematics（质心命令和执行器命令互转）
```
