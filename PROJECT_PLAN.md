# Project Plan

## Working method

- Complete and verify one milestone before advancing to the next.
- Build developed packages with `colcon build --symlink-install`.
- Record commands and observed results in `PROJECT_STATUS.md`.
- Keep generated output and machine-specific data out of Git.
- Make small local commits after verified milestones; publishing requires explicit approval.

## Milestone 0: Environment audit

Acceptance criteria:

- Ubuntu, ROS 2, RMW, simulator, and relevant package availability are documented.
- Initial workspace build succeeds, or the absence of packages and any failures are recorded.
- Existing Git state and project contents are documented.
- Simulator strategy, missing dependencies, and major risks are recorded.

Validation commands:

```bash
cat /etc/os-release
printenv | grep -E '^(ROS|RMW)_'
ros2 doctor --report
colcon list
colcon build --symlink-install
git status --short --branch
```

## Milestone 1: Robot description and TF

Acceptance criteria:

- Xacro expands without errors.
- URDF validation passes where supported.
- `robot_state_publisher` runs and the robot displays correctly in RViz.
- TF contains the intended robot frames without duplicate publishers.

Validation commands:

```bash
xacro <robot-xacro> > /tmp/robot.urdf
check_urdf /tmp/robot.urdf
ros2 launch robot_description display.launch.py
ros2 run tf2_tools view_frames
```

## Milestone 2: Gazebo simulation and teleoperation

Acceptance criteria:

- The robot spawns consistently in a small custom world.
- `/cmd_vel`, odometry, joint states, `/tf`, `/tf_static`, and `/scan` exist.
- Keyboard teleoperation moves the robot without visible instability.
- Sensor timestamps and frame IDs are correct.

Validation commands:

```bash
ros2 launch robot_simulation simulation.launch.py
ros2 topic list
ros2 topic hz /scan
ros2 topic echo /odom --once
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```

## Milestone 3: SLAM

Acceptance criteria:

- SLAM Toolbox produces a usable occupancy grid.
- Teleoperation can map the custom world.
- The map is saved under `maps/`.
- The `map -> odom -> base_footprint` TF relationship is verified and explained.

Validation commands:

```bash
ros2 launch robot_navigation slam.launch.py
ros2 topic echo /map --once
ros2 run tf2_ros tf2_echo map odom
ros2 run nav2_map_server map_saver_cli -f maps/custom_world
```

## Milestone 4: Localization and Nav2

Acceptance criteria:

- The saved map loads and AMCL localizes the robot after an initial pose is supplied.
- Nav2 reaches at least three RViz goal poses.
- Global and local costmaps represent obstacles correctly.
- One recovery or failed-goal case is demonstrated honestly.

Validation commands:

```bash
ros2 launch robot_navigation navigation.launch.py map:=maps/custom_world.yaml
ros2 lifecycle nodes
ros2 action list
ros2 topic echo /amcl_pose --once
```

## Milestone 5: Waypoint mission

Acceptance criteria:

- A ROS 2 Python node sends a short sequence of navigation goals.
- It accurately reports success, rejection, cancellation, timeout, and failure.
- The mission has documented, reproducible launch instructions.

Validation commands:

```bash
ros2 launch robot_missions waypoint_mission.launch.py
colcon test --packages-select robot_missions
colcon test-result --verbose
```

## Milestone 6: Portfolio packaging

Acceptance criteria:

- A new user can reproduce the demo by following the README.
- Documentation covers architecture, learning notes, demo capture, and troubleshooting.
- The README records motivation, features, environment, setup, results, limitations, attribution, and future work.
- Git excludes generated output, recordings, caches, secrets, and machine-specific files.
- Required screenshots and clips are listed and any uncaptured items are clearly marked.

Validation commands:

```bash
git status --short
git check-ignore -v build install log
colcon test
colcon test-result --verbose
```
