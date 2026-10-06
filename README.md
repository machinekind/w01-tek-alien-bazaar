# Wojtek

## Alien Bazaar: steering a robot dog from AR glasses

Alien Bazaar is our hackathon project: a Snap Spectacles (gen 5) headset
drives Wojtek, a small quadruped robot dog, with nothing but hand pinches
in front of the glasses' cameras.

- **Pinch-drag joystick.** Pinch in the air and drag; the drag sets
  forward speed and turn rate, a glass-locked arrow shows the command,
  and letting go stops the robot. The Lens streams `{"vx", "wz"}` over a
  WebSocket to `wojtek_spectacles_bridge`, which publishes the same
  `/cmd_vel` the gamepad and web console use, with a 0.5 s dead-man.
- **Navigation marker.** A pin on the floor in AR space starts on the
  robot; pinch it and drag it anywhere to set Wojtek's target position,
  and the drag direction sets the heading he turns to on arrival. While
  the marker is off the robot, Wojtek keeps walking to it, his costmap
  can still veto a collision, and the route his planner actually drives
  is drawn on the floor in world space as a live path trail. Dropping
  the marker back on the robot stops him.

Everything underneath is Wojtek's own stack: a MuJoCo MJX/Brax-trained
walking policy, the ROS 2 workspace that runs it, and `wojtek_nav`'s
goal-following and VLM brain.

Where to look:

- [Snap_Spectacles/](Snap_Spectacles/README.md) — the Lens Studio project
  (`Assets/PinchDragNavigator.ts`).
- [wojtek_spectacles_bridge](ros/src/wojtek_spectacles_bridge/README.md)
  — the ROS 2 side of the link.
- [Spectacles host setup](ros/docs/spectacles-lens-setup.md) — getting the
  glasses to reach the simulator, step by step.

The joystick is verified end to end against the MuJoCo simulator; the
navigation marker is landing in a separate change, and driving the
physical robot from the glasses has not been tried yet.

## Origins

The robot began as **4BarBot**, a quadruped designed in The Mechatronics
and Robotics Laboratory of the Wrocław University of Science and
Technology, within the Mechatronics and Theory of Mechanisms Team.
[Jakub Delicat](https://github.com/delipl)'s master thesis,
[*Comparative studies on walking control algorithms for the quadruped robot
4BarBot*](docs/comparative-studies-on-walking-control-algorithms-for-the-quadruped-robot-4barbot.pdf)
(2025), covers the robot and its first walking controllers, and his
[delipl/quadruped_ros2](https://github.com/delipl/quadruped_ros2) repository
is the ROS 2 stack this workspace grew out of.

## Documentation

- [Agent guide](CLAUDE.md)
- [Training configuration reference](training/docs/configuration.md)
- [Guided RL notebook](learning/wojtek_rl_course/README.md) — Colab, step by step: how Wojtek
  is trained, and your policy against the deployed keeper
- [ROS workspace guide](ros/README.md)

## License

Apache-2.0; see [LICENSE](LICENSE) and the attribution notes in
[NOTICE](NOTICE). One exception: the IMU firmware directories under
`ros/src/bmi160_serial_hardware_interface/` and
`ros/src/bmx160_serial_hardware_interface/` contain GPL-licensed AHRS
code; the `LICENSE.md` in each firmware directory is the authoritative
statement of what applies there.
