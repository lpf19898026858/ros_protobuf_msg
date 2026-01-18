# ros-protobuf-bridge
技术栈：c++，c++模板，shell，docker，protobuf，cmake，ros序列化库，特征库  
项目简介:  
ros-protobuf-bridge是一个基于ROS和Protobuf的桥接项目，旨在实现 ROS 消息和 protobuf 消息之间的兼容和互操作性。  
主要特点和贡献:  
1. 构建环境自动化: 使用 docker 构建整个项目环境，通过 dockerfile 安装 ROS-Noetic 组件、protobuf、cmake 等依赖项，同时使用 Shell 脚本编写第三方库源码安装和容器操作脚本，以实现项目构建流程的自动化和部署的便利性。  
2. 兼容性和可扩展性: 基于C++模板编程中的 SFINAE 机制，修改了 roscpp 的核心库 roscpp_serialization 和 roscpp_traits 的底层代码。这样，ros-protobuf-bridge可以同时兼容ROS原生msg和Protobuf消息。  
3. 项目管理和构建: 使用 cmake 作为项目的构建系统，编写 proto 测试文件生成相应的静态库，供 ros 自定义功能模块调用。简化了项目的管理和构建过程，并提供灵活的扩展机制，便于后续添加复杂数据类型。  
4. 插件化设计: ros-protobuf-bridge 可以作为一个插件嵌入到各种复杂的ROS功能项目中。通过将该项目中的cmake 指令集成到目标项目中，可以轻松地实现基于proto数据的发布和订阅。
![pb_talker](https://github.com/lpf19898026858/ros_protobuf_msg/blob/main/pb_talker.png)
![pb_listener](https://github.com/lpf19898026858/ros_protobuf_msg/blob/main/pb_listener.png)
## 1.通过项目中dockerfile文件，构建项目镜像 

```bash
cd ~/work/ros_protobuf_msg/docker/build
docker build --network host -t ros_protobuf:noetic  -f ros_x86.dockerfile .
```

## 2.进入docker容器

```bash
cd ~/work/ros_protobuf_msg/docker/scripts
#启动容器
./ros_docker_run.sh
#进入容器
./ros_docker_into.sh
```

## 3.编译代码

```bash
#创建build目录
mkdir build
cd build
cmake ..
make -j6
```

## 4.启动程序

```bash
#先启动roscore，并且启动pb_talker节点
cd /work
source devel/setup.bash
roscore &
rosrun myproject pb_talker
```

```bash
#打开新终端，再次进入容器，启动pb_listener节点
#进入容器中
cd ~/work/ros_protobuf_msg/docker/scripts
#进入容器
./ros_docker_into.sh
rosrun myproject pb_listener
```
