# 토픽, 서비스, 액션 인터페이스

## ROS2 인터페이스 신규작성

msg_srv_action_interface_example 패키지를 만들고 이 인터페이스 전용 패키지에는 msg인터페이스, srv 인터페이스, action 인터페이스를 포함시킨다.

- ArithmeticArgument.msg
- ArithmeticOperator.srv
- ArithmeticChecker.action

## 인터페이스 패키지 만들기

```
ros2 pkg create --build-type ament_cmake msg_srv_action_interface_example
```
위 명령어 실행 후 msg, srv, action 인터페이스를 작성한다.

## 패키지 설정파일 (package.xml)

인터패이스 전용 패키지를 만들때 필수적인 의존성 패키지는 아래와 같다.이는 일반 패키지와 다른점이다. 
빌드 시 DDS에서 사용되는 IDL(Interface Definition Language) 생성과 관련한 rosidl_default_generators가 사용되고
실행 시 builtin_interfaces와 rosidl_default_runtime이 사용된다

<buildtool_depend>rosidl_default_generators</buildtool_depend>
<exec_depend>builtin_interfaces</exec_depend>
<exec_depend>rosidl_default_runtime</exec_depend>
<member_of_group>rosidl_interface_packages</member_of_group>


## 빌드 설정파일 (CMakeLists.txt)

생성된 패키지에서 아래와 같은 내용을 추가해 주었다.
```
find_package(builtin_interfaces REQUIRED)
find_package(rosidl_default_generators REQUIRED)

set(msg_files "msg/ArithmeticArgument.msg")
set(srv_files "srv/ArithmeticOperator.srv")
set(action_files "action/ArithmeticChecker.action")

rosidl_generate_interfaces(${PROJECT_NAME}
	${msg_files}
	${srv_files}
	${action_files}
	DEPENDENCIES builtin_interfaces
)

ament_export_dependencies(rosidl_default_runtime)
```

# ROS2 패키지 설계(파이썬)

## 패키지 설계

ROS와 연동되는 로봇 프로그램과 일바적인 로봇 프로그램과의 차이는 프로세스를 목적별로 나누어 노드단위의 프로그램을 작성하고,
노드와 노드간의 데이터통신을 고려해 설계해야 한다는 것이다.
ROS2의 토픽, 서비스 액션프로그래밍을 이용해서 각 노드들이 서로 연동되어 구동하는 패키지를 설계하자.
해당 패키지는 ROS2 파라미터(Parameter)와 실행인자(Argument)사용법 그리고 런치(Launch) 파일도 포함한다.

이 패키지 이름을 topic_service_action_rclpy_example 패키지라고 하자.
해당 패키지에 포함된 각각의 노드와 토픽, 서비스, 액션도 고유의 이름을 가지고 있다.
```
ros2 pkg create --build-type ament_python topic_service_action_rcl_example
```

## 노드 작성

topic_service_action_rclpy_example 패키지는 argument노드, operator노드, calcurator노드, checker노드로 구성되어 있다.
이번 장에서는 패키지 및 노드 설계와 빌드, 실행에 대한 내용만 다룬다.

## 패키지 설정 파일 (package.xml)


<depend>rclpy</depend>
<depend>std_msgs</depend>
<depend>msg_srv_action_interface_example</depend>
를 추가해 준다.

## 파이썬 패키지 설정파일 (setup.py)

```
import os
import glob

share_dir = 'share/' + package_name

...

data_files=[
...

(share_dir + '/launch', glob.glob(os.path.join('launch', '*.launch.py'))),
(share_dir + '/param', glob.glob(os.path.join('param', '*.yaml'))),
],

...

entry_points={
	'consolo_scripts':[
	'argument = topic_service_action_rclpy_example.arithmetic.argument:main',
	'operator = topic_service_action_rclpy_example.arithmeti.operator:main',
	'calculator = topic_service_action_rclpy_example.calculator.main:main',
	'checker = topic_service_action_rclpy_example.checker.main:main',
	],
},
```

### data_files

해키지의 다양한 파일을 설치폴더에 추가해야 하기 때문에 빌드 후 심볼릭깅크 파일이나 원본이 복사된 파일이 신규로 생성된다.
이 패키지에는 arithmetic.launch.py 런치파일과 arithmetic_config.yaml 파라미터 파일이 사용되어 위 코드에 추가되었다.

### entry_points

위에 4개의 노드를 작성했다. 이는 ros2 run 과 같은 노드 실행 명령어를 통해 각각 노드를 실행할 수 있다.


## 소스코드 내려받기 및 빌드

미리 작성해 놓은 코드를 내려받은 후 빌드해 본다.

```
cd ros2_humble/src
git clone https://github.com/robotpilot/ros2-seminar-examples.git
cd ..
colcon build --symlink-install --packages-select topic_service_action_rclpy_example
```

## 실행

### 토픽 퍼블리셔
![6 실행(topicpubli)](https://github.com/orocapangyo/opg-book-study/assets/22469193/b2f59425-a3b1-441a-ae84-2731610024e7)

### 서비스 클라이언트
![6 servicecli](https://github.com/orocapangyo/opg-book-study/assets/22469193/95c3d8f1-2e73-4af3-9bbd-06d971514b16)

### 액션 클라이언트 
![6 실행(actioncli)](https://github.com/orocapangyo/opg-book-study/assets/22469193/e319d0f4-cdee-4b6e-8167-d7c402e35d6b)

### 런치 파일
![6 launch](https://github.com/orocapangyo/opg-book-study/assets/22469193/2788eb5e-d05c-4728-bd15-e4a6d8826241)


# 토픽 프로그래밍(파이썬)

## 토픽

비동기식 단방향 메시지 송수신 방식으로 메시지를 퍼블리시 하는 퍼블리셔와 메시지를 서브스크라이브 하는 서브스크라이버간의 통신
1:1, 1:N, N:N, N:1 가능

## 토픽 퍼빌리셔 코드

토픽 퍼블리셔 역할을 하는 argument 노드
create_publisher 함수를 이용해 arthimetic_argument_publisher를 선언한다.

## 토픽 서브스크라이버 코드

토픽 서브스크라이버 역할을 하는 calculator 노드
create_subscription 함수를 이용해 arithmetic_argument_subscriber를 선언한다.

퍼블리셔와 다른점은 get_arithmetic_argument 콜백함수를 두어 퍼블리셔로 부터 메시지를 서브스크라이브 할 때마다 실행되는 함수를 지정한다.

callback_group을 ReentrantCallbackGroup로 지정했는데 콜백함수를 병렬로 실행할 수 있게 해주며 MultiThreadedExecutor와 함께 사용한다.
callback_group를 지정하지 않게 되면 MutuallyExclusiveCallbackGroup가 기본 설정으로 사용되고 이 설정은 한번에 하나의 콜백함수만 실행된다.


# 서비스 프로그래밍(파이썬)

## 서비스

동기식 양방향 메시지 송수신 방식으로 서비스의 요청을 하는 쪽을 서비스 클라이언트, 요청받은 서비사를 수행 후 응답하는 쪽을 서비스 서버라고 한다.
서비스는 특정 요청을 하는 클라이언트 단과 요청받은 일을 수행한 후 결괏값을 전달하는 서버단과의 통신이다.

## 서비스 서버 코드

서비스 서버역할을 하는 calculator 노드
서비스 서버를 선언하는 부분과 콜백함수를 선언하는 부분으로 나뉜다.
create_service 함수를 이용해 arithmetic_service_server를 선언한다.
콜백함수는 get_arithmetic_operator로 지정하고, 멀티스레드 병렬콜백함수의 실행을 위해 callback_group을 설정한다.

## 서비스 클라이언트 코드

서비스 클라이언트 역할을 하는 operator 노드
create_client 함수를 이용해 arithmetic_service_client를 선언한다.
서비스 클라이언트는 토픽 퍼블리셔와 같은 지속적인 수행을 하지 않고, 한번만 수행한다.

# 액선 프로그래밍(파이썬)

## 액션

비동기+동기식 양방향 메시지 송수신방식으로 액션 목표(Goal)를 지정하는 액션클라이언트, 액션 목표를 받아 특정 태스크를 수행하면서 중간 결괏값을 전송하는
액션 피드백, 그리고 최종 결괏값에 해당되는 액션 결과를 전송하는 액션서버 간의 통신이다.

## 액션 서버 코드

액션 서버역할을 하는 calculator노드 (토픽 서브스크라이버, 서비스 서버 역할도 함)
액션 서버 코드는 액션 서버로 선언하는 부분과 콜백 함수를 지정하는 부분이다.
액션 서버는 rclpy.action 모듈의 ActionServer 클래스를 이용하여 arithmetic_action_server로 선언한다.
액션 클라이언트로 부터 액션 목표를 받으면 실행되는 콜백함수는 execute_checker로 지정한다.
멀티스레드 병렬 콜백함수의 실행을 위해 callback_group을 설정했다.

## 액션 클라이언트 코드

액션 클라이언트 역할을 하는 checker 노드
ActionClient 클래스를 이용하여 액션클라이언트를 arithmetic_action_client로 선언
액션 클라이언트에서 수행하는 액션목표는 주기적으로 퍼블리시하는 토픽과 달리 필요 시 비주기적으로 수행한다.
액션 처리과정은 다음과 같다.
- 액션 클라이언트 선언
- 액션 목푯값 전달함수 선언
- 액션 피드백값 콜백함수 선언
- 액션 결괏값 콜백함수 선언

# 파라미터 프로그래밍(파이썬)

## 파라미터

파라미터는 ROS1의 parameter server와 dynamic_reconfigure 패키지의 기능을 모두 가지고 있어 노드가 동작하는 동안 특정값에 대한
저장, 변경, 회수가 가능하다.

ROS2의 모든 노드는 파라미터 서버를 가지고 있어 파라미터 클라이언트와 서비스 통신을 통해 파라미터에 접근할 수 있도록 구현되었다.

서비스가 특정태스크 수행을 위한 요청과 응답이라는 RPC(Remote Procedure Call)에 가까운 목적이라면, 파라미터는 특정 매개변수를 노드 내부 또는
외부에서 쉽게 저장(set)하거나 변경할 수 있고, 쉽게 회수(get)하여 사용할 수 있는 점에서 목적이 다르다.

위에 설명한 argument, operator, calculator, checker노드 중 argument노드, calculator 노드는 파라미터를 사용하고 있다.
argument노드에서 사용되는 파라미터에 대해 알아보자.

## 파라미터 설정

파라미터를 사용하려면 3가지 함수가 필요하다
- declare_parameter 함수
- get_parameter 함수
- add_on_set_parameters_callback 함수


# 실행인자 프로그래밍(파이썬)

## 실행인자

프로그램 실행 시 옵션으로 인수를 추가해 실행하는 경우 사용되는 인자이다.
참고로 파라미터는 매개변수, 아규먼트(Argument)는 실행인자라 작성한다.

## 실행인자 처리

argparse를 import하여 사용한다.

