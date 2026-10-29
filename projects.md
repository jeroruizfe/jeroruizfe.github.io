---
layout: default
title: Projects
---

# Projects

## Object-Agnostic Placing Pipeline — ELPIS Lab, WPI
**Role:** Research Assistant (Sep 2024–present)  
**Stack:** Python, ROS 2, OpenCV, HDBSCAN, SciPy, Intel RealSense D435, SMACH, MoveIt, UR10  
**Problem.** In block-rearrangement tasks the robot needs collision-free placement poses for a grasped object at runtime — on a cluttered table or on top of other objects — without a prior model of the object or the scene.  
**Solution.** Designed and implemented a placement pipeline on RealSense depth data. HDBSCAN clustering in (x, y, depth) space separates the support surface from elevated obstacles; valid placement centers are computed by convolving the obstacle map with the grasped object's observed mask in two orientations, then cleaned with morphological filtering. A stacking mode validates candidate clusters by fit and orientation (minimum-area rectangle) for placing on top of other objects. Exposed the pipeline as a ROS 2 service (`GetPlacement`) and integrated it into the SMACH pick-and-place state machine, with 2D→3D projection through the lab's calibrated world-coordinates service and motion executed via the UR ROS 2 driver and MoveIt.  
**Result.** Replaced hard-coded drop locations with runtime placement selection in the lab's UR10 pick-and-place system, enabling table and stacking placements for arbitrary object shapes as part of the ManipulationNet "Text to Placement" benchmarking work. The system now sits alongside a PDDL task planner and predicate-grounding layer, so a goal like `(on green_cube_0 orange_cube_1)` is planned, grounded from the camera, and executed end to end.

**Repo:** [elpis-lab/block-rearrangement](https://github.com/elpis-lab/block-rearrangement)

---

## Autonomous Frontier Exploration on TurtleBot3 — RBE 3002
**Role:** Team of 3, D-Term 2026 (Mar–May)  
**Stack:** ROS 2 Jazzy, Python, NumPy, SciPy, slam_toolbox, AMCL, Nav2 map server, Gazebo, RViz, TurtleBot3  
**Problem.** Make a physical TurtleBot3 explore and map an unknown arena on its own, save the map, then relocalize from scratch and navigate to arbitrary goals in it.  
**Solution.** Built the full autonomy stack as four ROS 2 packages (`exploration`, `planning`, `control`, `simulation`) with no Nav2 planner or controller — only its map server and lifecycle manager.
- **Exploration:** a frontier node detects free cells bordering unknown space in the slam_toolbox map, scores candidates by planner distance, surrounding unknown area, and heading change, and keeps a world-space blacklist of visited and failed frontiers so the robot commits to one target instead of oscillating. A client node closes the loop: request frontier → request plan → drive → repeat until the map is closed, then saves it.
- **Planning:** A* over an obstacle-inflated grid with a BFS **risk map** (distance-from-obstacle) folded into edge cost so paths stay centered in corridors; orientation-aware neighbors, isolated-noise removal, waypoint optimization, and a `validate_trajectory` service the controller calls before committing to a segment.
- **Control:** **pure pursuit** path tracking (0.45 m lookahead, curvature `k = 2·y/ld²`) with a PD `go_to` fallback and smooth drive profiles; odometry via tf2.
- **Localization phase:** on a saved map, a navigator node triggers AMCL global localization, spins the robot while monitoring pose covariance until it converges, then hands off to a goal sender that routes RViz goals through the same planner and controller.

**Result.** The robot autonomously mapped the course on real hardware, relocalized from an unknown start, and drove to clicked goals using our own planner and controller end to end.

---

## Automated Mayer-Rod Coating System — MQP, sponsored by Saint-Gobain
**Role:** Solo Major Qualifying Project, advisor Prof. Constantinos Chamzas (2026–27)  
**Stack:** UR10, UR RTDE, MuJoCo, Python, inverse kinematics  
**Problem.** Mayer-rod draw-down coating is a manual, operator-dependent lab process; Saint-Gobain wants it automated with repeatable strokes, tool management, and verification that the rod is clean between runs.  
**Solution.** Developing a UR10 cell that executes the coating stroke routine through UR's RTDE interface. Built a MuJoCo digital twin of the workcell for IK-based stroke-trajectory generation and validation before running on the physical arm; designing the tool-handling and cleaning-verification steps around it.  
**Status.** In progress — stroke execution is moving from the digital twin to the real UR10.

---

## Approximate Q-Learning Bomberman Agent — RBE 4701 (AI for Robotics)
**Role:** Team 12 (3 students), A-Term 2026  
**Stack:** Python (standard library only — no NumPy/PyTorch), Expectimax, A*, approximate Q-learning  
**Problem.** Escape a Bomberman grid from the top-left corner to the exit by bombing through walls, across five variants ranging from a deterministic empty world to an aggressive monster plus a random monster.  
**Solution.** Each tick the agent enumerates 18 actions (9 moves × bomb/no-bomb), simulates each one for exactly one tick with a lightweight copy of the game rules (monsters commit to their move before the character, so their next step is known), and scores it with a linear Q-function `Q(s,a) = w_mode · f(s,a)`. A three-state machine (ADVANCE / BOMB_WAIT / DANGER) selects the active weight vector. Features include a bomb-aware planner distance to the exit, the distance a bomb would save, a five-tick survival probability from Expectimax over the true monster rules, and blast-zone / monster-proximity terms. A safety shield prunes actions the simulator proves fatal. Weights are learned offline with the standard approximate Q-learning update and evaluated headlessly with multiprocessing.  
**Result.** On 200 held-out seeds per variant: **100% / 99.0% / 99.5% / 95.0% / 94.0%** win rates for variants 1–5. Training from scratch takes ~11 minutes on 2 cores.

---

## Agentic LLM Data Assistant — Argano / Netlogistik
**Role:** Software Engineering Intern (May–Aug 2026, remote)  
**Stack:** MCP, Node.js, SQL, Snowflake, Azure AI Search, Azure AI Foundry  
**Problem.** A single-shot, JSON-parsing chatbot was the SaaS team's natural-language interface to enterprise logistics data for five clients (~$1.2M ACV), and it broke on multi-step questions.  
**Solution.** Migrated the chatbot to an agentic, multi-step tool-use framework built on MCP, connecting natural-language queries to MongoDB, Snowflake, and Power BI. Built a RAG pipeline on Azure AI Search over a ~547-chunk logistics corpus and wired retrieval in as a tool in the function-calling loop. Built an LLM-as-a-Judge evaluation pipeline driven by client feedback signals to measure regressions across releases.  
**Result.** Strengthened the production SaaS offering with a measurable eval loop instead of ad-hoc testing.

---

## Romi Trash-Collection Robots — RBE 3000
**Stack:** C/C++, Embedded Systems, AprilTags, Sensors  
**Problem.** Simulate real-world waste collection using small autonomous robots.  
**Solution.** Programmed Romi robots to retrieve 3D-printed trash bins from a grid using line-following at intersections and AprilTag detection. Integrated encoder feedback, reflectance arrays, and IMU sensing for robust navigation and handling.  
**Result.** Robots collected and delivered bins in real-world tests, coordinating navigation, perception, and object handling.

**Repo:** [jeroruizfe/RBE3000](https://github.com/jeroruizfe/RBE3000) · **Images and videos:** [Drive folder](https://drive.google.com/drive/folders/1aFdzE-Cx_up_Ry9M9-THeSc41uzn5KT_?usp=sharing)

---

## UR10 Robotic Arm — Camera Mount & Calibration
**Stack:** SolidWorks, hand–eye calibration, RealSense, UR10  
**Problem.** Accurate perception was required for reliable UR10 pick-and-place experiments.  
**Solution.** Designed and fabricated a dual-angle camera mount in SolidWorks. Performed hand–eye calibration using fiducial tags to align robot and camera frames.  
**Result.** Calibrated UR10 + RealSense setup with improved object localization, enabling higher-accuracy manipulation tasks.

---

## Fruit-Picking Autonomous Mobile Robot
**Stack:** Python, embedded control, team design  
**Problem.** Automate harvesting tasks by navigating an environment and sorting fruit-like objects.  
**Solution.** Collaborated on robot design and programming. Implemented linear navigation, tree alignment, approach logic, fruit recognition, and delivery to the correct baskets.  
**Result.** Autonomous system capable of navigating obstacles, recognizing targets, and performing task-specific object sorting.

**Images and videos:** [Drive folder](https://drive.google.com/drive/folders/1KPRDKVIPFE-sA9-1JCUUQLOuMFVhKRKM?usp=sharing)

---

### Other Work
- **Controls Mini-Labs:** Step response tuning, PID controllers, and Routh–Hurwitz stability analysis.
- **IK Exercises:** DH modeling + validation for SCARA and 3-DOF manipulators.
