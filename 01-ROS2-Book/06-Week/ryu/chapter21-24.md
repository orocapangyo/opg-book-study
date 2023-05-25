# ROS2의 시간

## 시계(Clock)와 시간(Time)

   ROS2는 여러 노드들이 서로 통신하며 다양한 정보(센서 값, 알고리즘을 수행한 결괏값 등)들을 주고 받기 때문에 해당 정보들이 퍼블리시된 정확한 시간이 필수적이다. 이 때문에 ROS2에서는 퍼블리시되는 토픽에 주요 데이터뿐만 아니라 해당 토픽이 퍼블리시되는 시간을 함께 포함시킬 수 있도록 한다. stamp, frame id를 포함하고 있는 std_msgs/msg/header 데이터 타입은 ROS2의 기본 메시지타입 대부분에 포함되어 있다.

![Header msg](https://github.com/orocapangyo/opg-book-study/assets/22469193/b37c9d65-4cf2-49dc-859b-42d4b1b757c7)


   ROS2에서 사용하는 기본시계는 System Clock이며, rclcpp에서는 std::chrono라이브러리를, rclpy는 time 모듈을 캡슐화하여 사용하고 있다. System Clock는 국제 표준시인 협정 세계시(UTC, Coordinated Universal Time)로 표시되어 전세계 어디서든 사용 가능하다. 한국은 표준시보다 9시간 빠르고 'UTC+9'로 표기한다.


## 시간 추상화(Time Abstractions)

   ROS2 에서는 기본시계 외에도 과거의 시점으로 시간을 돌려주거나, 시간을 빠르게 혹은 멈출 수 있는 기능을 가진 시계도 있다.
   이러한 시계는 과거에 기록한 데이터를 다룰 때(ros2bag)나 로봇 시뮬레이션(gazebo) 에서 사용할 수 있고, 이를 통해 개발된 알고리즘을 효율적으로 디버깅 할 수 있다.

   시간 추상화를 통해 총 세가지 시간을 제공한다.

### System Time

   System Clock 사욯하는 시간을 말한다. 타임 서버와의 동기화를 통해 시간이 거꾸로 가는 경우도 있다. server pc와 remote pc간 데이터 통신을 원활히 하기 위해 시간 동기화를 시켜야만 하는데, 다음 명령어를 통해 특정 서버시간으로 동기화 할 수 있다.
```
ntpdate timeserver
```
    timeserver에 시간 서버주소를 넣어주면 해당 서버와 시간동기화를 수행한다.

### ROS Time

    보통 시뮬레이션 환경에서 시간을 조절하기 위해 많이 사용한다.

### Steady Time

    Hardware timeouts를 사용한 시간을 말한다.


## Time API

    ROS2에서 제공하는 시간과 관련된 API는 크게 time, duration, rate가 있다.

### Time

    Time 클래스는 시간을 다룰 수 있는 오퍼레이터를 제공하고 그 결과를 seconds(double형), nanoseconds(unsigned int형) 
    단위로 반환해 준다. ROS2 노드에서는 now 멤버함수를 통해 노드시간을 확인 할 수 있다.
```
    rclcpp::Time now = node->now();
```
### Duration
 
    Duration 클래스는 순간의 시간(timestamp)이 아닌 기간(3시간 후, 1시간 전)을 다룰 수 있는 오퍼레이터를 제공하며 
    그 결과를 seconds, nanoseconds단위로 반환해 준다. Duration은 이전시간을 표현할 수 있고 음수로 표기된다.
    지금시간보다 1초 지난 시간은 다음과 같이 표현할 수 있다.
```
    rclcpp::Duration duration(1,0);
    msg.stamp = now + duration;
```    

### Rate

    Rate 클래스는 반복문에서 특정 주기를 유지시켜주는 API를 제공한다. 
    WallRate 클래스의 생성자에 Hz 단위로 주기를 설정한 후, 반복문 가장아래줄에 sleep 함수로 주기를 맞춰준다.
```
    rclcpp::WallRate loop_rate(1);

    while(rclcpp::ok()){
      ...
      loop_rate.sleep();
    }
```
    하지만 ROS2에서는 콜백함수를 사용하는 Timer API를 제공하고 있기에 이를 사용할 것을 추천한다.



# ROS2의 파일시스템

## 파일시스템

    ROS2 파일시스템을 설명하기 위해 기본적인 폴더, 파일구성, 설치폴더, 사용자 패키지에 대해 알아본다.


## 패캐지와 메타패키지

    ROS2에서 소프트웨어 구성을 위한 기본단위는 패키지로 ROS의 응용프로그램은 패키지 단위로 개발되고 관리된다.
    패키지는 노드를 하나이상 포함하거나 다른 노드를 실행하기 위한 런치(Launch)와 같은 실행 및 설정파일들을 포함하게 된다.
    이러한 패키지는 공통된 목적을 지닌 패키지들을 모아둔 패키지 집합단위인 메타패키지로 관리 되기도 한다.
    예를 들어 Navigation2 메타패키지는 nav2_amcl, nav2_bt_navigator, nav2_costmap_2d, nav2_core, 
    nav2_dwb_controller 등 20여개의 패키지로 구성되어 있다.
    각 패키지는 package.xml(패키지이름, 저작자, 라이선스, 의존성패키지 등의 패키지 정보), CMakeLists.txt(빌드설정), 
    노드의 소스코드, 인터페이스파일(msg,srv,action) 등으로 구성되어 있다.


## 바이너리 설치와 소스코드 설치

    ROS2 패키지 설치는 바이너리 형태로 제공되어 별도 빌드과정 없이 실행하는 방법과 해당 패키지의 소스코드를 직접 내려받은 후
    사용자가 빌드해 사용하는 방법이 있다. 패키지를 수정해 사용할 경우 빌드해 사용하면 된다.

    teleop_twist_joy 패키지를 예로 두 차이점을 기술한다.

### 바이너리 설치
```
    sudo apt install ros-humble-teleop-twist-joy
```

### 소스코드 설치
```
    cd src/
    git clone https://github.com/ros2/teleop_twist_joy.git
    cd ..
    colcon build --symlink-install --packages-select teleop_twist_joy
```

## 기본 설치 폴더와 사용자 작업 폴더
 
### 기본 설치 폴더

    ROS2는 /opt/ros/버전이름 폴더에 설치된다.

### 사용자 작업 폴더

    사용자가 원하는 폴더를 지정할 수 있다.

    예를들어 ~/ros2/ros2_humble
![사용자작업폴더](https://github.com/orocapangyo/opg-book-study/assets/22469193/7591fd3f-9198-4aef-aa26-e44f1cb67786)


    각 폴더의 세부 내용은 다음과 같다.
    build: 빌드 설정용 파일 폴더
    install: msg, srv, action 헤더파일과 사용자 패키지 라이브러리, 실행파일용 폴더
    log: 빌드 로깅파일용 폴더
    src: 사용자 패키지용 폴더



# ROS2의 빌드시스템과 빌드 툴

## 빌드 시스템과 빌드 툴

    빌드 시스템은 단일 패키지를 대상으로 하고 빌드 툴은 시스템 전체를 대상으로 한다.
    단일 패키지와 시스템 전체를 나누게 된 것은 의존성 때문이다. 의존성 레벨은 드라이버 단이나 기본적인 기능만 갖춘 로우 레빌일수록
    가장 기본적인 RCL(ROS CLient Libraries)만 의존성을 갖는 경우도 있고 상위 레빌의 응용일 수록 수십개의 패키지가 필요로 하는
    복합적인 의존성을 가지기도 한다.
    단일 패키지의 경우 빌드시스템을 사용하는데 ROS의 C++에서는 CMake기반의 catkin과 ament_cmake를 사용하고 있으며, 파이썬의
    경우 python의 setuptools를 사용하고 있다. 빌드시스템은 단일 패키지의 의존성을 해결하고 빌드하여 실행가능한 파일을 생성하는
    것으로 생각하면 된다

    ROS에서는 수많은 패키지를 함께 빌드하여 실행시키는 구조이기 때문에 패키지들의 종속성이 매우 얽혀있는 경우 이 얽혀있는 의존성을
    풀고 순서대로 빌드해야만 한다. 이때 사용하는 것이 ROS의 빌드 툴이다.
    ROS의 빌드 툴은 종속성 그래프를 해석하고 순서대로 각 패키지에 대한 특정 빌드시스템을 호출한다.
    ROS 빌드툴로는 rosbuild, catkin_make, catkin_make_isolated, catkin_tools, ament_tools 그리고 현재 ROS2 버전에서 
    널리 사용되고 있는 colcon이 있다.


## 빌드 시스템(Build system)
    
    빌드시스템은 기본적으로 CMake를 사용하고 빌드 환경은 패키지 폴더의 CMakeLists.txt에 기술한다. ROS에서는 CMake를 ROS에
    맞도록 수정해주는 catkin 빌드시스템을 제공해 준다. CMake를 사용하는 이유는 CMake가 유닉스 계열인 리눅스, macOS 뿐 아니라
    윈도우 계열도 지원하기 때무이다. catkin 빌드 시스템은 catkin_make 빌드 툴과 함께 사용되어 ROS와 관련된 빌드, 패키지관리,
    패키지간 의존관계등을 편리하게 사용할 수 있도록 하고있다.

    ROS2에서는 새로운 빌드시스템인 ament를 사용한다. ament_cmake는 ROS1에서 사용되는 빌드시스템인 catkin의 업그레이드 
    버전이다. ROS1의 catkin과 ROS2의 ament가 다른점은 파일 시스템에서 devel공간을 사용하지 않는다는 것과 CMAKE_PREFIX_PATH가 
    아닌 AMENT_PREFIX_PATH와 같은 고유의 환경설정을 사용할 수 있다는 것이다. 그리고 ROS1의 catkin이 CMake만 지원했던 반면,
    ament는 ament_python으로 CMake를 사용하지 않는 파이썬 패키지 관리도 지원한다.

    - ROS1(ROS fuerte까지): rosbuild
    - ROS1(ROS Groovy이후): catkin
    - ROS2: ament(CMake), Python setuptools


## 빌드 툴(Build tools)

    ROS2 에서는 Ardent release까지 빌드도구로 ament_tools가 이용되었고 ROS2 Bouncy부터는 colcon을 추천하고 있다.
    colcon은 CLI타입의 명령어 도구로 colcon build와 같은 형태로 사용한다.


## 패키지 생성

    패키지를 생성하는 방법에는 직접 만드는 방법과 ros2cli명령어를 이용하는 것이다.
    직접 만드는 방법은 파일시스템에 필수적인 package.xml이나 CMakeLists.txt 또는 setup.py 등을 포함시켜주고 소스코드를
    작성하는 것이다.
    ros2cli명령어를 사용하여 패키지를 생성하는 방법은 다음과 같다. 아래 명령어를 실행하는 위치는 ROS2 파일시스템에서 설명하였던
    사용자 작업폴더이다.

```
    ros2 pkg create [패키지이름] --build-type [빌드타입] --dependencies [의존하는 패키지1][의존하는 패키지n]
```
    빌드타입은 C++을 사용한다면 ament_cmake, 파이썬을 사용한다면 ament_python을 입력한다.
```
    ros2 pkg create my_rclcpp_pkg --build-type ament_cmake --dependencies rclcpp std_msgs
    ros2 pkg create my_rclpy_pkg --build-type ament_python --dependencies rclpy std_msgs
```
    ROS에서 패키지 이름은 모두 소문자를 사용하고 공백이 있으면 안된다. 그리고 붙임표(-) 대신 밑줄(_)을 사용해 각 단어를 이어붙이는
    것을 규칙으로 한다. 
    위 명령어에서 의존하는 패키지로 std_msgs와 사용되는 클라이언트 라이브러리에 때라 rclcpp or rclpy가 옵션으로 사용된다.
    명령어 실행후 아래와 같은 구조로 패키지폴더가 생성된다.

![pkgcreate](https://github.com/orocapangyo/opg-book-study/assets/22469193/288d7e6c-1179-4e0a-9c56-ec54b78ea8fb)


## 패키지 빌드

    ROS2 특정 패키지 또는 전체 패키지를 빌드할 때에는 colcon빌드 툴을 사용한다.
    전체 패키지 빌드 시 사용되는 명령어는 다음과 같다.
```
    colcon build --symlink-install
```
    특정 패키지만 빌드할 때 사용되는 명령어는 다음과 같다.
```
    colcon build --symlink-install --packages-select my_rclcpp_pkg
```
    특정 패키지의 의존성 패키지들까지도 함께 빌드할때 사용하는 명령어는 다음과 같다.
```
    colcon build --symlink-install --packages-up-to my_rclcpp_pkg
```

## 빌드 시스템에 필요한 부가기능

    빌드시스템에 필요한 부가기능으로 ROS의 버전컨트롤 시스템 툴인 vcstool, 의존성 관리툴인 rosdep, 바이너리 패키지 관리툴인 
    bloom이 있다.

### vcstool(버전 컨트롤 시스템 툴)

    vcstool의 실행 명령어는 vcs(Version Control System)를 사용한다. 
``` 
    vcs import src < ros2.repos
```
    다양한 버전관리 시스템을 사용하더라도 ROS를 사용함에 있어 불편함 없는 통합적인 툴이 필요해서 만들어진 툴이다.

### rosdep(의존성 관리 툴)

    위에서 언급된 빌드 툴들은 각 패키지의 의존성을 고려해 빌드해 주지만, 의존성 자체를 해결해 주지는 않는다. 이를위해 ROS에서는
    rosdep라는 툴을 쓰고 있는데 이는 package.xml에 기술된 의존성 정보를 가지고 의존성 패키지들을 설치해 주는 역할을 한다.
    각 패키지의 package.xml의 depend 옵션을 보고 사용자가 직접 의존성 패키지를 설치하는 방법도 있지만 rosdep툴을 이용하여 해당
    패키지의 의존성 환경을 해결하는 것은 상당히 편리하다.

### bloom(바이너리 패키지 관리 툴)

    bloom은 바이너리 패키지 배포 및 관리를 위한 툴이다. apt명령어를 통해 쉽게 설치하고 사용할 수 있도록 해준다.



# ROS2의 패키지 파일

## 패키지 파일

    패키지 파일은 패키지 설정파일인 package.xml, 빌드 설정파일인 CMakeLists.txt, 파이썬 패키지 설정파일 setup.py, 
    파이썬 패키지 환경설정파일 setup.cfg, RQt 플러그인 설정파일 plugin.xml, 패키지 변경 로그파일 CHANGELOG.rst, 
    라이선스 파일 LICENSE, 패키지 설명파일 README.md가 있다.


## 패키지 설정 파일(package.xml)

    ROS 패키지의 필수 구성요소로서 패키지의 정보를 기술하는 파일이다.
    기술내용으로 패키지 이름, 저작자, 라이선스, 의존성 패키지 등이 있고 xml형식으로 기술한다.
    각 패키지당 무조건 1개의 패키지 설정파일을 포함한다.

    패키지 설정파일에 대한 작성법은 ROS2 사용자라면 REP 149에 해당되는 Package Format 3문서만 참고하면 된다.


## 빌드 설정 파일(CMakeLists.txt)

    ROS2의 빌드시스템인 ament에서는 C++언어를 사용한 패키지나 RQt Plugin의 경우 CMake를 이용하고 있고 패키지 폴더의 CMakeLists.txt 파일에 빌드환경을 기술해 사용하고 있다. 이 빌드 설정파일에 실행파일 생성, 의존성 패키지 우선빌드, 
    링크생성 등을 설정한다.


## 파이썬 패키지 설정 파일(setup.py)

    ROS2 파이썬 패키지에서만 사용하는 배포를 위한 설정파일로 ROS2 C++ 패키지의 CMakeLists의 기능을 한다고 생각하면 된다.

### 파이썬 패키지 환경설정 파일(setup.cfg)

    ROS2 파이썬 패키지에서만 사용하는 배포를 위한 구성파일이다.
    setup.py의 setup함수에서 설정하지 못한 기타옵션을 이 구성파일에 정의할 수 있다.
    파이썬 패키지에서는 이 파일에 [develop]와 [install] 옵션을 설정하여 스크립트의 저장위치를 설정한다.
![setup cfg](https://github.com/orocapangyo/opg-book-study/assets/22469193/69fef544-fbb4-42e2-8e72-4b536d6a6b7e)

### RQt 플러그인 설정파일(plugin.xml)

### 패키지 변경로그 파일(CHANGELOG.rst)

    패키지의 업데이트 내역을 기술하는 파일이다.

### 라이선스 파일(LICENSE)

    패키지의 코드에 사용된 라이선스를 기술하는 파일이다.

### 패키지 설명 파일(README.md)

    패키지의 부가 설명을 기술하는 파일로 ROS 패키지의 필수 포함 파일은 아니지만 패키지를 사용하는 사용자를 배려하는 문서이다.




   





