# ROS 2 Clock
## Environment Infomation
| Item | Description |
|-|-|
| Author | 오민석 |
| Date | 2026-01-24 |
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

## Simulation Time and Clock
외부 ROS 2 노드가 시뮬레이션 시간과 동기화되는 경우 일반적으로 클록 토픽이 사용됩니다.
RViz2와 같은 많은 ROS 2 노드는 매개변수 use_sim_time을 사용하며,
True로 설정하면 노드가 /clock 토픽에 가입하고 게시된 시뮬레이션 시간과 동기화를 시작하도록 표시합니다.

이 매개변수를 ROS 2 실행 파일에서 설정하거나 새 ROS 2 소스 터미널에서 다음 명령을 사용하여 매개변수를 설정할 수 있습니다:
```bash
ros2 param set /node_name use_sim_time true
```

현재 실행 중인 노드로 /node_name을 교체해야 합니다.
터미널을 사용하여 설정하는 경우 매개변수를 설정하기 전에 노드가 이미 먼저 실행 중이어야 합니다.

## Running ROS 2 Clock Publisher
