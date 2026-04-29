---
title: 系统架构说明
---

# 控制系统架构

半醒提供全栈开放架构： 
    
- 基于CANFD的电机硬件控制|调试接口；     
- 基于Mujoco的仿真环境、机器人URDF/xml等；     
- 基于ROS2的硬件管理节点，控制策略无缝部署到仿真/真机硬件；     
- 基于强化学习的运控控制训练示例代码；    

![系统架构](../../assets/elf3/developer/overview/software_structure.png) 




## 文档目录

- 基于ROS2的控制策略部署框架，无缝部署到仿真/真机硬件：https://github.com/bxirobotics/bxi_rl_controller_ros2_example    
- 机器人URDF：https://github.com/bxirobotics/bxi_rl_controller_ros2_example/tree/main/resources    

