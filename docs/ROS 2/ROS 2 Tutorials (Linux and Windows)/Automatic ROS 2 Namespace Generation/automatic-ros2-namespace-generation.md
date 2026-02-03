# Automatic ROS 2 Namespace Generation
## Environment Infomation
| Item | Description |
|-|-|
| Author | 오민석 |
| Date | 2026-02-03 |
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

## ROS 2 Namespaces
ROS 2에서 namespaces를 관리하는 것은 다중 로봇 시뮬레이션에서 각 로봇의 topic를 고유하게 식별할 수 있도록 하는 데 매우 중요합니다.<br>
현재 OmniGraph 내에서 ROS publisher, subscriber 및 서비스를 위한 namespaces를 설정하는 방법은 크게 두 가지가 있습니다.<br>
1. nodeNamespace 필드의 namespaces 필드를 수동으로 설정합니다.
> <img width="736" height="218" alt="image" src="https://github.com/user-attachments/assets/cbc90cda-6e98-4743-a719-b5e21675275b" /><br>
2. (Recommended) 모든 Isaac Sim ROS OmniGraph 노드의 namespaces를 자동으로 생성하도록 asset을 구성합니다.<br>이 튜토리얼은 Isaac Sim에서 namespaces를 설정하는 과정을 안내하여 효율적인 topic 관리와 충돌 방지를 가능하게 합니다.
<br>

### Configuring the Asset
#### Setting Up the Base Asset
1. **Window > Script Editor**을 열고 다음 코드를 넣고 실행하세요.
> ```python
> # Import necessary modules
> from pxr import UsdGeom
> import omni.usd
> 
> # Retrieve the current stage
> stage = omni.usd.get_context().get_stage()
> 
> # Ensure a stage is loaded
> if not stage:
>     print("No stage is currently loaded. Please load a stage and try again.")
> else:
>     # Create the mock_robot Xform as the root
>     mock_robot = UsdGeom.Xform.Define(stage, "/mock_robot")
> 
>     # Create the base_link Xform under mock_robot
>     base_link = UsdGeom.Xform.Define(stage, "/mock_robot/base_link")
> 
>     # Create lidar_link and position it 0.4 meters above the base_link (Z-axis)
>     lidar_link = UsdGeom.Xform.Define(stage, "/mock_robot/base_link/lidar_link")
>     lidar_link.AddTranslateOp().Set(value=(0, 0, 0.4))  # Offset along Z-axis
> 
>     # Create camera_link and position it 0.2 meters above the base_link (Z-axis)
>     camera_link = UsdGeom.Xform.Define(stage, "/mock_robot/base_link/camera_link")
>     camera_link.AddTranslateOp().Set(value=(0, 0, 0.2))  # Offset along Z-axis
> 
>     # Create wheel_left and wheel_right Xforms under base_link
>     wheel_left = UsdGeom.Xform.Define(stage, "/mock_robot/base_link/wheel_left")
>     wheel_right = UsdGeom.Xform.Define(stage, "/mock_robot/base_link/wheel_right")
> 
>     # Position wheel_left 0.2 meters to the left of the center (X-axis)
>     wheel_left.AddTranslateOp().Set(value=(-0.2, 0, 0))
> 
>     # Position wheel_right 0.2 meters to the right of the center (X-axis)
>     wheel_right.AddTranslateOp().Set(value=(0.2, 0, 0))
> ```

2. **Create > Sensors > RTX Lidar > NVIDIA > Example Rotary 2D**로 이동하여 2D RTX Lidar sensor를 추가하고 `/mock_robot/base_link/lidar_link` 아래로 드래그합니다.

3. **reate > Sensors > Camera and Depth Sensors > LeopardImaging > Hawk**로 이동하여 Hawk stereo camera system을 추가하고 `/mock_robot/base_link/camera_link` 아래로 드래그합니다.

4. **Tools > Robotics > ROS 2 OmniGraphs > Generic Publisher**로 이동하여 Generic Publisher를 만듭니다.<br>**Generic Publisher Graph**를 `Publish String`로 설정하고 **Graph Path**를 `/mock_robot/base_link/wheel_left/String_graph`로 설정합니다.<br>그런 다음 **OK**을 누릅니다.

5. **Tools > Robotics > ROS 2 OmniGraphs > TF Publisher**로 이동하여 TF Publisher를 만듭니다.<br>**Target Prim**을 `/mock_robot`으로 설정하고 **Graph Path**를 `/mock_robot/base_link/wheel_left/TF_graph`로 설정합니다.<br>그런 다음 **OK**을 누릅니다.

6. **Tools > Robotics > ROS 2 OmniGraphs > Camera**로 이동하여 Camera Publisher를 만듭니다.<br>**Camera Prim**을 `/mock_robot/base_link/camera_link/Hawk/left/camera_left`로 설정하고 **Graph Path**를 `/mock_robot/base_link/camera_link/Hawk/Camera_Left_Graph`로 설정합니다. **Depth** 항목을 선택 취소한 다음 **OK**을 누릅니다.

7. **Tools > Robotics > ROS 2 OmniGraphs > Camera**로 이동하여 second Camera Publisher를 만듭니다.<br>**Camera Prim**을 `/mock_robot/base_link/camera_link/Hawk/right/camera_right`로 설정하고 **Graph Path**를 `/mock_robot/base_link/camera_link/Hawk/Camera_Right_Graph`로 설정합니다. **Depth** 항목을 선택 취소한 다음 **OK**을 누릅니다.

8. **ools > Robotics > ROS 2 OmniGraphs > RTX Lidar**로 이동하여 2D RTX Lidar Publisher를 만드세요.<br>**Lidar Prim**을 `/mock_robot/base_link/lidar_link/Example_Rotary_2D`로 설정하고 **Graph Path**를 `/mock_robot/base_link/lidar_link/Lidar_Graph`로 설정합니다. **Laser Scan**만 활성화된 다음 **OK**을 누르세요.













