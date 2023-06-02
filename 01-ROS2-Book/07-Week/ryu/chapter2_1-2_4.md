# ROS 프로그래밍 규칙(코드 스타일)

## 코드 스타일 가이드
  
   ROS2 developer guide, ROS Enhancement Proposals(REPs)와 같은 가이드와 규칙을 만들고 ROS2 Code style과 같이 일관된 코드 스타일을 지키기 위해 각 언어에 대한 스타일 가이드라인을 세우고
   구성원들의 합의하에 이를 따르고 있다.
   일관된 코드 스타일을 관리하기 위해 린트와 같은 자가 검토툴을 제공한다.

## 기본 이름 규칙

    - 파일 이름 및 변수명, 함수명에서는 모두 소문자로 snake_caske 이름규칙을 사용한다.
    - 타입 및 클래스는 CamelCased 이름 규칙을 사용한다.
    - 상수는 ALL_CAPTITALS 이름규칙을 사용한다.
    - 인터페이스 파일명은 CamelCased 규칙을 따른다. 
       *.msg, *.srv, *.action파일은 *h(pp)및 모듈로 변환 후 구조체 및 타입으로 사용되기 때문이다.
    - 특정 목적에 의해 만들어지는 파일 이름은 예외적으로 대소문자 규칙을 따르지 않고 고유의 이름을 사용한다.
        ex) package.xmp, CMakeLists.txt, README.md, LICENSE, CHANGELOG.rst, .gitignore,.travis.yml, *.repos

## C++ Style

    Google C++ Style Guide를 사용하고 ROS특성에 따라 일부 수정해 사용한다.
    - C++14 Standard를 준수한다.
    - 라인길이: 최대 100문자
    - 이름규칙:
        1) CamelCased, snake_case, ALL_CAPTICALS만 사용
        2) 전역변수는 접두어 g_를 붙임
        3) 클래스 멤버변수는 마지막에 밑줄 _ 을 붙임
    - 기본들여쓰기는 Space 2ㅐ, Tab 사용금지, Class의 접근지정자(public, protected, private)는 들여쓰기 하지 않는다.
    - 주석은 /** */ 사용, 구현주석은 // 사용
    - 린터: 코드스타일 자동 오류검출을 위해 ament_cpplint, ament_uncrustify 사용, 정적코드 분석이 필요한 경우 ament_cppcheck를 사용.
    - 포인터는 char * c 처럼 사용.

## Python Style

    - Python Enhancement Proposals(PEPs)의 PEP8을 준수
    - 기본들여쓰기는 Space 4개, Tab 사용금지
    - 문서주석은 """ tkdyd 구현주석은 # 사용
    - 린터: 코드스타일 자동 오류검출을 위해 ament_flake8을 사용
    - 모든 문자는 작은따옴표 ' ' 사용


# ROS 프로그래밍 기초(파이썬)

## ROS의 Hello World, rcppy 패키지 생성
  
    ros2 pkg create my_first_ros_rclpy_pkg --build-type ament_python --dependencies rclpy std_msgs

## 패키지 설정
    
    package.xml, setup.py, setup.cfg 가 있다.

    setup.py에 있는 entry_points 옵션의 console_scripts 키를 사용하여 실행파일을 설정한다.

## 퍼블리셔 노드 작성 및 서브스크라이버 노드 작성

## 빌드

    colcon build --symlink-install --packages-select my_first_ros_rclpy_pkg
    . install/local_setup.bash

## 실행

    ros2 run my_first_ros_rclpy_pkg helloworld_subscriber
    ros2 run my_first_ros_rclpy_pkg helloworld_publisher


# ROS 프로그래밍 기초(C++)

## ROS의 Hello World, rclcpp 패키지 생성

    ros2 pkg create my_first_ros_rclcpp_pkg --build-type ament_cmake --dependencies rclcpp std_msgs

## 패키지 설정

    package.xml, CMakeLists.txt

## 퍼블리셔 노드 작성 및 서브스크라이버 노드 작성

## 빌드

    colcon build --symlink-install --packages-select my_first_ros_rclcpp_pkg
    . install/local_setup.bash

## 실행

    ros2 run my_first_ros_rclcpp_pkg helloworld_subscriber
    ros2 run my_first_ros_rclcpp_pkg helloworld_publisher


# ROS2 Tips

## 설정 스크립트(setup script)

    새로운 패키지를 빌드한 뒤 다음설정을 한다.
`
source install/local_setup.bash
`

## setup.bash vs local_setup.bash

    underlay: ros가 설치된 경로
    overlay: workspace

## colcon_cd

    현재 작업디렉터리를 패키지 디렉터리로 빠르게변경 가능.
`
colcon_cd 패키지이름
`

    사용하기 위해서는 .bashrc에 추가해야함. p283

## ROS_DOMAIN_ID vs Namespace

    동일 네트워크를 사용하면 다른 사람의 노드정보에 쉽게 접근이 가능하다. 독립적인 작업을 해야할때 작업을 방해하는데
    이를 방지하기위해 3가지 방법이 있다.

    1. 다른네트워크 사용
    2. ROS_DOMAIN_ID 를 이용하여 DDS의 domain 변경
    3. 각 노드 및 토픽/서비스/액션의 이름에 Namespace 추가


## colcon과 vcstool 명령어 자동완성 기능 추가

    자동완성 기능을 사용하려면 아래와 같이 한다.
`
    source share/colcon_argcomplete/hook/colcon-argcomplete.bash
    source share/vcstool-completion/vcs.bash
`







### setup.cfg

   m/s, rad/s를 cm/s, degree/s로 사용하면 원하는 결과물을 얻을 수 없다.
   이러한 불일치를 방지하고자 표준 단위에 관한 규칙을 세웠다.

## ROS2의 표준단위

   국제단위계인 SI단위(SI unit)와 국제단위계의 7개 기본단위를 조합해 만들어진 SI 유도단위(SI derived unit)를 표준단위로 정했다.
   예를 들어 전압단위는 Volt(V), 각의 크기는 Radian(rad), 길이는 Meter(m), 속도는 m/s, 회전속도는 rad/s를 사용해야 한다.

## ROS2의 표준단위 사용

   노드간에 주고받는 데이터의 이름과 자료형을 ROS2 인터페이스에서 규정하고 있다.


# ROS2의 좌표표현

## ROS2의 좌표표현 통일의 필요성
 
   다른 좌표표현 방식을 사용할 경우 발생하는 좌표계 불일치를 사전에 막기위한 규칙이다.

## 좌표표현의 기본 규칙

   모든 좌표계를 오른손 법칙에 따라 표현한다.




