# ROS 2 Ackermann Controller
## Environment Infomation
| Item | Description |
|-|-|
| Author | 오민석 |
| Date | 2026-01-25 |
| OS | Ubuntu 22.04 |
| GPU | NVIDIA RTX 6000 Ada Generation |
| Driver Version | 580.126.09 |
| CUDA Version | 13.0 |
| Isaac Sim Installation Type | Container |
| Container Runtime | Docker |
| Isaac Sim Version | 5.1.0 |
| ROS 2 Distribution | Humble |
| ROS 2 Installation | Native (Host) |
| DDS Implementation | Fast DDS |

## Getting Started
해당 예제를 진행하기 위해 다음을 진행하세요.
- ackermann_msgs ROS 2 package가 필요합니다.<br>다음 명령어를 통해 설치하세요.
> ```bash
> sudo apt install ros-humble-ackermann-msgs
> ```
- Isaac Sim 내에서 **Window > Extensions**로 이동하여 `isaacsim.ros2.bridge`를 Enable하세요.
> <img width="500" alt="image" src="https://github.com/user-attachments/assets/0fa2211b-b9fa-4a93-aa73-92dfd93973b3" />
- ROS 2 워크스페이스에 `isaac_tutorials`, `cmdvel_to_ackermann` 패키지가 있어야 합니다.<br>없을 경우 [ROS 2 Humble Installation](/docs/Installation/ROS%202/ros2-humble-installation.md)를 참고하세요.

## Ackermann Controller and Drive Setup

















