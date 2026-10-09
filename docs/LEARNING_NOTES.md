# Learning Notes

## Workspace and repository

A **colcon workspace** is a directory where one or more ROS 2 packages are built together. Its source packages live under `src/`; `colcon` generates `build/`, `install/`, and `log/`.

A **Git repository** records the source code and documentation history. Generated colcon directories should not be committed because they are large, machine-specific, and reproducible from source.

The ROS installation at `/opt/ros/jazzy` is the **underlay**. After this project is built, its `install/` directory becomes an **overlay**. Source the underlay first and the project overlay second.

Common failure: sourcing an old workspace overlay can make ROS load the wrong package version. Diagnose this with `printenv | grep -E '^(AMENT|COLCON|CMAKE_PREFIX_PATH)'` and open a clean terminal when necessary.

## Milestone 0 checkpoint

1. What is a colcon workspace?
   A directory that contains ROS packages and the generated output used to build and run them together.
2. Why are `build/`, `install/`, and `log/` excluded from Git?
   They are generated, machine-specific, and can be recreated from the source.
3. What is a ROS overlay?
   A workspace sourced after a base ROS installation so its packages are added or take precedence.

Small task: in a new terminal, run `echo $ROS_DISTRO`. The expected output on this machine is `jazzy`.
