---
marp: true
theme: gaia
---

<!-- _class: lead -->

# **ROS2** Book study

#### [2nd] Week

###### Created by HanSop Kim ([@seobi](https://github.com/))

---

<!-- _class: invert lead -->

> In Greek mythology, **Gaia** also spelled **Gaea**, was the personification of the Earth and one of the Greek primordial deities.
>
> — *[Gaia (mythology) - Wikipedia, the free encyclopedia](https://en.wikipedia.org/wiki/Gaia_%28mythology%29)*

---

<!-- paginate: true -->

# 5. Why ROS 2?

**ROS2**의 중요 Concept으로 10가지 세부내용

```
1. 시장출시 시간 단축
2. 생산을 위한 설계
3. 멀티 플랫폼
4. 다중 도메인
5. 벤더 선택 기능
6. 공개 표준 기반
7. 자유 재량 허용 범위가 넓은 오픈소스 라이센스 채택
8. 글로벌 커뮤니티
9. 산업 지원
10. ROS 1과의 상호 운용성 확보
```

---

# 6. ROS1 vs ROS2

**ROS1**의 경우 초기 학술 분양에서 사용 되었지만, 차츰 상업적인 사용이 늘면서 특정 기능 및 추가적인 기능의 요구 사항이 필요하면서 기존 호환성을 유지하면서 새로운 기능 추가가 힘들며, 대규모 API 변경이 필요하면서 자연스럽게 ROS의 차세대 기능을 도입한 버전인 **ROS2**가 생겨났음.

---

# 6. ROS2 feature

```
- Platforms
- Real-time : DDS(RTPS:Real-time Publish-Subscribe Protocol)
- Security : TCPROS -> DDS,  DDS-Security / SROS 2
- Communication : DDS, QoS
- Middleware interface : RMW - support virtual API interface
- Node manager : roscore(ROS Master) -> DDS
- Languages :  C++14(C++17), python 3.5+
- Build system : catkin -> ament
- Build tools : colcon
- Build options : Multiple workspace, No non-isolated build, No devel space
- Version control system :  wstool -> vstool
- Client library
- Lifecycle , Multiple nodes, Threading model, Messages,
  Command Line Interface, Launch, Graph API, Embedded Systems
```

---

# 7. ROS2 and DDS

: DDS test with publisher and subscriber node
```
ros2 run demo_nodes_cpp listener
ros2 run demo_nodes_cpp talker
rqt_graph

```

---

# 7. ROS2 and DDS

: How to change RMW(ROS Middleware)
```
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
ros2 run demo_nodes_cpp listener
export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
ros2 run demo_nodes_cpp talker
```
: change Domain
```
export ROS_DOMAIN_ID=11
```

---

# 7. ROS2 and DDS

: QoS Test
```
sudo tc qdisc add dev lo root netem loss 10%
ros2 run demo_nodes_cpp listener
ros2 run demo_nodes_cpp talker
sudo tc qdisc delete dev lo root netem loss 10%
```
: Reliability BEST_EFFORT
```
ros2 run demo_nodes_cpp listener_best_effort
ros2 run demo_nodes_cpp talker
```

---
# 8. DDS of QoS

- Reliability
- History
- Durability
- Deadline
- Lifespan
- Liveliness

---

# 8. DDS of QoS

: QoS Test
```
sudo tc qdisc add dev lo root netem loss 10%
ros2 run demo_nodes_cpp listener
ros2 run demo_nodes_cpp talker
sudo tc qdisc delete dev lo root netem loss 10%
```
: Reliability BEST_EFFORT
```
ros2 run demo_nodes_cpp listener_best_effort
ros2 run demo_nodes_cpp talker
```

---
