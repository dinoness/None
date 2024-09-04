### 终端基本指令
`ls`：返回当前文件夹下的内容  
`mkdir name0`:新建一个为"name0"的文件夹  
`cd dir_name`:进入"dir_name"文件夹  
`cd ..`:回到上层目录  
`cd ~`:回到主文件夹  
`cd /bin`:来到根目录下的某个文件夹  
`gedit filename`:在当前目录下 创建/编辑 一个文件  
`vim filename`:在当前目录下 创建/编辑 一个文件   
`source filename`:执行制定文件中的命令  
`rm -r 文件夹路径`:文件夹删除

###### DEB格式安装教程
https://blog.csdn.net/Xeno233/article/details/118278311  
`cd /home/index10/下载`到达安装包所在文件夹  
``sudo dpkg -i 文件名.deb`进行安装

###### 解压缩
https://blog.csdn.net/2301_80317247/article/details/139904932  
zip文件：直接右键解压缩

###### 软件卸载
`sudo apt-get remove --purge 软件名`  
如果不知道软件名是什么，可以进行局部词检索`dpkg -l|grep v2ray`,如果下载了v2raya，则检索结果里会出现。不过不同的软件名可能会与其安装包有所不同。  


### 科学上网
代理是okgg.top,使用v2raya，教程参考https://www.pengtech.net/network/v2rayA_install.html  
新的飞机场地址https://cokecloud.net/#/home  
报错处理：
1.`failed to connect: failed to connect: failed to get the version of v2ray-core: cannot find v2ray executable binary`  
现按照教程配置，如果配置完geosite.dat和geoip.dat之后仍然报错，可以尝试重启以下系统  
其中
```
V2RAYA_V2RAY_BIN=/usr/local/v2ray-core/v2ray
V2RAYA_V2RAY_CONFDIR=/usr/local/v2ray-core
```
直接写入到"v2raya"的配置文件，`sudo vim 路径\v2raya`，直接将上述两句复制到文件中，开头不用加”#“，按`esc`退出输入模式，输入`:w`+回车进行保存，输入`:q`+回车退出。    
  
2.`failed to start v2ray-core: not support "system proxy" mode of transparent proxy: does not support to configure system proxy on your OS`  
选择大陆白名单模式，在代理方式上有‘redirect’,'tproxy','system proxy'。当时选择'system proxy'就会出现上面的报错，但是换为'tproxy'就可以使用。  

由于V2RayA导致查找不到相关的库,删除V2Ray源即可
源的查找：`ls /etc/apt/sources.list.d/`
https://bbs.deepin.org/post/266156  



### vscode使用技巧
取消错误显示：  
按下ctrl+shift+P,在搜索栏中输入`error squiggles`,选择错误关闭

### git
Your identification has been saved in /home/index10/.ssh/id_rsa  
Your public key has been saved in /home/index10/.ssh/id_rsa.pub  
指令获取共钥  
`cd /home/index10/.ssh`  
`gedit id_rsa.pub`  

git使用教程  
https://blog.csdn.net/sduati/article/details/120847837  

文件下载  
在目标文件夹下输入`git clone [SSH地址]`  

#### 分支选择
--brach 选择对应ROS版本，否则编译容易报错


### gcc/g++版本升级
https://blog.csdn.net/hzx19901102/article/details/138465656  


### cmake使用方法
在源代码的文件夹下编写CMakeLists.txt文件，内容如下
```
cmake_minimum_required(VERSION 3.0.2)

# 声明工程名
project( XXXX )

# 添加可执行程序（生成的程序名 源代码文件名）
add_executable( run run.cpp)

# 创建库(库的名称 共享库 库的源文件)
add_library( lib_a SHARED function.cpp)

# 链接库，可执行程序才能使用库(可执行程序名 库的名称)
target_link_libraries( run lib_a )

# 添加查询路径
include_directories(/path)
```

终端指令
```
mkdir bulid  # 生成过程文件夹，防止与源代码混合在一个文件夹里，生成的可执行文件在build中
cd build
cmake ..  # cmake编译上一层文件夹中的内容
make
./可执行文件名  # 运行可执行文件 ./的意思是当前文件夹下,中间不要加空格
```

###### 安装指定cmake版本
https://blog.csdn.net/qq_42264030/article/details/128142926


#### Cmake指令解析
CMake中的**指令**不区分大小写  
`${}` 表示取出变量中的值  

### 依赖库安装
#### eigen
安装eigen版本  
https://blog.csdn.net/hltt3838/article/details/139244605  
eigen软连接  
https://blog.csdn.net/qq_57061492/article/details/126163112  

#### Pangolin
github下载，要先安装libpoxy  
`sudo apt-get install libepoxy-dev`  
参考教程：  
https://blog.csdn.net/Callme_TeacherPi/article/details/125066039  

#### sophus
https://blog.csdn.net/qq_37648371/article/details/134765488
报错1：版本不对——下载最新版本（参考前面的文章）  
报错2：安装最新版本后，sophus找不到eigen——建立软连接  
报错3：找不到sophus，在CMakeLists中添加`include_directories( "/home/index10/slam/lib/Sophus" )`,即添加文件路径  
报错4：找得到文件，但是sophus编译报错——因为最新的库使用了c++17，因此在检查编译器gcc/g++支持c++17后，在CMakeLists中修改与添加  
```
cmake_minimum_required(VERSION 3.8)
set(CMAKE_CXX_STANDARD 17)
```

#### openCV
https://blog.csdn.net/whitephantom1/article/details/136406214  

宏定义错误问题  
https://blog.csdn.net/qq_44164791/article/details/131210608  
https://blog.csdn.net/weixin_45807759/article/details/129099042  

#### g2o
命名空间 "g2o" 没有成员 "make_unique"  
https://blog.csdn.net/chaisxp/article/details/137077554  
报错： `undefined reference to g2o::csparse::CSparse::~CSparse()`  
在链接库里添加相关项，此处添加`g2o_csparse_extensio g2o_solver_csparse`  

下载好g2o之后发现没有g2o_viewer，原因是g2o_viewer依赖库没装全，解决方法如下，添加依赖库后重新编译安装即可  
https://blog.csdn.net/m0_60346726/article/details/128017257  

### DBoW3
在网址安装，进行常规的cmake安装即可，只是要求opencv版本为3  

### pcl
https://blog.csdn.net/weixin_40653140/article/details/126282010  
https://blog.csdn.net/weixin_43563233/article/details/113096791  
建议直接命令行安装PCL，需要安装VTK,建议每个教程都看下    

`#include <pcl/visualization/pcl_visualizer.h>  // 报错fatal error: vtkSmartPointer.h`:在终端查询vtkSmartPointer.h的位置，`locate vtkSmartPointer.h`,依据结果，在vscode 的路径中添加`/usr/include/vtk-7.1`  

### qmake版本更换  
https://blog.csdn.net/qq_29797957/article/details/103896164  

### 关于编译器找不到库
能编译但头文件有红波浪:  
method1:  
在vscode中按下ctrl+shift+p,选择C/C++: 编辑配置(UI)，将文件路径添加到“包含路径”中  
案例：opencv  
https://blog.csdn.net/jianzhuozhu/article/details/109586234  

method2：
将头文件添加上级目录  
案例：`<Eigen/core.hpp>` -> `<eigen3/Eigen/core.hpp>`  

头文件有红波浪且不能编译：  
大概率原因是CmakeLists文件没配置好，没有找到库的正确位置  


CMake Error: The following variables are used in this project, but they are set to NOTFOUND.
Please set them or make sure they are set and tested correctly in the CMake files:
DMVIO_LIBRARY
    linked by target "dmvio_ros_node" in directory /home/index10/dm-vio/caktin_ws/src/dm-vio-ros

-- Generating done
CMake Generate step failed.  Build files cannot be regenerated correctly.
make: *** [Makefile:996：cmake_check_build_system] 错误 1
Invoking "make cmake_check_build_system" failed

安装dm-vio-ros找不到库，是因为没有给库（dm-vio编译产生的库）添加路径。添加路径的方法有两种，一个是在~./bashrc中添加环境变量，给变量赋值，另一个是在dm-vio-ros的cmaklist文件中直接修改$EVN{DMVIO_BUILD}为对应的路径/home/index10/dm-vio/build。  
另：一定要看注释！！！！！！



```C++
// X = scale * (X - median)  此归一化的数学原理？
for (int i = 0; i < num_points_; ++i) {
    VectorRef point(points + 3 * i, 3);
    point = scale * (point - median);
}

```

损失函数(Loss function)：计算的是一个样本的误差。  
代价函数(Cost function)：是整个训练集上所有样本误差的平均。  

`assert(条件句)`:若为false，则终止并报错
`new int(100)`:分配这种类型的**一个**大小的内存空间,并以括号中的值来初始化这个变量;  
`new int[100]`:分配这种类型的**n个**大小的内存空间,并用默认构造函数来初始化这些变量;   
`AngleAxisToQuaternion(const T *angle_axis, T *quaternion)`：将三维角度位姿转化为四元数，输入的是两者的首地址  



## ORB-SLAM3相关
#### 十四讲代码解析
https://blog.csdn.net/weixin_62952541/article/details/134285612  

#### 相机标定
https://blog.csdn.net/qinqinxiansheng/article/details/108629530
https://blog.csdn.net/oakchina/article/details/126342370
https://blog.csdn.net/GuanLingde/article/details/129415831  
https://blog.csdn.net/catpico/article/details/120688795  

#### yaml文件参数解析
https://blog.csdn.net/weixin_43846627/article/details/116712431
https://blog.csdn.net/catpico/article/details/120688795

#### depth accuracy
https://docs.luxonis.com/hardware/platform/depth/depth-accuracy/
#### 红外对其
https://docs.luxonis.com/software/depthai/examples/rgb_depth_aligned/


#### 代码解读
https://zhaoxuhui.top/blog/2021/11/11/imu-optimization-and-recently-lost-state-in-orb-slam3.html
http://zhaoxuhui.top/tags/#SLAM





## Ubuntu界面美化
修改登录界面背景
https://blog.csdn.net/qq_43332081/article/details/129932775
终端
https://blog.csdn.net/weixin_54241007/article/details/129206866


https://docs.luxonis.com/software/ros/depthai-ros/build/
卸载钉钉
https://blog.csdn.net/YezhanCHN/article/details/126860744




glog
https://blog.csdn.net/Cv_Ys/article/details/127327904
absl
https://blog.csdn.net/k1419197516/article/details/130118003
benchmark
https://blog.csdn.net/L_jessica7227/article/details/120327143

