# Patches on top of hku-mars/FAST_LIO (ROS2 branch)

Kept rebasable: one concern per commit, upstream behaviour by default. Used by
[PG-HEAT/rover-slam](https://github.com/PG-HEAT/rover-slam), which pins this fork by SHA.

- **C++17 instead of C++14** (`CMakeLists.txt`). ROS 2 Jazzy's rclcpp headers use
  `std::is_same_v` and other C++17 features, so the C++14 flags fail to compile there.
  C++17 also builds on Humble. Nothing else in the flags changes, including `-O3`.
- **`/Odometry` pose covariance** (`src/laserMapping.cpp`, `publish_odometry`). Upstream copied
  it in after publishing, so each message carried the previous scan's covariance and the first
  carried zeros; it also put the rotation block where ROS expects position, and left rotation in
  body axes. It is now filled before publishing, in ROS order (x, y, z, rx, ry, rz), with both
  blocks in `camera_init` axes: position is additive in `camera_init`, and IKFoM perturbs
  rotation on the right (`SO3::boxplus`, R·Exp(δ)), so the rotation rows are rotated by R. Not
  behind a parameter: the old values were wrong, not an alternative. Twist is still not filled.
