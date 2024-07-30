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

#### g2o
命名空间 "g2o" 没有成员 "make_unique"  
https://blog.csdn.net/chaisxp/article/details/137077554  

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