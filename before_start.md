### 终端基本指令
`ls`：返回当前文件夹下的内容  
`mkdir name0`:新建一个为"name0"的文件夹  
`cd dir_name`:进入"dir_name"文件夹  
`cd ..`:回到上层目录  
`cd ~`:回到主文件夹
`cd /bin`:来到根目录下的某个文件夹  
`gedit filename`:在当前目录下 创建/编辑 一个文件  
`source filename`:执行制定文件中的命令  

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

###### 文件夹删除
`rm -r 文件夹路径`

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


https://blog.csdn.net/qq_35649686/article/details/117414203  
https://bbs.deepin.org/post/266156  


安装lxd,一种类似虚拟机性质的容器(好像要在有线网络下下载)  
https://cn.linux-console.net/?p=29763  

SLAM3 + ROS + OAK-D  
https://qiita.com/nindanaoto/items/20858eca08aad90b5bab


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

qmake版本更换  
https://blog.csdn.net/qq_29797957/article/details/103896164  


https://blog.csdn.net/weixin_62952541/article/details/134285612  



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

相机标定
https://blog.csdn.net/qinqinxiansheng/article/details/108629530
https://blog.csdn.net/oakchina/article/details/126342370

yaml文件参数解析
https://blog.csdn.net/weixin_43846627/article/details/116712431
https://blog.csdn.net/catpico/article/details/120688795


修改登录界面背景
https://blog.csdn.net/qq_43332081/article/details/129932775
终端
https://blog.csdn.net/weixin_54241007/article/details/129206866
--brach 选择对应ROS版本，否则编译容易报错
https://docs.luxonis.com/software/ros/depthai-ros/build/
卸载钉钉
https://blog.csdn.net/YezhanCHN/article/details/126860744


depth accuracy
https://docs.luxonis.com/hardware/platform/depth/depth-accuracy/
红外对其
https://docs.luxonis.com/software/depthai/examples/rgb_depth_aligned/



parafrom python
Left mono camera focal length in pixels: [800.4771728515625, 0.0, 621.0770263671875]
Right mono camera focal length in pixels: [808.477294921875, 0.0, 657.5164794921875]
RGB Camera Default intrinsics...
[[3114.652587890625, 0.0, 1976.50537109375], [0.0, 3118.354736328125, 1082.0452880859375], [0.0, 0.0, 1.0]]
3840
2160
RGB Camera Default intrinsics...
[[3114.652587890625, 0.0, 1976.50537109375], [0.0, 3118.354736328125, 1082.0452880859375], [0.0, 0.0, 1.0]]
3840
2160
RGB Camera resized intrinsics... 3840 x 2160 
[[3.11465259e+03 0.00000000e+00 1.97650537e+03]
 [0.00000000e+00 3.11835474e+03 1.08204529e+03]
 [0.00000000e+00 0.00000000e+00 1.00000000e+00]]
RGB Camera resized intrinsics... 4056 x 3040 
[[3.28985181e+03 0.00000000e+00 2.08768384e+03]
 [0.00000000e+00 3.29376221e+03 1.52216028e+03]
 [0.00000000e+00 0.00000000e+00 1.00000000e+00]]
LEFT Camera Default intrinsics...
[[800.4771728515625, 0.0, 621.0770263671875], [0.0, 801.2078857421875, 402.0769958496094], [0.0, 0.0, 1.0]]
1280
800
LEFT Camera resized intrinsics...  1280 x 720
[[800.47717285   0.         621.07702637]
 [  0.         801.20788574 362.07699585]
 [  0.           0.           1.        ]]
RIGHT Camera resized intrinsics... 1280 x 720
[[808.47729492   0.         657.51647949]
 [  0.         809.19854736 369.82177734]
 [  0.           0.           1.        ]]
LEFT Distortion Coefficients...
k1: -8.615811347961426
k2: 63.073707580566406
p1: 0.0003414236707612872
p2: 0.0009171671117655933
k3: -110.84514617919922
k4: -8.707328796386719
k5: 63.390594482421875
k6: -111.10065460205078
s1: 0.0
s2: 0.0
s3: 0.0
s4: 0.0
τx: 0.0
τy: 0.0
RIGHT Distortion Coefficients...
k1: -0.7525549530982971
k2: 15.032126426696777
p1: -0.0013255453668534756
p2: -0.0005480577819980681
k3: 216.9330291748047
k4: -0.8765493035316467
k5: 14.87918758392334
k6: 217.2581329345703
s1: 0.0
s2: 0.0
s3: 0.0
s4: 0.0
τx: 0.0
τy: 0.0
RGB FOV 68.7938003540039, Mono FOV 71.86000061035156
LEFT Camera stereo rectification matrix...
[[ 1.02595896e+00  3.90409423e-03  2.86255521e+00]
 [ 4.71203718e-03  1.00969170e+00  1.78316074e+00]
 [ 2.45925316e-05 -7.35147312e-07  9.84798370e-01]]
RIGHT Camera stereo rectification matrix...
[[ 1.01580680e+00  3.86554213e-03 -2.78635898e+01]
 [ 4.66541018e-03  9.99721215e-01 -2.49042317e+00]
 [ 2.43491812e-05 -7.27887891e-07  9.84065247e-01]]
Transformation matrix of where left Camera is W.R.T right Camera's optical center
[[ 9.99856889e-01 -7.03364611e-03 -1.53849907e-02 -7.43088007e+00]
 [ 7.05150235e-03  9.99974549e-01  1.10666663e-03 -8.45413730e-02]
 [ 1.53768146e-02 -1.21499551e-03  9.99881029e-01  3.20463181e-02]
 [ 0.00000000e+00  0.00000000e+00  0.00000000e+00  1.00000000e+00]]
Transformation matrix of where left Camera is W.R.T RGB Camera's optical center
[[ 9.99828756e-01  2.39309375e-04 -1.85042676e-02 -3.69976473e+00]
 [-4.43344907e-05  9.99944508e-01  1.05363913e-02 -1.75123848e-02]
 [ 1.85057614e-02 -1.05337659e-02  9.99773264e-01 -4.70057100e-01]
 [ 0.00000000e+00  0.00000000e+00  0.00000000e+00  1.00000000e+00]]


->可以访问成员和函数，  .只能访问成员
glog
https://blog.csdn.net/Cv_Ys/article/details/127327904
absl
https://blog.csdn.net/k1419197516/article/details/130118003
benchmark
https://blog.csdn.net/L_jessica7227/article/details/120327143

