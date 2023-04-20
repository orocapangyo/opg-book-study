# ROS2

## ROS란

   애플리케이션과 분산 컴퓨팅 자원간의 가상화 레이어로 분산컴퓨팅 자원을 활용하여 스케쥴링 및 로드, 감시, 에러처리 등을 실행하는 시스템이다.

## 개발환경 구축
   
   - Ubuntu20.04
   - ROS2 Foxy Filzroy
   - Anaconda with python3.8

   - 책과 아래링크를 참조하여 설치.
   [소스빌드](https://docs.ros.org/en/foxy/Installation/Alternatives/Ubuntu-Development-Setup.html#)

  
   ```bash
   mkdir ros2
   cd ros2
   conda create -n py3.8 python==3.8
   conda activate py3.8
   sudo apt install software-properties-common
   sudo add-apt-repository universe
   sudo apt install curl -y
   sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
   echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
   sudo apt update
   sudo apt install libbullet-dev
   sudo apt install python3-pip
   sudo apt install python3-pytest-cov
   sudo apt install ros-dev-tools
   pip install argcomplete   flake8-blind-except   flake8-builtins   flake8-class-newline   flake8-comprehensions   flake8-deprecated   flake8-docstrings   flake8-import-order   flake8-quotes   pytest-repeat   pytest-rerunfailures   pytest
   sudo apt install --no-install-recommends libasio-dev libtinyxml2-dev libcunit1-dev
   mkdir -p ros2_foxy/src
   cd ros2_foxy
   vcs import --input https://raw.githubusercontent.com/ros2/ros2/foxy/ros2.repos src
   sudo rosdep init
   rosdep update
   sudo rosdep init
   sudo rm /etc/ros/rosdep/sources.list.d/20-default.list 
   rosdep update
   sudo rosdep init
   rosdep install --from-paths src --ignore-src -y --skip-keys "fastcdr rti-connext-dds-5.3.1 urdfdom_headers"
   colcon build --symlink-install
   pip install catkin_pkg
   colcon build --symlink-install
   pip install lark
   colcon build --symlink-install
   pip install empy
   colcon build --symlink-install
   pip install sip
   export PYTHONPATH=$PYTHONPATH:/usr/lib/python3/dist-packages
   colcon build --symlink-install
```
## 예제 실행
### 터미널 실행
```bash
   . ~/ros2/ros2_foxy/install/local_setup.bash
   ros2 run demo_nodes_cpp talker
```
### 다른터미널 실행
```bash
   . ~/ros2/ros2_foxy/install/local_setup.bash
   ros2 run demo_nodes_py listener
```
### 실행결과
![Screenshot from 2023-04-13 22-54-04](https://user-images.githubusercontent.com/22469193/231793315-9cb0adc9-864e-4b38-95c8-b01abfead1c3.png)