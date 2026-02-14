# ROS 2 Generic Server and Client
## Environment Infomation
| Item | Description |
|-|-|
| Author | 오민석 |
| Date | 2026-02-14 |
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


## ROS 2 Messages Types
환경에 있는 모든 기존 서비스를 검토하려면 다음을 사용하십시오.
```bash
ros2 interface list --only-srvs
```
ROS distro의 실제 service configuration은 `share/packageName/srv/serviceName` 하위 디렉토리(예: `std_srvs/srv/SetBool` 서비스의 경우 `share/std_srvs/srv/SetBool`)에서 검토할 수 있습니다.

## Generic Server
1. **Window > Graph Editors > Action Graph**로 이동하여 Action Graph를 생성합니다.

2. Action Graph를 다음과 같이 구성합니다.
> [!NOTE]
> **ROS2 Service Server Request**, **ROS2 Service Server Response**의 **Property**를 정의해야 노드의 인풋 및 아웃풋이 생성됩니다.

> <img width="500" alt="image" src="https://github.com/user-attachments/assets/b76d9dc5-6427-4bed-be09-90d66fa6f288" /><br>
> 
> **ROS2 Service Server Request** 노드는 모든 유형의 ROS2 service request 메시지를 receive합니다.<br>
> **ROS2 Service Server Response** 노드는 모든 유형의 ROS2 service request 메시지에 respond합니다.<br>
> 
> **ROS2 Service Server Request**, **ROS2 Service Server Response** 노드의 **Property**에서<br>
> `messagePackage`를 `std_srvs`,<br>
> `messageSubfolder`를 `srv`,<br>
> `messageName`를 `SetBool`<br>
> 로 설정합니다.

> [!NOTE]
> **ROS2 Service Server Request**와 **ROS2 Service Server Response** 노드는 동일한 메시지 필드를 입력해야 하며, Service 이름도 동일해야 합니다. 

3. **Play**를 클릭하여 시뮬레이션을 시작합니다.

4. 새로운 터미널에서 다음 명령어를 실행합니다.
> ```bash
> cd ~/IsaacSim-ros_workspaces/humble_ws/
> export FASTRTPS_DEFAULT_PROFILES_FILE=/home/oms/IsaacSim-ros_workspaces/humble_ws/fastdds.xml
> source /opt/ros/humble/setup.bash
> source install/local_setup.bash
> ```
> ```bash
> ros2 service call /service_name std_srvs/srv/SetBool "{data: true}"
> ```
> <img width="500" alt="image" src="https://github.com/user-attachments/assets/faed16ed-fa7b-4ddf-9c32-7f70dd616aa5" /><br>

## Generic Client
1. **Window > Graph Editors > Action Graph**로 이동하여 Action Graph를 생성합니다.

2. Action Graph를 다음과 같이 구성합니다.
> [!NOTE]
> **ROS2 Service Server Request**, **ROS2 Service Server Response**, **ROS2 ROS2 Service Client**의 **Property**를 정의해야 노드의 인풋 및 아웃풋이 생성됩니다.

> <img width="500" alt="image" src="https://github.com/user-attachments/assets/b76d9dc5-6427-4bed-be09-90d66fa6f288" /><br>
>
> **Generic Server** 예제에서 **ROS2 Service Client** 노드를 추가합니다.<br>
> **ROS2 ROS2 Service Client** 노드는 모든 유형의 ROS2 request 메시지를 보내고 모든 유형의 request 메시지를 receive합니다.
> <br>
> **ROS2 ROS2 Service Client** 노드의 **Property**에서<br>
> `messagePackage`를 `std_srvs`,<br>
> `messageSubfolder`를 `srv`,<br>
> `messageName`를 `SetBool`<br>
> 로 설정합니다.

3. **Play**를 클릭하여 시뮬레이션을 시작합니다.

4. **ROS2 Service Client** 노드의 **Property**에서 `Request:data`를 체크하여 **ROS2 Service Server Request**, **ROS2 Service Server Response** 노드가 응답 및 수신하는지 확인합니다.
> [ros2_generic_server_and_client.webm](https://github.com/user-attachments/assets/2012f4fe-8116-4f01-811c-5ebc0409b9e4)
















