# Project Status

Last updated: 2026-10-09

## Current milestone

Milestone 0 — environment audit and repository setup (in progress).

## Verified environment

| Item | Observed value |
| --- | --- |
| Operating system | Ubuntu 24.04.5 LTS (Noble) |
| Kernel | Linux 7.0.0-38-generic, x86_64 |
| ROS 2 | Jazzy; environment sourced from `/opt/ros/jazzy/setup.bash` |
| RMW | `rmw_fastrtps_cpp` (Fast DDS) |
| ROS installation | Jazzy Desktop is installed |
| Build tooling | `colcon` 0.21.3 core; common extensions installed |
| RViz | Installed (`rviz2` 14.1.24) |
| Xacro | Installed through the Jazzy desktop installation |
| State publisher | `robot_state_publisher` 3.3.4 installed |
| Teleoperation | `teleop_twist_keyboard` 2.4.1 installed |
| Gazebo executable | No `gz` or `gazebo` executable found |
| Gazebo ROS integration | Full `ros_gz` integration not found; only several vendor support packages are installed |
| Nav2 | Not found |
| SLAM Toolbox | Not found |
| TurtleBot3 simulation | Not found |
| Git | 2.43.0 installed |
| GitHub CLI | 2.45.0 installed for the current user |

## Repository audit

- The original `/home/karem/Kubuntu` folder was not a Git repository.
- It contained the project brief and an empty `test.py` file.
- The brief was moved into this project repository.
- The empty Python file was preserved at `~/Robotics/experiments/python_scratch/test.py`.
- No ROS packages, URDF/Xacro files, launch files, or prior build artifacts existed.
- No `AGENTS.md` instructions were found.

## Chosen organization

The machine now uses `~/Robotics/` as its robotics root:

```text
Robotics/
├── projects/       # Version-controlled portfolio and personal projects
├── learning/       # Focused ROS 2 learning workspaces
├── experiments/    # Disposable prototypes and scratch code
├── reference/      # Personal Linux, ROS, Gazebo, and navigation notes
├── third_party/    # External repositories
├── datasets/       # Bags and larger datasets
├── tools/          # Reusable local utilities
└── archive/        # Inactive work
```

## Simulator strategy

Use modern Gazebo with `ros_gz`, matching ROS 2 Jazzy, rather than Gazebo Classic. Exact packages will be proposed before installation. TurtleBot3 may be installed later as a learning reference, but the portfolio robot will remain a small custom model.

## Risks and open items

- Gazebo, `ros_gz`, Nav2, and SLAM Toolbox are required but currently missing.
- Git identity is configured locally for this repository with GitHub's private `noreply` address.
- GitHub CLI is authenticated as `KaremWaledSa` using HTTPS.
- GUI and simulated-motion acceptance checks will require interactive observation.
- The empty workspace has no package to build yet; initial build validation becomes meaningful in Milestone 1.

## Verified commands and outcomes

```text
cat /etc/os-release                 -> Ubuntu 24.04.5 LTS
printenv | rg '^(ROS|RMW)_'        -> ROS_DISTRO=jazzy; base environment sourced
ros2 doctor --report               -> ROS 2 Jazzy; rmw_fastrtps_cpp
command -v ros2                    -> /opt/ros/jazzy/bin/ros2
command -v colcon                  -> /usr/bin/colcon
command -v gz; command -v gazebo   -> neither executable found
git status                         -> original folder was not a Git repository
gh --version                       -> GitHub CLI 2.45.0
gh auth status                    -> authenticated as KaremWaledSa using HTTPS
```

## Next action

Create and push the initial repository commit. After that, review and approve the minimal ROS/Gazebo dependency installation for Milestones 1–4.
