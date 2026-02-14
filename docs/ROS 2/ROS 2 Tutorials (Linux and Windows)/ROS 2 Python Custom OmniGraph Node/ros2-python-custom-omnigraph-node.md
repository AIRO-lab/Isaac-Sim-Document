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

6. `custom.python.ros2_node/custom/python/ros2_node/ogn/python/nodes`경로에 있는 `OgnCustomPythonRos2NodePy.py` 파일을 수정합니다.<br>파일 전체 내용을 아래와 같이 수정합니다.
> ```python
> import rclpy
> import std_msgs.msg
> 
> import omni.graph.core
> from isaacsim.core.nodes import BaseResetNode
> 
> from custom.python.ros2_node.ogn.OgnCustomPythonRos2NodePyDatabase import OgnCustomPythonRos2NodePyDatabase
> 
> 
> class OgnCustomPythonRos2NodePyInternalState(BaseResetNode):
>     """Convenience class for maintaining per-node state information.
> 
>     It inherits from ``BaseResetNode`` to do custom reset operation when the timeline is stopped."""
> 
>     def __init__(self):
>         """Instantiate the per-node state information"""
>         self._data = None
>         self._ros2_node = None
>         self._subscription = None
>         # call parent class to set up timeline event for custom reset
>         super().__init__(initialize=False)
> 
>     @property
>     def data(self):
>         """Get received data, and clean it after reading"""
>         tmp = self._data
>         self._data = None
>         return tmp
> 
>     def _callback(self, msg):
>         """Function that is called when a message is received by the subscription."""
>         self._data = msg.data
> 
>     def initialize(self, node_name, topic_name):
>         """Intitialize ROS 2 node and subscription."""
>         try:
>             rclpy.init()
>         except:
>             pass
>         # create ROS 2 node
>         if not self._ros2_node:
>             self._ros2_node = rclpy.create_node(node_name=node_name)
>         # create ROS 2 subscription
>         if not self._subscription:
>             self._subscription = self._ros2_node.create_subscription(
>                 msg_type=std_msgs.msg.Int32, topic=topic_name, callback=self._callback, qos_profile=10
>             )
>         self.initialized = True
> 
>     def spin_once(self, timeout_sec=0.01):
>         """Do ROS 2 work to take an incoming message from the topic, if any."""
>         rclpy.spin_once(self._ros2_node, timeout_sec=timeout_sec)
> 
>     def custom_reset(self):
>         """On timeline stop, destroy ROS 2 subscription and node."""
>         if self._ros2_node:
>             self._ros2_node.destroy_subscription(self._subscription)
>             self._ros2_node.destroy_node()
> 
>         self._data = None
>         self._ros2_node = None
>         self._subscription = None
>         self.initialized = False
> 
>         rclpy.try_shutdown()
> 
> 
> class OgnCustomPythonRos2NodePy:
>     """The OmniGraph node class"""
> 
>     @staticmethod
>     def fibonacci(n):
>         """Compute the Fibonacci sequence value for the given number iteratively"""
>         if n <= 0:
>             return 0
>         elif n == 1:
>             return 1
>         a, b = 0, 1
>         for _ in range(2, n + 1):
>             a, b = b, a + b
>         return b
> 
>     @staticmethod
>     def internal_state():
>         """Get per-node state information."""
>         return OgnCustomPythonRos2NodePyInternalState()
> 
>     @staticmethod
>     def compute(db) -> bool:
>         """Compute the output based on inputs and internal state."""
>         state = db.per_instance_state
> 
>         try:
>             # check if state (ROS 2 node and subscriber is initialized)
>             if not state.initialized:
>                 state.initialize(node_name="custom_python_ros2_node", topic_name=db.inputs.topic)
>             # spin state to take incoming messages
>             state.spin_once()
> 
>             # cache incomming data
>             number = state.data
>             if number is not None:
>                 # compute the Fibonacci sequence value for the given number
>                 value = OgnCustomPythonRos2NodePy.fibonacci(number)
>                 # check for uint64 overflow
>                 if value > 2**64:
>                     db.log_warn(f"Fibonacci number {number} exceeds uint64's storage capacity")
>                     return False
>                 # output value and trigger output execution
>                 db.outputs.fibonacci = value
>                 db.outputs.execOut = omni.graph.core.ExecutionAttributeState.ENABLED
>         except Exception as e:
>             db.log_error(f"Computation error: {e}")
>             return False
>         return True
> 
>     @staticmethod
>     def release(node):
>         """Release per-node state information."""
>         try:
>             state = OgnCustomPythonRos2NodePyDatabase.per_instance_internal_state(node)
>         except Exception as e:
>             return
>         # reset state
>         state.reset()
>         state.initialized = False
> ```

## Running the Custom OmniGraph Node
1. Isaac Sim에서 **Window > Extensions**로 이동하여 `custom.python.ros2_node`을 활성화합니다.
> - **Setting** 클릭
> > <img width="1000" alt="image" src="https://github.com/user-attachments/assets/48409f49-b783-45ce-a047-062c8f49992b" />
> - **Extension Search Paths**에 경로 추가
> > <img width="1000" halt="image" src="https://github.com/user-attachments/assets/31b94141-bd9d-44c6-8f65-e985a27934b1" />
> - **Refresh** 클릭
> > <img width="1000" alt="image" src="https://github.com/user-attachments/assets/f6095bf8-cbb0-48eb-85e8-029a4fda28cf" />
> - 검색창에 `custom.python.ros2_node` 입력 후 **THIRD PARTY** 클릭하여 `custom.python.ros2_node` extension 활성화
> > <img width="1000" alt="image" src="https://github.com/user-attachments/assets/52ce0adb-b572-4714-9aa3-42184a932dae" />

2. **Window > Graph Editors > Action Graph**로 이동하여 Action Graph를 생성 후 다음과 같이 구성합니다.
> <img width="1000" alt="image" src="https://github.com/user-attachments/assets/f246f6bd-fa61-4c9b-96d5-7a06d5f20d76" /><br>
>
> **Custom Python ROS 2 Node** 노드는 커스텀 노드입니다.<br>
> **To String** 노드는 사용자 지정 노드의 출력을 문자열로 변환합니다.<br>
> **Print Text** 노드는 인쇄하여 뷰포트 또는 터미널에 대한 사용자 지정 노드의 출력을 문자열로 표시합니다. 속성 패널에서 `To Screen`을 체크하면 뷰포트에 텍스트가 출력됩니다.<br>
> `To Screen`을 체크합니다.

3. **Play**를 눌러 시뮬레이션을 시작합니다.

4. 새로운 터미널에서 다음 명령을 실행하면 Fibonacci 수가 뷰포트 왼쪽 상단에 나타납니다.
> ```bash
> cd ~/IsaacSim-ros_workspaces/humble_ws/
> export FASTRTPS_DEFAULT_PROFILES_FILE=/home/oms/IsaacSim-ros_workspaces/humble_ws/fastdds.xml
> source /opt/ros/humble/setup.bash
> source install/local_setup.bash
> ```
> ```bash
> ros2 topic pub -1 /number std_msgs/msg/Int32 "{data: 10}"
> ```
> <img width="200" alt="image" src="https://github.com/user-attachments/assets/5bef2eff-3e0e-40df-92a8-a51b51fdfa59" /><br>
> [ros2_python_custom_omnigraph_node.webm](https://github.com/user-attachments/assets/c24c0e8e-febc-400c-b457-0888e882e003)

















