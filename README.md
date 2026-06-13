# WRO Future Engineers — Robot Control System

This repository contains the source code, computer vision pipelines, and mechanical design documentation for our autonomous robot competing in the **World Robot Olympiad (WRO) Future Engineers** category.

---
## 👥 Our Team

We are a team of student innovators participating in the WRO Future Engineers category. Here is how we divided our responsibilities to build and program the robot:

| **[Team Member]** | **[Primary Roles & Responsibilities]** |

| **[Shabarova Amira]** | • Chief Mechanical Engineer<br>• Chassis Architecture & Independent Suspension Design<br>• Parallel Steering Mechanism Prototyping<br>• Hardware Optimization & 3D Logistics |
| **[Nuralinova Aizere]** | • Lead Software Engineer<br>• Computer Vision Pipeline (OpenMV H7 Plus R3)<br>• Navigation Algorithms ($\arcsin$ Obstacle Tracking & Odometry)<br>• Gyro-assisted Control Loops |
![Our Team](docs/team.JPG)

## 🛠 Engineering & Mechanics

### Evolution from V1 to V2 (Suspension System)
Our previous robot was overly bulky and structurally unstable, which severely affected performance during the first simulation runs. The major issue was odometry drift. 

* **The Problem:** When a rigid 4-wheeled robot hits a surface bump or an uneven mat joint, the entire chassis tilts. This temporarily lifts the other wheels off the ground, causing a severe drop in motor traction and inducing structural vibrations that introduce noise into the Gyro sensor readings.
* **Our Solution:** We engineered an independent suspension system that allows a single wheel to compress and isolate the impact when driving over a bump.
* **Key Benefit:** Instead of forcing the entire chassis to tilt or climb the obstacle, only the affected wheel lifts to clear the unevenness. The remaining three wheels maintain 100% continuous contact with the surface. This ensures perfect odometry accuracy, rock-solid tire grip, and flawless line-tracking stability throughout the race.

![Independent Suspension System](docs/suspension.JPG)

### Steering Geometry: Parallel Steering
During the prototyping phase, we conducted a study on steering geometries. While Ackermann Steering is ideal for real-world cars to reduce tire scrub, we intentionally chose **Parallel Steering** for this robot due to the following reasons:

1. **Precision in Micro-movements:** At the small scale of LEGO parts, the mechanical backlash ("play") in Ackermann linkages often absorbs the steering input. Parallel steering provides a more direct, rigid, and predictable connection to the motor.
2. **Maximum Turning Angle:** Our parallel mechanism allows for an impressive **70-degree wheel rotation** without the linkages locking up. This is crucial for the *Parallel Parking* maneuver where maneuverability space is extremely limited.
3. **Friction Compensation:** Since we use thin front tires, the slight "sliding" effect of parallel steering actually helps the robot pivot faster in sharp corners without the bouncing effect often seen in complex LEGO linkages.

![Parallel Steering Mechanism](docs/ackermann.JPG)

---

## 📦 Hardware & Electronics

Our robot utilizes a hybrid electronic ecosystem to maximize power efficiency and computational capabilities:

* **Main Controller:** LEGO Spike Prime Hub.
* **Computer Vision:** **OpenMV Cam H7 Plus R3**. This high-performance camera handles real-time color thresholding and spatial calculations.
* **Propulsion (Rear Wheels):** **LEGO EV3 Servo Motor**. We intentionally chose the EV3 motor over the Spike Prime motor because it delivers significantly higher torque and power, ensuring stable acceleration.
* **Steering (Front Wheels):** **Small LEGO Spike Prime Motor**. Its compact form factor allows us to keep the front steering assembly lightweight and minimal.

![Robot Components Layout](docs/robot_view.jpg)

---

## 🏎 Game Strategy & Navigation Algorithms

### 1. Qualification Round (Open-in / Obstacle-free)
In the first round, the robot focuses on high-speed stability and boundary keeping:
* **Initial Detection:** The robot drives forward using integrated side gyroscopes for stabilization. It approaches the boundary lines and reads the line color using color sensors.
* **Direction Lock:** Based on the detected color, the robot determines the race direction (clockwise or counter-clockwise).
* **Gyro Navigation:** For the rest of the round, the robot maintains its trajectory using strict gyro-sensor feedback loops to minimize drift on straight lines.

### 2. Obstacle Avoidance Round (Obstacle Course)
When obstacles (pillars) are introduced, the robot switches to an advanced localization and mathematical tracking mode:

* **Odometry Tracking:** The robot constantly monitors its relative position using motor encoders (odometry tracking).
* **Distance Estimation via Vision:** When the **OpenMV H7 Plus R3** camera detects a pillar, the algorithm calculates the distance to the object based on the **bounding box area (contour area)** of the pillar. A larger area indicates closer proximity.
* **Arc-Sine Trigonometry Steering:** Using the estimated distance and the camera's horizontal offset angle, the robot applies **arcsin ($\arcsin$) formulas** to calculate the exact coordinate of the pillar relative to the robot's current odometry baseline. The steering angle is adjusted dynamically to smoothly avoid the pillar while keeping the optimal racing line.

![OpenMV Vision Processing and Math](docs/vision_processing.jpg)

---

## 📂 Repository Layout

* `src/main.py`: Main state machine managing the logic for Qualification and Obstacle rounds.
* `src/opening.round`: Low-level motor control routines including gyro-straightening and encoder-based movement.
* `src/camera.code`: OpenMV script for color blob tracking and distance estimation via contour area.
* `src/drive.odometry`: Mathematical implementation of the $\arcsin$ obstacle localization algorithms.
* `src/obstacle.round`: Code for obctacle round. Reading odometry and driving above obctacles.
