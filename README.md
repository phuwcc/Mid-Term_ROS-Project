# my_robot_description

ROS 2 package mo phong robot banh xich co canh tay 2 khop trong Gazebo.

Package hien tai gom:

- Mo hinh URDF + mesh cua robot
- Camera, lidar, IMU
- Diff-drive cho de robot bang ban phim
- `ros2_control` cho 2 khop tay `l1`, `l2`
- Node `arm_controller` de nhap goc cho canh tay

## 1. Yeu cau

Moi truong da kiem tra voi ROS 2 Humble.

Can cac goi sau:

```bash
sudo apt update
sudo apt install -y \
  ros-humble-gazebo-ros \
  ros-humble-gazebo-ros2-control \
  ros-humble-ros2-control \
  ros-humble-ros2-controllers \
  ros-humble-xacro \
  ros-humble-joint-state-publisher-gui \
  ros-humble-rviz2 \
  ros-humble-turtlebot3-gazebo \
  ros-humble-teleop-twist-keyboard
```

Neu chua co workspace:

```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
```

Dat package `my_robot_description` vao trong `~/ros2_ws/src`.

## 2. Build Package

```bash
cd ~/ros2_ws
source /opt/ros/humble/setup.bash
colcon build --packages-select my_robot_description
source install/setup.bash
```

Moi terminal moi deu nen chay:

```bash
source /opt/ros/humble/setup.bash
source ~/ros2_ws/install/setup.bash
```

## 3. Chay Gazebo

```bash
cd ~/ros2_ws
source /opt/ros/humble/setup.bash
source install/setup.bash
ros2 launch my_robot_description gazebo.launch.py
```

Launch nay se:

- Nap `robot_description`
- Mo Gazebo voi `turtlebot3_world`
- Spawn robot vao the gioi
- Khoi tao `joint_state_broadcaster`
- Khoi tao `joint_position_controller` cho canh tay

## 4. Dieu Khien De Robot Bang Ban Phim

Robot su dung plugin `gazebo_ros_diff_drive`, vi vay co the dieu khien bang `cmd_vel`.

Mo terminal moi:

```bash
cd ~/ros2_ws
source /opt/ros/humble/setup.bash
source install/setup.bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```

Node nay mac dinh publish len `/cmd_vel`, phu hop voi diff-drive trong URDF.

Phim hay dung:

- `i`: tien
- `,`: lui
- `j`: quay trai
- `l`: quay phai
- `k`: dung

## 5. Dieu Khien Canh Tay

Mo terminal moi:

```bash
cd ~/ros2_ws
source /opt/ros/humble/setup.bash
source install/setup.bash
ros2 run my_robot_description arm_controller
```

Node `arm_controller` publish len topic:

```bash
/joint_position_controller/commands
```

Cach nhap:

```text
l1 l2
```

Vi du:

```text
0.5 0.3
1.0 1.2
```

Y nghia:

- Robot dua `l1` toi goc muc tieu
- Cho 2 giay
- Sau do dua `l2` toi goc muc tieu

Gioi han hien tai:

- `l1`: `-0.4 -> 1.57` rad
- `l2`: `-0.4 -> 1.57` rad

Nhap:

```text
q
```

de thoat node.

## 6. Xem Robot Trong RViz

Neu muon xem nhanh model trong RViz thay vi Gazebo:

```bash
cd ~/ros2_ws
source /opt/ros/humble/setup.bash
source install/setup.bash
ros2 launch my_robot_description display.launch.py
```

## 7. Topic Huu Ich

Kiem tra topic:

```bash
ros2 topic list
```

Mot so topic quan trong:

- `/cmd_vel`
- `/joint_states`
- `/joint_position_controller/commands`
- `/scan`
- `/camera/image_raw`
- `/camera/camera_info`
- `/imu`

## 8. Lenh Kiem Tra Nhanh

Kiem tra controller:

```bash
ros2 control list_controllers
```

Kiem tra joint state:

```bash
ros2 topic echo /joint_states --once
```

Kiem tra camera:

```bash
ros2 topic echo /camera/camera_info --once
```

Kiem tra lidar:

```bash
ros2 topic echo /scan --once
```

## 9. Thu Tu Chay Day Du

Terminal 1:

```bash
cd ~/ros2_ws
source /opt/ros/humble/setup.bash
source install/setup.bash
ros2 launch my_robot_description gazebo.launch.py
```
Terminal 2:

```bash
cd ~/ros2_ws
source /opt/ros/humble/setup.bash
source install/setup.bash
ros2 launch my_robot_description display.launch.py
```

Terminal 3:

```bash
cd ~/ros2_ws
source /opt/ros/humble/setup.bash
source install/setup.bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```

Terminal 4:

```bash
cd ~/ros2_ws
source /opt/ros/humble/setup.bash
source install/setup.bash
ros2 run my_robot_description arm_controller
```
Read data from Sensor:

```bash
ros2 topic echo /imu
ros2 topic echo /scan
```

## 10. Loi Thuong Gap

`ros2 run my_robot_description arm_controller` khong chay:

- Kiem tra da `colcon build`
- Kiem tra da `source install/setup.bash`

Controller bi treo o `waiting for service /controller_manager/list_controllers`:

- Tat Gazebo cu
- Build lai package
- Launch lai tu dau

Robot khong di duoc bang ban phim:

- Kiem tra `teleop_twist_keyboard` da cai
- Kiem tra topic `/cmd_vel` co du lieu:

```bash
ros2 topic echo /cmd_vel
```

Canh tay khong nhuc nhich:

- Kiem tra controller:

```bash
ros2 control list_controllers
```

- Kiem tra topic lenh:

```bash
ros2 topic echo /joint_position_controller/commands
```
