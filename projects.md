---
layout: default
title: Projects
---

# Projects

## Object-Agnostic Placing Pipeline (ELPIS Lab)
**Stack:** Python, ROS 2, OpenCV, HDBSCAN, SciPy, Intel RealSense D435, SMACH, MoveIt, UR10  
**Problem.** In block-rearrangement tasks the robot needs collision-free placement poses for a grasped object at runtime, on a cluttered table or on top of other objects, without a prior model of the object or the scene.  
**Solution.** Designed and implemented a placement pipeline on RealSense depth data. HDBSCAN clustering in (x, y, depth) space separates the support surface from elevated obstacles; valid placement centers are computed by convolving the obstacle map with the grasped object’s observed mask in two orientations, then cleaned with morphological filtering. A stacking mode validates candidate clusters by fit and orientation (minimum-area rectangle) for placing on top of other objects. Exposed the pipeline as a ROS 2 service (GetPlacement) and integrated it into the SMACH pick-and-place state machine, with 2D-to-3D projection through the lab’s calibrated world-coordinates service and motion executed via the UR ROS 2 driver and MoveIt.  
**Result.** Replaced hard-coded drop locations with runtime placement selection in the lab’s UR10 pick-and-place system, enabling table and stacking placements for arbitrary object shapes as part of the Manipulation-Net benchmarking work.

**Repo:** [GitHub Link](https://github.com/elpis-lab/block-rearrangement).

---

## Romi Trash-Collection Robots
**Stack:** C/C++, Embedded Systems, AprilTags, Sensors  
**Problem.** Simulate real-world waste collection using small autonomous robots.  
**Solution.** Programmed Romi robots to retrieve 3D-printed trash bins from a grid using line-following at intersections and AprilTag detection. Integrated encoder feedback, reflectance arrays, and IMU sensing for robust navigation and handling.  
**Result.** Robots successfully collected and delivered bins in real-world tests, demonstrating reliable coordination of navigation, perception, and object handling.

**Repo:** [GitHub Link](https://github.com/jeroruizfe/RBE3000)
**Images and Videos:** [Images and Videos Link](https://drive.google.com/drive/folders/1aFdzE-Cx_up_Ry9M9-THeSc41uzn5KT_?usp=sharing).

---


## UR10 Robotic Arm — Camera Mount & Calibration
**Stack:** SolidWorks, Hand–Eye Calibration, RealSense, UR10  
**Problem.** Accurate perception required for reliable UR10 manipulator pick-and-place experiments.  
**Solution.** Designed and fabricated a dual-angle camera mount in SolidWorks. Performed hand–eye calibration using fiducial tags to align robot and camera frames.  
**Result.** Achieved calibrated UR10 + RealSense setup with improved object localization, enabling higher-accuracy manipulation tasks.  

---

## Fruit-Picking Autonomous Mobile Robot
**Stack:** Python, Embedded Control, Team Design  
**Problem.** Automate harvesting tasks by navigating an environment and sorting fruit-like objects.  
**Solution.** Collaborated on robot design and programming. Implemented linear navigation, tree alignment, approach logic, fruit recognition, and delivery to correct baskets.  
**Result.** Built and tested an autonomous system capable of navigating obstacles, recognizing targets, and performing task-specific object sorting.

**Images and Videos:** [Images and videos link](https://drive.google.com/drive/folders/1KPRDKVIPFE-sA9-1JCUUQLOuMFVhKRKM?usp=sharing)

---

### Other Work
- **Controls Mini-Labs:** Step response tuning, PID controllers, and Routh-Hurwitz stability analysis.  
- **IK Exercises:** DH modeling + validation for SCARA and 3-DOF manipulators.  
