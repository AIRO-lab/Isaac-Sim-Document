# Add Noise to Camera
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

## Running the Example
1. sample script를 실행하세요.
```bash
./python.sh standalone_examples/api/isaacsim.ros2.bridge/camera_noise.py
```
위 명령어를 Docker에서 실행하게 되면 오류 몇 가지가 발생하여 순차적으로 오류 해결법을 제시한다.

**ROS2 Bridge startup failed**
```text
[15.443s] Using backup internal ROS2 jazzy distro
Could not load the dynamic library from /isaac-sim/exts/isaacsim.ros2.bridge/jazzy/lib/librmw_implementation.so. Error: libament_index_cpp.so: cannot open shared object file: No such file or directory

[15.457s] To use the internal libraries included with the extension please set the following environment variables to use with FastDDS (default) or CycloneDDS before starting Isaac Sim:

FastDDS (default):
export ROS_DISTRO=jazzy
export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/isaac-sim/exts/isaacsim.ros2.bridge/jazzy/lib

OR

CycloneDDS:
export ROS_DISTRO=jazzy
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/isaac-sim/exts/isaacsim.ros2.bridge/jazzy/lib


2026-01-28T09:57:54Z [15,436ms] [Error] [isaacsim.ros2.bridge.impl.extension] ROS2 Bridge startup failed
[18.240s] [ext: isaacsim.ros2.bridge-4.12.4] shutdown
```
```bash
export ROS_DISTRO=humble
export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/isaac-sim/exts/isaacsim.ros2.bridge/humble/lib
```

**Augmentation cannot run, script nodes are disabled**
```text
2026-01-28T10:00:39Z [36,731ms] [Warning] [omni.replicator.core.ogn.python.impl.nodes.OgnAugment] Augmentation cannot run, script nodes are disabled.
```
```bash
./python.sh standalone_examples/api/isaacsim.ros2.bridge/camera_noise.py \
  --/app/omni.graph.scriptnode/opt_in=true \
  --/app/omni.graph.scriptnode/enable_opt_in=false
```

