# 5 - 8 장 요약

## ROS 2의 중요 콘셉트

   로봇 응용 프로그램을 위한 오픈소스 소프트웨어(Apache 2.0) 개발키트이다. 
   ROS2의 목적은 로봇 산업 전반의 개발자에게 연구 및 프로토 타이핑에서 배포 및 생산에 이르는 표준 소프트웨어 플랫폼을 제공하는 것이다.

## ROS 1과 2 의 차이점으로 알아보는 ROS2의 특징
   - Platforms: 리눅스, 윈도우, macOS 모두 지원
   - Real-time: 실시간성 지원. (3부 9장)
   - Security: TCP기반 통신은 OMG(Object Management Group)에서 산업용으로 사용중인 DDS(Data Distribution Service)를 도입. DDS-Security를 통해 보안이슈 해결.
               SROS2(Secure Robot Operation System2) 개발하여 보안관련 RCL서포트 강화.
               (ros2 dds security, ros2 access control policies, ros2 secutiry enclaves, ros2 threat model, ROSCon2019-Is your robot secure?)
   - Communication: RTPS(Real Time Publish Subscribe) 지원하는 통신 미들웨어 DDS사용. (1부 7-8장) 
   - Middleware interface: DDS벤더는 10곳이 있고, 이런 벤더들의 미들웨어를 유저가 원하는 목적에 맞게 선택하여 사용할 수 있게 ROS Middlewate(RMW) 형태로 지원한다.
   - Node manager(Discovery): ROS1의 roscore가 없어지고, 노드는 DDS의 Participant로 취급하게 되었고, 
                              Dynamic Discovery를 이용해 DDS미들웨어를 통해 노드를 직접 검색하여 연결할 수 있게 됨.
   - Build system and tools: ament사용했으니 지금은 colcon 추천. (1부 23장) 
   - vcs(Version Control System): vcstool로 통합.
   - Client library: rclcpp, rclc, rclpy, rcljava, rclobjc, rclada, rclgo, rclnodejs 등으로 제공됨.
   - Lifecycle: 패키지의 각 노드들의 현재 상태를 모니터링 하고 상태 제어가 가능한 Lifecycle을 클라이언트 라이브러리에 포함시켰고, 
                이를통해 ROS 시스템 상태를 효과적으로 제어할 수 있게 되멌다.(3부7장)
   - Multiple nodes: 동일한 실행파일에서 복수의 노드를 실행시킬 수 있게 됨. (3부 3장, 3부 5장)
   - Threading model: single threaded executor, multi threaded executor for rclcpp
   - Messages(Topic, Service, Action): 단일 데이터 구조를 메시지라고 정의. ROS2에서는 OMG에서 정의된 IDL(Interface Description Language)를 
                                       사용해 메시지 정의 및 직렬화를 더 쉽고 포괄적으로 다룰 수 있게 됨.
   - Command Line Interface: (1부 17장, 3부 2장)
   - Launch: run은 단일 프로그램 실행, launch는 사용자 지정 프로그램 실행을 수행함. (2부 18장)
   - Graph API: 노드가 시작할 때 뿐 아니라 실행도중 재미핑이 가능하며, 그 결과를 바로 그래프로 표현할 수 있게 하려고 하고 있다. 아직 구현되지는 않았다.
   - Embedded Systems: (ROSCon2015-ROS2 on small embedded systems)

## ROS2와 DDS
   - DDS는 데이터 분산시스템 이라는 개념을 나타내는 단어다. 실제로는 데이터를 중심으로 연결성을 갖는 미들웨어의 프로토콜(DDSI-RTPS)과 같이 DDS사양을 만족하는 미들웨어 API가 그 실체이다.


## DDS의 QoS(Quality of Service)
   - 22가지의 QoS항목이 있다.
   - ROS2의 RMW에서 QoS 설정을 쉽게 사용할 수 있도록 가장 많이 사용하는 QoS설정을 세트로 표현해둔 것이 있는데 이를 RMW QoS Profile 이라고 한다.
     목적에 따라 Default, Sensor, Data, Service, Action Status, Parameters, Pameter Events와 같이 6가지로 구분한다.
   - 유저가 직접 설정하여 새로운 QoS 프로파일을 만들어 사용할 수 있다. 

## Test
### RMW(ROS Middleware) & rqt_graph
![Screenshot from 2023-04-18 23-06-27](https://user-images.githubusercontent.com/22469193/232819873-96977aab-72b5-4e08-92ed-8f8ad185801a.png)

### RMW 변경
![Screenshot from 2023-04-18 23-09-50](https://user-images.githubusercontent.com/22469193/232820044-b9ed7bcb-92ce-4042-a29e-23e94635dcb1.png)

### RMW 상호운용
![Screenshot from 2023-04-18 23-10-49](https://user-images.githubusercontent.com/22469193/232820403-f48ab9f1-78a9-4b8b-8184-d9b75d77726d.png)

### Domain 변경
![Screenshot from 2023-04-18 23-14-46](https://user-images.githubusercontent.com/22469193/232820516-25360027-1f7c-472a-9ad1-0e0165133910.png)

### QoS 테스트: Reliability가 RELIABLE로 되어있을 때
![Screenshot from 2023-04-18 23-25-40](https://user-images.githubusercontent.com/22469193/232821573-7b3c4d3d-e28b-4b99-b96a-220372b9d086.png)

### QoS 테스트: Reliability가 BEST_EFFORT로 되어 있을 때 
![Screenshot from 2023-04-18 23-28-36](https://user-images.githubusercontent.com/22469193/232820579-55c9761b-df13-4082-a3f5-cab946262f9d.png)