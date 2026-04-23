# System Architecture

## Overview

Grasshopper → COMPAS → ROS2 → RO1

## Layer Breakdown

### 1. Geometry  
(Grasshopper / Rhino)

- Curves / toolpaths
- Frames / targets
- Custom parameters (speed, accel, tool id)

### 2. Kinematics (COMPAS + Python)

- Converts geometry → robot motion
- Process:
  Frame → IK → Joint values

### 3. Communication

- ROS2 (real-time streaming)
- REST API (web-based control)
- Modbus (low-level registers)

### 4. RO1

- JointTrajectory (ROS2)
- HTTP commands (REST)
- Register values (Modbus)

---

## Simulation

- HKS MADE StandardBots online web simulation
  https://cb2347.sb.app/
- Example manufacturing applications such as Collaborative, Curve Following, Polishing, Sanding using Standard Bots RO1.
  https://robodk.com/3D/example/Polishing-and-Sanding-with-Standard-Bots-RO1 