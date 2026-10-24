# Patches on top of hku-mars/FAST_LIO (ROS2 branch)

Kept rebasable: one concern per commit, upstream behaviour by default. Used by
[PG-HEAT/rover-slam](https://github.com/PG-HEAT/rover-slam), which pins this fork by SHA.

- **C++17 instead of C++14** (`CMakeLists.txt`). ROS 2 Jazzy's rclcpp headers use
  `std::is_same_v` and other C++17 features, so the C++14 flags fail to compile there.
  C++17 also builds on Humble. Nothing else in the flags changes, including `-O3`.
