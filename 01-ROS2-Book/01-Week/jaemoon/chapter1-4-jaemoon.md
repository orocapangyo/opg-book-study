**WEEK1 ROS2 Chaptor (1-4)**
==================
###### (챕터 별 중요한 내용 및 궁금한 사항 위주로 정리했습니다)  


> ## **1. ROS2 소개**
+ 로봇 응용 프로그램을 개발하기 위해 로봇에 특화된 다양한 개발환경을 제공하는 로봇 소프트웨어 플랫폼이자 로봇 소프트웨어 개발을 위한 툴

+ ROS = META OPERATING SYSTEM 메타 운영 체제 : 컴퓨팅 자원을 활용하여 스케줄링 및 로드, 감시, 에러 처리 등을 실행하는 시스템

+ ROS가 다른 로봇 소프트웨어 플랫폼과의 차별점은 '로보틱스 소포트웨어 개발을 전 세계 레벨에서 공동 작업이 가능하도록 하는 환경을 구축하는 것' 즉, <span style="color:blueviolet">**소프트웨어 플랫폼, 미들웨어, 프레임워크를 지향하기보다는 로보틱스 연구, 개발에서의 코드 재사용을 극대화하는 것에 초점을 둔 것이 바로 ROS**</span>
  + <span style="color:darkorange">**분산 시스템 구조 :**</span> 여러 컴퓨터나 디바이스에서 로봇 제어와 데이터 처리가 동시에 가능. 이를 통해 로봇 제어 시스템의 유연성과 확장성을 높일 수 있다.
  + <span style="color:darkorange">**모듈성 :**</span> ROS는 모듈성을 강조하며, 각 모듈은 독립적으로 개발 및 테스트 가능.
  + <span style="color:darkorange">**다양한 언어 지원 :**</span> C++,Python,Java 등 다양한 프로그래밍 언어를 지원
  + <span style="color:darkorange">**강력한 시각화 도구 :**</span> RVis 도구 사용. RVis를 사용하여 로봇의 센서 데이터나 제어 명령 등을 시각적으로 확인하거나 디버깅등의 작업 가능
  + <span style="color:darkorange">**네트워크 기능 :**</span> 여러대의 컴퓨터나 분산된 디바이스와 효율적인 데이터 전송 가능
  + <span style="color:darkorange">**활발한 커뮤니티 :**</span> ROS 커뮤니티를 통해 전 세계 다양한 로봇 소프트웨어 개발자들이 지속적으로 ROS를 발전시켜 나가고 있음.

+ <span style="color:red">**[Q]**</span> 현재 Ubuntu 20.04 LTS를 기반으로 ROS 2를 설치했는데 Ubuntu 최신 버전을 설치할 경우 ROS 2 소프트웨어가 제대로 실행되지 않거나 하는 이슈가 있을까요?
+ <span style="color:red">**[Q]**</span> 다른 로봇 소프트웨어를 사용해본 적이 없어서 경험자분들의 의견이 궁금합니다.



> ## **2. ROS2 기반 로봇 개발에 필요한 정보**  

+ ROS 커뮤니티 게시판 (https://discourse.ros.org/)
+ ROS 2 문서 (https://docs.ros.org/)
+ ROS 2 디자인 문서 (http://design.ros2.org/)
+ ROS 위키 (http://wiki.ros.org/)
+ ROSCon 페이지 (https://roscon.ros.org/)
+ ROS 2 리포지터리 (https://github.com/ros2)
+ ROS Enhancement Propsals (https://ros.org/reps/rep-0000.html)
+ ROS 배포판 리포지터리 (https://github.com/ros/rosdistro)
+ ROS 2 패키지 배포현황 (http://repo.ros2.org/status_page/)
+ ROS 질의응답 페이지 (https://answers.ros.org/questions/)
+ ROS 통계 자료 (https://metrics.ros.org/)
+ ROS 커뮤니티 중요 소식 (https://planet.ros.org/)

> ## **3. ROS 2 개발환경 구축**
+ Ubuntu 20.04.6 LTS (Focal Fossa) (https://releases.ubuntu.com/focal/)
+ Ubuntu Install tutorials (https://ubuntu.com/tutorials/install-ubuntu-desktop#1-overview)
+ balenaEtcher - Bootable USB stick (https://www.balena.io/etcher)
+ ROS2 Debians Package (https://docs.ros.org/en/foxy/Installation/Ubuntu-Install-Debians.html)

+ **설치 과정에서 겪은 내용 공유** (설치 오류로 인해 이후 Run commands 설정, IDE 진행 노력중!)
  + ROS 2 패키지를 설치하는 과정에서 아래와 같은 오류 메시지를 받았습니다.  
   $ sudo apt install ros-foxy-desktop ros-foxy-rmw-fastrtps* ros-foxy-rmw-cyclonedds*  
   <span style="color:RED">**E :**</span> Could not open lock file /var/lib/dpkg/lock-fronted - open (13: Permission denied)  
   <span style="color:RED">**E :**</span> Unable to qcquire the dpkg fronted lock (/var/lib/dpkg/lock-fronted). are you root?
  + ROS 개발 툴 설치하는 과정에서 모든 소프트웨어 모음을 다 입력했는데  
  No mudule named pip 의 답변을 계속 받습니다.
  



> ## **4. 왜? ROS2로 가야하는가?**
  > **ROS와 비교했을때 ROS2의 차별점**  

<span style="color:blue">**1. 통신 프레임 워크 (Communication Framework)**</span>
  + ROS는 TCPROS와 UDPROS와 같은 특정 통신 프로토콜을 사용하여 노드간의 통신을 처리했다면 ROS2는 DDS(Data Distribution Service)프로토콜을 기반으로 하며, 데이터 송수신을 위한 다양한 옵션을 제공. 즉, 더욱 신뢰성 높은 통신을 제공  

<span style="color:blue">**2. 멀티 플랫폼 지원 (Multi-platform support)**</span>  
  + ROS는 Linux 운영체제를 위주로 동작했지만 ROS2는 Windows, macOS, Linux 등 다양한 운영체제 위에서 작동이 가능함  

<span style="color:blue">**3. 리얼타임 (Real-time)** </span>
  +  ROS는 기본적으로 리얼타임 시스템을 지원하지 않았지만, ROS2는 RTOS(Real-Time Operating System)와 같은 리얼타임 환경에서 동작할 수 있도록 설계되었음  

<span style="color:blue">**4. 보안 (Security)** </span>
  + ROS2는 기본적으로 데이터 암호화와 인증 기능을 제공하여 보안성을 높였음

<span style="color:blue">**5. 설치 및 빌드 (Intallation and build)** </span>
  + ROS는 종속성 패키지 버전 충돌과 같은 문제로 빌드 과정에서 문제가 간혹 발생되기도 했는데 ROS2는 향상된 빌드 시스템을 제공하여 이러한 문제 개선 

<span style="color:blue">**5. 기타 개선 사항 (Etc.)** </span>
  + ROS2는 메시지 타입 지원이 향상되었으며, 파라미터 서버와 같은 ROS의 기본 기능을 개선 및 자동 코드 생성 도구와 같은 개발자와 같은 다양한 유틸리티도 제공  
##### (Google research data)

