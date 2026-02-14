# ROS 2 Service for Manipulating Prims Attributes
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

## Service Message Types
ROS2 Service Prim 노드는 다음과 같은 메시지 유형의 네 가지 Service를 제공합니다:
> - 특정 경로 아래의 모든 prim path(및 types) 가져오기
> > `isaac_ros2_messages/srv/GetPrims`
> > ```text
> > string path             # get prims at path
> > ---
> > string[] paths          # list of prim paths
> > string[] types          # prim type names
> > bool success            # indicate a successful execution of the service
> > string message          # informational, for example, for error messages
> > ```
> - 특정 프림에 대한 모든 attribute names과 types 가져오기
> > `isaac_ros2_messages/srv/GetPrimAttributes`
> > ```text
> > string path             # prim path
> > ---
> > string[] names          # list of attribute base names (name used to Get or Set an attribute)
> > string[] displays       # list of attribute display names (name displayed in Property tab)
> > string[] types          # list of attribute data types
> > bool success            # indicate a successful execution of the service
> > string message          # informational, for example, for error messages
> > ```
> - prim attribute type 및 values 가져오기
> > `isaac_ros2_messages/srv/GetPrimAttribute`
> > ```text
> > string path             # prim path
> > string attribute        # attribute name
> > ---
> > string value            # attribute value (as JSON)
> > string type             # attribute type
> > bool success            # indicate a successful execution of the service
> > string message          # informational, for example, for error messages
> > ```
> - prim attribute value 설정
> > `isaac_ros2_messages/srv/SetPrimAttribute`
> > ```text
> > string path             # prim path
> > string attribute        # attribute name
> > string value            # attribute value (as JSON)
> > ---
> > bool success            # indicate a successful execution of the service
> > string message          # informational, for example, for error messages
> > ```









































