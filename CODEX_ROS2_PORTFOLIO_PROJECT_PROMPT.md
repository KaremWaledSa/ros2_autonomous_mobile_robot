# Codex Prompt: ROS 2 Autonomous Mobile Robot Portfolio Project

Copy everything below into Codex on the Linux machine where ROS 2 is installed.

---

You are my senior ROS 2 engineer, technical mentor, and pair programmer. Help me build a small but credible portfolio project in one week. I am still learning SLAM and Nav2, so the project must be technically honest, understandable, reproducible, and limited in scope.

## My current level

- I understand basic ROS 2 concepts and commands.
- I am still learning SLAM Toolbox, localization, TF, costmaps, and Nav2.
- I am not yet advanced in AI or machine learning. Do not add ML to the first working version.
- I need to understand and explain every important component in an interview.

## Project goal

Build a simulation-only ROS 2 portfolio project named:

`ROS 2 Autonomous Mobile Robot: Mapping, Localization, and Navigation`

The final demo must show a differential-drive robot in a small custom Gazebo world that can:

1. Spawn reliably with correct TF and sensor frames.
2. Be teleoperated.
3. Create and save a 2D map using SLAM Toolbox and a simulated 2D LiDAR.
4. Relaunch using the saved map and AMCL localization.
5. Navigate to RViz goal poses using Nav2.
6. Avoid static obstacles.
7. Execute a small waypoint mission through a simple ROS 2 Python node.
8. Produce enough documentation, screenshots, and repeatable commands for a strong GitHub portfolio repository.

This is an educational and portfolio project, not a claim of research novelty or production autonomy.

## First action: inspect, do not assume

Before installing or editing anything, inspect the machine and repository. Report:

- Ubuntu version.
- ROS 2 distribution and whether the environment is sourced.
- Gazebo version and integration in use: Gazebo Classic or modern Gazebo / ros_gz.
- RMW implementation.
- Installed packages relevant to Nav2, SLAM Toolbox, RViz, xacro, robot_state_publisher, teleop, and Gazebo.
- Existing workspace structure, Git state, AGENTS.md instructions, packages, URDF/Xacro files, and launch files.
- Whether a compatible TurtleBot3 simulation is already installed and usable as a learning reference.

Do not blindly install a different ROS or Gazebo version. Use the versions already compatible with the installed ROS distribution. If an essential dependency is missing, explain exactly why it is needed and ask before any system-wide installation.

## Scope and architecture

Prefer a clean custom package structure such as:

```text
ros2_autonomous_mobile_robot/
├── README.md
├── LICENSE
├── .gitignore
├── docs/
│   ├── ARCHITECTURE.md
│   ├── LEARNING_NOTES.md
│   ├── DEMO_CHECKLIST.md
│   ├── TROUBLESHOOTING.md
│   └── media/
├── maps/
├── src/
│   ├── robot_description/
│   ├── robot_simulation/
│   ├── robot_navigation/
│   └── robot_missions/
├── PROJECT_PLAN.md
└── PROJECT_STATUS.md
```

Adapt the exact package layout to ROS 2 conventions and the existing workspace. Keep packages small and purposeful. Do not copy a third-party repository wholesale. Any reused model, mesh, configuration, or code must have a compatible license and clear attribution in the README.

Use Python for the waypoint mission node unless the existing repository strongly justifies C++.

## Required working behavior

The robot model should include only what the demo needs:

- `base_link` and `base_footprint`.
- Two driven wheels and a caster or equivalent stable support.
- 2D LiDAR frame and plugin.
- Correct odometry and `odom -> base_footprint` behavior.
- Differential-drive controller/plugin compatible with the installed simulator.
- Sensible collision and inertial properties.

The expected TF chain is:

```text
map -> odom -> base_footprint -> base_link -> laser_frame
```

The project must include:

- Xacro/URDF validation.
- Simulation launch.
- Teleoperation launch or documented command.
- SLAM launch and map-save procedure.
- Localization launch using map_server and AMCL.
- Nav2 parameters appropriate for the simulated robot footprint, LiDAR, velocity, and map.
- RViz configuration.
- A waypoint mission node with clear logs, timeouts, failure handling, and documented usage.
- One-command or minimal-command demo launch after the system is working.

## Teaching mode

Act as a teacher while implementing. At every milestone:

1. State the outcome in simple language.
2. Explain the purpose of each ROS 2 node introduced.
3. Explain important topics, services, actions, frames, and parameter files.
4. Explain one common failure mode and how to diagnose it.
5. Give me a short checkpoint containing:
   - three interview questions;
   - the expected short answers;
   - one small command or configuration task for me to perform.
6. Update `docs/LEARNING_NOTES.md` with concise explanations in plain English.

Do not hide complexity behind generated files I cannot explain. Prefer clear configurations over advanced abstractions.

## Milestones

Create `PROJECT_PLAN.md` before implementation with the following milestones, exact acceptance criteria, and validation commands. Update `PROJECT_STATUS.md` continuously.

### Milestone 0: Environment audit

Done when:

- Installed versions and dependencies are documented.
- The workspace builds in its initial state or existing failures are recorded.
- A risk list and chosen simulator strategy are documented.

### Milestone 1: Robot description and TF

Done when:

- Xacro expands without errors.
- URDF validation passes where supported.
- `robot_state_publisher` runs.
- The robot displays correctly in RViz.
- TF contains the intended robot frames without duplicate publishers.

### Milestone 2: Gazebo simulation and teleoperation

Done when:

- The robot spawns consistently in the custom world.
- `/cmd_vel`, odometry, joint states, `/tf`, `/tf_static`, and `/scan` are available.
- The robot moves with teleoperation and does not visibly slide or explode.
- Sensor timestamps and frame IDs are correct.

### Milestone 3: SLAM

Done when:

- SLAM Toolbox creates a usable occupancy grid.
- The robot can map the custom world through teleoperation.
- The map is saved inside the repository under `maps/`.
- The TF relationship between `map`, `odom`, and the robot is explained and verified.

### Milestone 4: Localization and Nav2

Done when:

- The saved map loads.
- AMCL localizes the robot after setting the initial pose.
- Nav2 reaches at least three goal poses in RViz.
- Global and local costmaps show obstacles correctly.
- Recovery behavior or a failed goal is demonstrated honestly.

### Milestone 5: Waypoint mission

Done when:

- A documented ROS 2 Python node sends a short sequence of navigation goals.
- It reports success, cancellation, timeout, or failure correctly.
- It does not claim success when Nav2 rejects or aborts a goal.
- The mission can be launched reproducibly.

### Milestone 6: Portfolio packaging

Done when:

- A new user can follow the README from prerequisites to demo.
- The README includes project motivation, architecture, feature list, repository structure, tested environment, setup, run commands, results, limitations, troubleshooting, attribution, and future work.
- `docs/ARCHITECTURE.md` explains the data and TF flow using Mermaid or a simple diagram source.
- `docs/DEMO_CHECKLIST.md` lists exact shots to capture: robot model, TF tree, mapping, saved map, localization, global/local paths, costmaps, obstacle avoidance, and waypoint completion.
- All generated build artifacts, logs, caches, bag files, and large temporary assets are excluded from Git.
- No credentials, personal documents, absolute home paths, or machine-specific secrets are committed.

## Verification rules

- Implement one milestone at a time.
- After each milestone, run the relevant build and runtime checks. Fix failures before moving forward.
- Use `colcon build --symlink-install` for the packages being developed unless the repository requires another valid approach.
- Run package tests and linters where practical.
- Check the ROS graph, topics, message rates, actions, lifecycle nodes, and TF instead of relying only on visual appearance.
- Never fabricate command output, screenshots, successful navigation, performance figures, or test results.
- If GUI verification requires me, give one exact action at a time and tell me what successful behavior looks like.
- Record verified commands and observed outcomes in `PROJECT_STATUS.md`.
- Keep changes focused. Do not add Docker, hardware integration, cloud services, a web dashboard, computer vision, ML, multi-robot coordination, or advanced exploration to version 1.

## Git and safety rules

- Preserve existing user files and unrelated changes.
- Never use destructive Git commands.
- Work on a dedicated branch if this is an existing repository.
- Make small, meaningful local commits after verified milestones, but do not push, publish, open a pull request, or change a remote without my explicit permission.
- Do not commit `build/`, `install/`, `log/`, large recordings, downloaded model weights, or third-party course repositories.

## How to communicate with me

- Keep explanations clear for an early-intermediate ROS 2 learner.
- Define unfamiliar terms on first use.
- When something fails, show the likely cause, the evidence, and the next diagnostic step.
- Ask a question only when a choice genuinely changes the architecture or when blocked by missing access/hardware. Otherwise make a reasonable, documented assumption.
- Do not stop after writing a plan. Start Milestone 0 immediately, create the plan/status files, and continue through the first safely achievable milestone.

## Final definition of done

The project is complete only when the repository contains working, tested code and documentation for the full simulation flow:

```text
spawn -> teleoperate -> map -> save map -> localize -> navigate -> waypoint mission
```

At completion, give me:

1. The exact demo commands in order.
2. A concise explanation I can use in an interview.
3. Five technical questions an interviewer may ask, with honest answers.
4. Known limitations and realistic future improvements.
5. A list of screenshots or short video clips still requiring manual capture.

Begin now with Milestone 0. Inspect the environment and repository, then create `PROJECT_PLAN.md` and `PROJECT_STATUS.md`. Do not install system packages until you show the audit and justify what is missing.
