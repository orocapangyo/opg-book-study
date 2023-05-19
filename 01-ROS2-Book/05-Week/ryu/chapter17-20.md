# ROS2 도구와 CLI명령어

## ROS2도구
  
   ROS도구에는 그 형태에 따라 4가지로 분류할 수 있다. CLI 형태의 Command-Line Tools, GUI형태의 RQt, 3차원 시각화툴인 RViz, 3차원 시뮬레이터 Gazebo.

## ROS2 CLI 종류와 각 sub-verbs의 기능

   - ROS2실행명령어: ros2 run, ros2 launch
     ```bash
     ros2 run turtlesim turtlesim_node
     ros2 run turtlesim turtle_teleop_key
     ```

   - ROS2 정보명령어: 
     ```bash
     ros2 topic echo /turtle1/cmd_vel
     ```
     ![Screenshot from 2023-05-18 22-18-40](https://github.com/orocapangyo/opg-book-study/assets/22469193/b4460c90-fcda-47e5-a3d5-5b25c605a121)

     
     ```bash
     ros2 interface show turtlesim/srv/Spawn
     ```

   - ROS2 기능보조 명령어
     ```bash
     ros2 run rclcpp_components component_container
     ros2 component list
     ros2 component load /ComponentManager composition composition::Talker
     ```
     ![Screenshot from 2023-05-18 22-39-13](https://github.com/orocapangyo/opg-book-study/assets/22469193/45fca507-493d-4ff8-8767-8550bcc1b387)

     ```bash
     ros2 security create_keystore demo_keys
     ros2 security create_enclave demo_keys /talker_listener/listener
     ```
     ![Screenshot from 2023-05-18 22-48-58](https://github.com/orocapangyo/opg-book-study/assets/22469193/dc393d6b-b378-42a9-95ef-6686df4b3a8e)
     
 

# ROS2 GUI 개발을 위한 RQt
 
## ROS의 종합 GUI 툴 RQt

   RQt는 플러그인 형태로 다양한 목적의 GUI 툴을 모아둔 ROS의 종합 GUI 툴박스이다.
   ROS1 에서 비슷한 기능을 수행하는 GUI가 많아져 공통의 API를 사용하여 개발의 일관성을 높이고자 RQt를 만들었다.


# ROS2의 표준단위

## ROS2의 표준단위의 필요성

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




