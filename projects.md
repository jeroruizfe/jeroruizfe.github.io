---
layout: default
title: Projects
---


# Projects

## Object-Agnostic Placing Pipeline — ELPIS Lab, WPI
**Role:** Research Assistant (Sep 2024–present) · **Stack:** Python, ROS 2, OpenCV, HDBSCAN, SciPy, RealSense D435, SMACH, MoveIt, UR10

<img src="/assets/ur10_stacking_scene.jpg" alt="Cubes stacked by the UR10 using runtime placement selection" style="max-width:560px;width:100%;border-radius:6px;">
<p style="font-size:0.85em;color:#666;margin-top:4px;">A rearrangement goal executed by the lab's UR10 — each stacked cube was placed at a location chosen at runtime by the pipeline.</p>

**Problem.** In block-rearrangement tasks the robot needs a collision-free placement pose for whatever it is holding — on a cluttered table or on top of another object — with no prior model of the object or the scene.  
**Solution.** From a RealSense depth frame, HDBSCAN clustering in (x, y, depth) separates the support surface from elevated obstacles; convolving the obstacle map with the grasped object's observed mask yields valid placement centers. A stacking mode validates candidate surfaces by fit and orientation. Exposed as a ROS 2 service (`GetPlacement`) and wired into the SMACH pick-and-place state machine.  
**Result.** Replaced hard-coded drop locations with runtime placement selection in the lab's ManipulationNet "Text to Placement" system; now sits under a PDDL task planner so a goal like `(on green_cube_0 orange_cube_1)` is planned, grounded from the camera, and executed end to end.

<details markdown="1"><summary>Details</summary>

- Depth normalization and 2D→3D projection through the lab's calibrated world-coordinates service.
- Obstacle-mask convolution is run in two orientations of the object mask, then cleaned with morphological filtering.
- Stacking mode fits a minimum-area rectangle to each elevated cluster and rejects clusters the object would overhang.
- Motion executed via the UR ROS 2 driver and MoveIt; the SMACH sequencer handles detect → pick → place with failure recovery.
</details>

**Repo:** [github.com/elpis-lab](https://github.com/elpis-lab)

---

## Autonomous Frontier Exploration on TurtleBot3 — RBE 3002
**Role:** Team of 3, D-Term 2026 · **Stack:** ROS 2 Jazzy, Python, NumPy, SciPy, slam_toolbox, AMCL, Gazebo, RViz, TurtleBot3

<img src="/assets/turtlebot3_risk_astar.png" alt="Risk-weighted A* vs plain A* on a course map" style="max-width:560px;width:100%;border-radius:6px;">
<p style="font-size:0.85em;color:#666;margin-top:4px;">Our planner's risk-weighted A* (green) vs. plain shortest-path A* (red) on a course map, using the planner's actual edge-cost function. The risk term pushes paths away from walls.</p>

**Problem.** Make a physical TurtleBot3 explore and map an unknown arena on its own, save the map, then relocalize from scratch and drive to arbitrary goals in it.  
**Solution.** Our own exploration, planning, and control stack in four ROS 2 packages — no Nav2 planner or controller. A frontier node picks the next target by planner distance, surrounding unknown area, and heading change; A\* runs over an inflated grid with a BFS distance-from-obstacle risk map in the edge cost; a pure-pursuit controller tracks the path. On a saved map, AMCL global localization with a covariance-monitored spin hands off to goal-driven navigation.  
**Result.** The robot autonomously mapped the course on real hardware, relocalized from an unknown start, and drove to clicked goals end to end.

<details markdown="1"><summary>Details</summary>

- **Exploration:** free cells bordering unknown space are scored and the winner is published as a goal; a world-space blacklist of visited and failed frontiers stops the robot oscillating between targets. A client node loops request-frontier → plan → drive until the map is closed, then saves it with the Nav2 map server.
- **Planning:** `GetPlan` service; obstacle inflation by the robot footprint, isolated-noise removal, orientation-aware 8-neighbors, waypoint optimization, and a `validate_trajectory` service the controller calls before committing to a segment. Edge cost = Euclidean + `16.5·exp(−0.15·risk)`.
- **Control:** pure pursuit with a 0.45 m lookahead and curvature `k = 2·y / ld²`, a PD `go_to` fallback, smooth drive profiles, odometry via tf2.
- **Localization:** a navigator node triggers AMCL global localization and spins the robot while monitoring pose covariance until it converges; a goal sender routes RViz goals through the same planner and controller.
</details>

**Repo:** [jeroruizfe/RBE-3002-D26-Team1](https://github.com/jeroruizfe/RBE-3002-D26-Team1)

---

## Automated Mayer-Rod Coating System — MQP, sponsored by Saint-Gobain
**Role:** Solo Major Qualifying Project, advisor Prof. Constantinos Chamzas (2026–27) · **Stack:** UR10, UR RTDE, MuJoCo, Python, inverse kinematics

**Problem.** Mayer-rod draw-down coating is a manual, operator-dependent lab process; Saint-Gobain wants repeatable strokes, tool management, and verification that the rod is clean between runs.  
**Solution.** A UR10 cell that executes the coating stroke routine through UR's RTDE interface. A MuJoCo digital twin of the workcell generates and validates IK-based stroke trajectories before anything runs on the physical arm; tool-handling and cleaning-verification steps are being designed around it.  
**Status.** In progress — stroke execution is moving from the digital twin to the real UR10.

---

## Approximate Q-Learning Bomberman Agent — RBE 4701 (AI for Robotics) {#bomberman}
**Role:** Team 12 (3 students), A-Term 2026 · **Stack:** Python standard library only, Expectimax, A\*, approximate Q-learning

<img src="/assets/bomberman_variant5.gif" alt="The agent escaping variant 5 (aggressive + random monsters)" style="max-width:220px;width:100%;border-radius:6px;">
<p style="font-size:0.85em;color:#666;margin-top:4px;">Variant 5 — two monsters, one aggressive. The agent bombs through walls and reaches the exit.</p>

**Problem.** Escape a Bomberman grid by bombing through walls, across five variants from a deterministic empty world to an aggressive monster plus a random one.  
**Solution.** Each tick the agent enumerates 18 actions (9 moves × bomb/no-bomb), simulates each for exactly one tick with a lightweight copy of the game rules, and scores it with a linear Q-function whose weight vector is chosen by a three-state machine (ADVANCE / BOMB_WAIT / DANGER). A safety shield prunes actions the simulator proves fatal. Weights are learned offline with the approximate Q-learning update.  
**Result.** On 200 held-out seeds per variant: **100% / 99.0% / 99.5% / 95.0% / 94.0%** win rates for variants 1–5.

<details markdown="1"><summary>Details</summary>

- Monsters commit to their move before the character, so the one-tick simulation of each action is exact.
- Features: bomb-aware planner distance to the exit (walls cost one bomb cycle), distance a bomb would save, a five-tick survival probability from Expectimax over the true monster rules, blast-zone and monster-proximity terms.
- `train.py` learns all five variants in ~11 minutes on 2 cores; `evaluate.py` plays hundreds of seeded games headlessly with multiprocessing.
- No NumPy or PyTorch — the whole agent is `json`, `heapq`, `random`, `collections`, `multiprocessing`.
</details>

**Repo:** [jeroruizfe/RBE470x-projects](https://github.com/jeroruizfe/RBE470x-projects) · **Report:** [PDF](https://github.com/jeroruizfe/RBE470x-projects/blob/main/Team12/report.pdf)

---

## Agentic LLM Data Assistant — Argano / Netlogistik
**Role:** Software Engineering Intern (May–Aug 2026, remote) · **Stack:** MCP, Node.js, SQL, Snowflake, Azure AI Search, Azure AI Foundry

**Problem.** A single-shot, JSON-parsing chatbot was the SaaS team's natural-language interface to logistics data for five enterprise clients (~$1.2M ACV), and it broke on multi-step questions.  
**Solution.** Migrated it to an agentic, multi-step tool-use framework on MCP connecting natural-language queries to MongoDB, Snowflake, and Power BI. Built a RAG pipeline on Azure AI Search over a ~547-chunk logistics corpus, exposed as a tool in the function-calling loop, and an LLM-as-a-Judge evaluation pipeline driven by client feedback signals.  
**Result.** A production assistant with a measurable eval loop instead of ad-hoc testing.

---

## Romi Trash-Collection Robots — RBE 3000
**Stack:** C/C++, embedded systems, AprilTags, encoders, reflectance array, IMU

**Problem.** Simulate waste collection with small autonomous robots on a grid.  
**Solution.** Romi robots retrieve 3D-printed bins using line following at intersections and AprilTag detection, fusing encoder, reflectance, and IMU feedback.  
**Result.** Robots collected and delivered bins in real-world tests.

**Repo:** [jeroruizfe/RBE3000](https://github.com/jeroruizfe/RBE3000) · **Images and videos:** [Drive folder](https://drive.google.com/drive/folders/1aFdzE-Cx_up_Ry9M9-THeSc41uzn5KT_?usp=sharing)

---

## UR10 Camera Mount & Hand–Eye Calibration
**Stack:** SolidWorks, hand–eye calibration, fiducial tags, RealSense, UR10

**Problem.** Reliable pick-and-place needed an accurate camera-to-robot transform.  
**Solution.** Designed and fabricated a dual-angle camera mount in SolidWorks; performed hand–eye calibration with fiducial tags to align robot and camera frames.  
**Result.** Calibrated UR10 + RealSense setup with improved object localization.

**Repo:** [jeroruizfe/Calibration](https://github.com/jeroruizfe/Calibration)

---

## Fruit-Picking Autonomous Mobile Robot
**Stack:** Python, embedded control, team design

**Problem.** Navigate an environment and sort fruit-like objects into the right baskets.  
**Solution.** Linear navigation, tree alignment, approach logic, fruit recognition, and delivery.  
**Result.** Autonomous navigation around obstacles with task-specific object sorting.

**Images and videos:** [Drive folder](https://drive.google.com/drive/folders/1KPRDKVIPFE-sA9-1JCUUQLOuMFVhKRKM?usp=sharing)

---

### Other Work
- **Controls Mini-Labs:** step-response tuning, PID controllers, Routh–Hurwitz stability analysis.
- **IK Exercises:** DH modeling and validation for SCARA and 3-DOF manipulators.
