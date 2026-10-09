# ROS 2 Autonomous Mobile Robot

A simulation-only educational portfolio project for mapping, localization, and navigation with ROS 2 Jazzy.

The intended demonstration flow is:

```text
spawn -> teleoperate -> map -> save map -> localize -> navigate -> waypoint mission
```

## Current status

The repository is in Milestone 0: environment audit and project setup. See [PROJECT_STATUS.md](PROJECT_STATUS.md) for verified machine details and [PROJECT_PLAN.md](PROJECT_PLAN.md) for the delivery plan.

## Workspace layout

ROS 2 packages developed for this project will live in `src/`. Generated `build/`, `install/`, and `log/` directories are intentionally excluded from Git.

## Tested environment

- Ubuntu 24.04.5 LTS
- ROS 2 Jazzy
- RMW: Fast DDS (`rmw_fastrtps_cpp`)

Simulator, navigation, and mapping dependencies are still being audited. No system packages have been installed by this project yet.
