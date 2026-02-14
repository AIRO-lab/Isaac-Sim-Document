# ROS 2 Python Custom OmniGraph Node
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

## Creating the ROS 2 Custom OmniGraph Python Node Template
1. **VS Code**에서 **Isaac Sim VS Code Edition**을 설치합니다.
> <img width="1000" alt="image" src="https://github.com/user-attachments/assets/38dcd185-f09f-4a32-9bfe-c620e1fc840f" />

2. **Template > Extension**로 이동하여 Isaac Sim 확장 프로그램을 만드는 마법사에서 다음과 같이 설정하세요.
> **Ext. name**: `custom.python.ros2_node`<br>
> **Ext. path**: 확장 프로그램이 생성될 대상 경로를 정의합니다.<br>
> **Ext. title**: `ROS 2 Python Custom OmniGraph Node`<br>
> **Ready-to-use extension**: 파이썬에서 바로 사용할 수 있는 확장 기능을 생성하려면 이 옵션을 선택하세요.<br>
> **Omnigraph node**: 확장 프로그램을 생성할 때 OmniGraph 관련 파일/폴더를 생성하려면 이 옵션을 선택하세요.<br>
> <img width="1000" alt="image" src="https://github.com/user-attachments/assets/94f0bfc0-90a9-4349-8895-4275b820147c" />

3. **Create**를 클릭하면 **Ext. path**에 기입된 경로에 생성됩니다.
> <img width="300" alt="image" src="https://github.com/user-attachments/assets/9c9a11d1-5471-4b0f-813f-13014f809dff" />

4. 생성된 경로로 이동하여 `custom.python.ros2_node/config` 경로에 `extension.toml` 파일을 수정합니다.<br>아래 내용을 `[dependencies]`아래에 추가하세요.
> ```text
> "isaacsim.ros2.bridge" = {}
> ```

5. `custom.python.ros2_node/custom/python/ros2_node/ogn/python/nodes`경로에 있는 `OgnCustomPythonRos2NodePy.ogn` 파일을 수정합니다.<br>파일 전체 내용을 아래와 같이 수정합니다.
> ```text
> {
>     "CustomPythonRos2NodePy": {
>         "version": 1,
>         "language": "python",
>         "icon": "icons/icon.svg",
>         "uiName": "Custom Python ROS 2 Node",
>         "description": [
>             "This node subscribes to a ROS 2 topic (with message type 'std_msgs/msg/Int32') and computes and outputs the Fibonacci number"
>         ],
>         "categoryDefinitions": "config/CategoryDefinition.json",
>         "categories": ["extension:Category"],
>         "inputs": {
>             "execIn": {
>                 "type": "execution",
>                 "description": "Input execution trigger"
>             },
>             "topic": {
>                 "type": "string",
>                 "uiName": "Subscription topic",
>                 "description": "Topic to subscribe to",
>                 "default": "/number"
>             }
>         },
>         "outputs": {
>             "execOut": {
>                 "type": "execution",
>                 "description": "Output execution trigger"
>             },
>             "fibonacci": {
>                 "type": "uint64",
>                 "uiName": "Fibonacci",
>                 "description": "Computed Fibonacci number"
>             }
>         }
>     }
> }
> ```




















