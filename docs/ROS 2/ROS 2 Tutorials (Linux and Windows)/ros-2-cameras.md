# ROS 2 Cameras
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

## Setting Up Cameras
| Step | Screenshot | Description |
|-|-|-|
| 1 | <img src="https://github.com/user-attachments/assets/534a28c1-07c3-4b61-ba0e-572bfe966aa4"/> | Content 창에서 **Isaac Sim > Environment > Simple_Room**에서 `simple_room.usd`를 Stage 창으로 드래그 |
| 2 | <img src="https://github.com/user-attachments/assets/e011d9bf-08f6-4d9a-a3f9-fe02df035b30" width="300"/> | **Window > Viewports > Viewports 2**<br>Viewports 2 활성화 |
| 3 | <img src="https://github.com/user-attachments/assets/c2711f5f-b786-4d2d-ae8d-02b3b22bd77b" width="300"/> | **Create > Camera**<br>Camera 2대 생성 |
| 4 | <img src="https://github.com/user-attachments/assets/2b9cdcfe-f5f5-4364-9df2-5e8454eb2428" width="300"/> | Stage 창에서 생성된 카메라를 `Camera_1`, `Camera_2`로 변경 |
| 5 | <img src="https://github.com/user-attachments/assets/76616624-d713-45ed-8f4c-08de10f0871f" width="300"/> | `Camera_1`의 Translate를 `x=-3.5,y=-2.5,z=0.05` Orient를 `x=90.0,y=-50.0,z=0.0`<br>`Camera_2`의 Translate를 `x=3.5,y=-2.5,z=0.05` Orient를 `x=90.0,y=50.0,z=0.0`<br>(`Camera`의 위치와 회전은 임의의 서로 다른 위치로 하셔도 됩니다.) |
| 6 | <img src="https://github.com/user-attachments/assets/06db84f0-bcae-4875-b994-165ee4c0f1e8" width="300"/> | Viewport 상단에 Camere 버튼을 눌러<br>`Viewport 1` **Cameras > Camera_1**<br>`Viewport 2` **Cameras > Camera_2** |

## Building the Graph for an RGB Publisher
1. **Window > Action Editors > Action Graph**
2. **New Action Graph** 클릭
3. 다음과 같이 Action Graph 구성
<img width="971" height="409" alt="image" src="https://github.com/user-attachments/assets/002afdcc-1d58-46f0-a304-e16b524873c9" />

Parameter:
| Node | Input Field | Value |
|-|-|-|
| Isaac Create Render Product | cameraPrim | /World/Camera_1 |
|  | enabled | True |
| ROS 2 Camera Helper | type | rgb |
|  | topicName | rgb |
|  | frameId | turtle |

### Graph Explained
- **On Playback Tick Node**: 시뮬레이션이 "Playing"일 때 틱 생성. 이 노드로부터 틱을 수신한 노드는 시뮬레이션 단계마다 계산 기능을 실행합니다.
- **ROS 2 Context Node**: ROS 2는 미들웨어 통신에 DDS를 사용합니다. DDS는 Domain ID를 사용하여 물리적 네트워크를 공유하더라도 서로 다른 논리적 네트워크가 독립적으로 작동할 수 있도록 합니다. 같은 Domain의 ROS 2 노드는 서로 자유롭게 검색하고 메시지를 보낼 수 있는 반면, 다른 Domain의 ROS 2 노드는 그렇지 않습니다. ROS 2 컨텍스트 노드는 주어진 Domain ID로 컨텍스트를 생성합니다. 기본적으로 0으로 설정되어 있습니다. Domain ID Env Var 사용을 선택하면 현재 Isaac Sim 인스턴스를 실행한 환경에서 `ROS_DOMAIN_ID`를 가져옵니다.
- **Isaac Create Render Product**: 주어진 카메라 프림에서 렌더링된 데이터를 획득하고 그 경로를 렌더 제품 프림으로 출력하는 렌더 제품 프림 만듭니다. 명령에 따라 활성화된 필드를 확인하거나 선택 해제하여 렌더링을 활성화하거나 비활성화할 수 있습니다.
- **Isaac Run One Simulation Frame**: 이 노드는 파이프라인이 처음부터 한 번만 실행되도록 보장합니다.
- **ROS 2 Camera Helper**: publish 할 데이터 유형과 이를 publish 할 ros topic을 나타냅니다.














