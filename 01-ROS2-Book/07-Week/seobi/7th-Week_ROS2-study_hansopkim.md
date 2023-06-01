---
marp: true
theme: gaia
---

<!-- _class: lead -->

# **ROS2** Book study

#### [7th] Week

###### Created by HanSop Kim ([@seobi](https://github.com/))

---

<!-- paginate: true -->

# 1. ROS 프로그래밍 규칙

**기본 이름 규칙**
```
snake_case, CamelCased, ALL_CAPITALS
```
**C++, Python Style**
```
기본규칙, 라인길이, 이름 규칙, 공백문자 대 탭, 괄호, 주석, 린터, 기타
```
**다른언어 (C 언어, 자바스크립트)**

---

# 2. ROS 프로그래밍 기초 파이썬

- 패키지 생성
- 패키지 설정
- 퍼블리셔 노드 작성
- 서브스크라이버 노드 작성
- 빌드
- 실행

---

# 3. ROS 프로그래밍 기초(C++)

- 패키지 생성
- 패키지 설정
  package.xml, CMakeLists.txt

- 퍼블리셔 노드 작성
- 서브스크라이버 노드 작성

---

# 3. ROS 프로그래밍 기초(C++)

- 빌드
  colcon 빌드 툴 사용
  특정패키지만 선택적으로 빌드 할때는 "--packages-select"
  심볼릭 파일 형태로 저장하려면 "--symlink-install"

- 실행

---

# 4. ROS 2 Tips

- 설정 스크립트 (setup script)

- setup.bash vs local_setup.bash
underlay 개발 환경의 setup.bash를 우선 소싱한 후 자신의 워크스페이스인 overlay 개발환경의
local_setup.bash를 소싱

```
$ source /opt/ros/humble/setup.bash
$ source ~/robot_ws/install/local_setup.bash
```

---

# 4. ROS 2 Tips

- colcon_cd

```
$ colcon_cd 패키지이름
```

- ROS_DOMAIN_ID vs Namespace
 :독립적인 작업시 확인

```
1. 물리적으로 다른 네트워크를 사용
2. ROS_DOMAIN_ID를 이용하여 DDS의 domain을 변경
3. 각 노드 및 토픽/서비스/액션의 이름에 Namespace 추가
```

