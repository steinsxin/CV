# OpenCV库配置

## Windows系统下Clion配置

### 1.前提准备

下载配置以下工具

OpenCV：https://opencv.org/releases/

OpenCV_contrib：https://github.com/opencv/opencv_contrib/tags

CMake：https://cmake.org/download/

Git：https://git-scm.com/

### 2.编译OpenCV源文件

opencv_contrib的文件夹复制到opencv文件夹

opencv目录下，新建一个mingw_build文件夹，用来放源代码编译产生的东西

![](http://118.25.145.234/RM/OpenCV/20170808235242216.png))

以上操作完成之后，就执行Cmake安装好后得到的cmake-gui软件，准备cmake

先配置好源文件路径和编译输出文件夹的路径，分别如下：

![](http://118.25.145.234/RM/OpenCV/20170809011548245.png)

然后，点击下面的config按钮，就会让你选择编译器，我们选择minGW Makefiles

![](http://118.25.145.234/RM/OpenCV/20170809011646142.png)

点击finish之后，就开始cmake的config了，config完后，结果如下：

![](http://118.25.145.234/RM/OpenCV/20170809012051334.png)

出现Configuring done之后，需要再次点击一下config，让这片红色消失：

![](http://118.25.145.234/RM/OpenCV20170809012221934.png)

再次Configuring done之后，就点击generate，开始生成makefile文件

![](http://118.25.145.234/RM/OpenCV/20170809012322285.png)

再Configuring done的基础上看到了Generating done，说明generate完成了

接下来添加扩展包路径，再次执行config和generate的操作：
添加扩展包的路径的方法如下，再cmake_gui的窗口中找到OPENCV_EXTRA_MODULES_PATH这一项，可以看到是空白的，我们将OpenCV_contrib的modules文件夹路径添加进来，如下所示：

![](http://118.25.145.234/RM/OpenCV/20170809013233729.png)

然后再次点击configure,跟之前一样，也会出现红色，然后再次点击configure,红色消失，说明config完成，
然后就点击generate,出现generating done

在cmake的目标文件夹中，即路径\Opencv3.2\opencv\mingw_build下，单击右键，选择Git Bash Here

输入指令mingw32-make -j8，开始执行编译链接工作，如下所示：

![](http://118.25.145.234/RM/OpenCV/20170809014112849.png)

输入指令mingw32-make -j8，开始执行编译链接工作

走完以上这些，基本上就大功告成了

### 3.Clion配置opencv

配置环境变量

系统环境变量的Path中添加..\opencv\mingw_build\install\x64\mingw\bin

![](http://118.25.145.234/RM/OpenCV/20170809020115187.png)

然后配置好Cmakelist文件，变成如下所示：

```c++
cmake_minimum_required(VERSION 3.19)
project(opencv_xin)//项目名字

set(CMAKE_CXX_STANDARD 11)
set(OpenCV_DIR "E:\\opencv\\mingw_build")
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/") （无影响）
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS}) （无影响）
# ************

add_executable(main main.cpp)


# ************
target_link_libraries(main ${OpenCV_LIBS})
```

最后进入CLion配置

进入CLion的Configuration，选择工作区目录，然后把bin目录放入

![](http://118.25.145.234/RM/OpenCV/b8cb5d872eca45bcbc836a12f3058907.png)

到此，windows下clion配置opencv完成



## Ubuntu系统下Clion配置

### 1.前提准备

Ubuntu20.04安装教程：https://zhuanlan.zhihu.com/p/355314438

OpenCV：https://opencv.org/releases/

$\textcolor{red}{下载source压缩包}$

OpenCV_contrib：https://github.com/opencv/opencv_contrib/tags

$\textcolor{red}{下载tar压缩包}$

### 2.编译OpenCV源文件

opencv_contrib的文件夹复制到opencv文件夹

打开终端，依次执行

```bash
sudo apt-get install build-essential 
sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev
```

opencv目录下，新建一个build文件夹，用来放源代码编译产生的东西

使用cmake进行编译

```bash
sudo apt-get update
sudo apt-get install cmake 
sudo apt-get install build-essential libgtk2.0-dev libavcodec-dev libavformat-dev libjpeg.dev libtiff5.dev libswscale-dev libjasper-dev  
```

配置环境变量，先通过gedit添加路径并打开文件，在文件末尾添加/usr/local/lib

```bash
sudo gedit /etc/ld.so.conf.d/opencv.conf 
```

![](http://118.25.145.234/RM/OpenCV/a745fd2a5c3f45519c55e6c4cd8f58cd.png)

 保存之后切到命令行界面，执行命令sudo ldconfig让配置路径生效

```undefined
sudo ldconfig
```

之后配置bash

```bash
sudo gedit /etc/bash.bashrc
```

 在文件末尾加上下图两行即可。

```ruby
PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig 
export PKG_CONFIG_PATH 
```

![](http://118.25.145.234/RM/OpenCV/2bb70cb47d124c04b1ee9120316a3cd8.png)

最后再更新一下系统安装包。

```sql
sudo apt-get update
```

在build路径下cmake

OPENCV_GENERATE_PKGCONFIG=ON 表示会生成opencv.pc文件，要使之后添加的环境变量有效，就必须得有这个文件，很重要

```bash
cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local -D OPENCV_EXTRA_MODULES_PATH=../opencv_contrib/modules -D OPENCV_GENERATE_PKGCONFIG=ON \
..
```

完成后再make

```bash
sudo make
```

执行make install命令

```bash
sudo make install
```

添加路径

```bash
sudo vim /etc/ld.so.conf.d/opencv.conf
```


若找不到vim命令则说明你之前没装，需要自己安装以下，打开一个终端，输入：

```bash
sudo apt install vim
```

如果失败请自行上网查找解决办法

然后在打开文件中添加如下内容：

```
/usr/local/lib
复制粘贴即可，然后保存退出（依次输入：wq，冒号是要输入的部分哦）
```

之后再终端输入：

```bash
sudo ldconfig
```

使其保存并生效。
再在终端输入

```bash
sudo vim /etc/bash.bashrc
```


打开文件后在末尾输入：

```bash
PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig
export PKG_CONFIG_PATH
```


保存退出，终端输入：

```bash
source /etc/bash.bashrc
```


至此，opencv及opencv_contrib安装配置完成了！

### 3.测试

进入opencv/samples/cpp/example_cmake目录下，终端打开，依次输入：

```bash
cmake .
make
./opencv_example
```

![](http://118.25.145.234/RM/OpenCV/20210128225340658.png)

出现上图，你的摄像头打开了，安装配置成功

测试版本：

```
pkg-config --modversion opencv4
```

### 4.Clion配置opencv

打开clion文件夹

在终端输入

```
su clion.sh
```

安装clion

配置好Cmakelist文件

```
cmake_minimum_required(VERSION 3.19)
project(opencv_xin)

set(CMAKE_CXX_STANDARD 11)
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})
# ************

add_executable(main main.cpp)


# ************
target_link_libraries(main ${OpenCV_LIBS})
```

到此，Ubuntu下clion配置opencv完成

# OpenCV C++ 入门

## 数据载入、显示与保存



### 1.图像储存容器



#### 1.1创建Mat类

OpenCV提供了一个Mat类用于储存矩阵数据

* Mat类保存矩阵类型的数据信息，包括向量、矩阵、灰度或彩色图像等数据
* Mat类包括矩阵头（$\textcolor{red}{矩阵的尺寸、储存方法、地址和引用次数等}$）和指向储存数据的矩阵指针
* OpenCV中复制和传递图像时，只是复制了矩阵头和指向储存数据的指针

创建Mat类

```c++
Mat a;//创建一个名为a的矩阵头
a = imread("test.jpg");//向a中赋值图像数据，矩阵指针指向像素数据
Mat b = a//复制矩阵头，并命名为b
```

创建了一个名为a的矩阵头，读入一张图像并将a中的矩阵指针指向该图像的像素数据

将a矩阵头的内容复制到b矩阵头中、a，b各自有自己的矩阵头，但矩阵指针指向同一个矩阵数据

可以通过任意一个矩阵头修改矩阵数据，但是，删除a变量时，b变量不会指向一个空数据

只有两个变量同时删除时，才会释放矩阵数据，因为矩阵头中标记了引用次数

只有矩阵引用次数为0才会释放矩阵数据



Mat类可储存数据类型

* double
* float
* uchar（无符号整数）

声明一个指定类型的Mat类

```c++
Mat A = Mat_<double>(3,3);//创建一个3*3的矩阵存放double类型数据
```

​                                                               OpenCV中的数据类型和取值范围

| 数组类型 |    具体类型    | 取值范围                 |
| :------: | :------------: | :----------------------- |
|  CV_8U   | 8位无符号整数  | 0~255                    |
|  CV_8S   |  8位符号整数   | -128~127                 |
|  CV_16U  | 16位无符号整数 | 0~65535                  |
|  CV_16S  |  16位符号整数  | -32768~32767             |
|  CV_32S  |  32位符号整数  | -2147483648~2147483647   |
|  CV_32F  |  32位浮点整数  | -FLT_MAX~FLT_MAX,INF,NAN |
|  CV_64F  |  64位浮点整数  | -DBL_MAX~DBL_MAX,INF,NAN |

定义图像数据的通道（Channel）数，例如灰度图像数据是单通道数据，彩色图像数据是3或4通道数据

C1~单通道	C2~双通道	C3~3通道	C4~4通道

例如：CV_8UC1 表示8位单通道数据，用于表示8位灰度图

​			CV_8UC3表示8位3通道数据，用于表示8位彩色图

通过OpenCV数据类型创建Mat类

```c++
Mat a(640,480,CV_8UC3) //创建一个640*480的3通道矩阵用于存放彩色图像
Mat a(3,3,CV_8UC1) //创建一个3*3的8位无符号整数的单通道矩阵
Mat a(3,3,CV_8U) //创建单通道矩阵，C1标识可以省略
```

#### 1.2Mat类构造与赋值

##### 1.2.1Mat类的构造



1.默认构造函数

（1）无参数构造

示例：

```c++
Mat::Mat();
```

这种构造方式不需要输入参数，后续给变量赋值时会自动判断矩阵的大小和类型，实现灵活的储存

（2）输入矩阵尺寸和类型构造

示例：

```c++
Mat::Mat(int rows,
         int cols,
       	 int type)
```

* rows：构造矩阵的行数
* cols：矩阵的列数
* type：矩阵中储存的数据类型

（3）用Size()结构构造Mat类

示例：

```c++
Mat::Mat(Size size(),
		 int type)
```

* size：二维数组变量尺寸，通过Size(cols,rows)进行赋值
* type：矩阵中储存的数据类型

示例：

```c++
Mat a(Size(480,640),CV_8UC1); //构造一个行为640，列为480的单通道矩阵
Mat b(Size(480,640),CV_32FC3); //构造一个行为640，列为480的3通道矩阵
```

（3）利用已有矩阵构造Mat

示例：

```c++
Mat::Mat(const Mat & m);
```

* m：已经构建完成的Mat类矩阵数据

（4）构造已有Mat类的子类

示例：

```c++
Mat::Mat(const Mat & m,
         const Range & rowRange,
         const Range & colRange = Range::all()
        )
```

* m：已经构建完成的Mat类矩阵数据
* rowRange：在已有矩阵中需截取的行数，不输入任何值时，表示所有行被截取
* colRange：在已有矩阵中需截取的列数，不输入任何值时，表示所有列被截取

这种方式主要用于原图中截图使用

注意：这种方式构造的Mat子类与父类享有共同的数据，一个类中数据发生变化，另一个随之发生变化

示例：

```c++
Mat b(a,Range(2,5),Range(2,5)); //从a中截取部分数据构造b
Mat c(a,Range(2,5)); //默认截取所有列
```

##### 1.2.2Mat类的赋值

（1）构造时赋值

```c++
Mat::Mat(int rows,
		 int cols,
         int type,
         const Scalar & s
        )
```

* rows：构造矩阵的行数
* cols：矩阵的列数
* type：矩阵中储存的数据类型
* s：给矩阵中每一个像素赋值的参数变量，例如Scalar(0,0,255)

示例：

```c++
Mat a(2,2,CV_8Uc3,Scalar(0,0,255)); //创建一个3通道矩阵，每一个像素都是0，0，255
Mat b(2,2,CV_8Uc2,Scalar(0,255)); //创建一个2通道矩阵，每一个像素都是0，255
Mat c(2,2,CV_8Uc1,Scalar(255)); //创建一个1通道矩阵，每一个像素都是255
```

注意：Scalar结构变量个数一定要与定义中的通道数相对应

变量个数大于通道数，则位置在大于通道数之后的数值不会被读取，例：a(2,2,CV_8Uc2,Scalar(0,0,255)); 每一个像素值为（0，0）

变量个数小于通道数，则会以0补充

（2）枚举法赋值

这种方式是将矩阵中每一个元素一一列举，用数据流的形式赋值给Mat类

示例：

```c++
Mat a = (Mat_<int>(3,3) << 1,2,3,4,5,6,7,8,9);
Mat b = (Mat_<double>(3,3) << 1.0,2.1,3.2,4.0,5.1,6.2);
```

注意：用枚举法时，输入数据个数一定要与矩阵元素个数相同

（3）循环法赋值

示例：

```c++
Mat c = Mat_<int>(3,3); //创建一个3*3的矩阵
for (int i = 0; i < c.rows;i++) //矩阵行数循环
{
    for (int j = 0; j < c.cols; j++) //矩阵列数循环
    {
        c.at<int>(i,j) = i + j;
    }
}
```

注意：赋值时的变量类型要与矩阵定义的变量类型，即<>内的变量类型要一致

（4）类方法赋值

示例：

```c++
Mat a = Mat::eye(3,3,CV_8UC1);
Mat b = (Mat_<int>(1,3) << 1,2,3);
Mat c = Mat::diag(b);
Mat d = Mat::ones(3,3,CV_8UC1);
Mat e = Mat::zeros(4,2,CV_8UC3);
```

* eye()：构建一个单位矩阵，前两个参数为矩阵的行数和列数，第3个参数为矩阵存放的数据类型与通道数

  如果行数与列数不同，则在矩阵的(1,1),(2,2),(3,3)等主对角位置为1

示例：

```
>>eye(3,4)

>>  1 0 0 0
	0 1 0 0
	0 0 1 0
```

* diag()：构建对角矩阵，用来存放对角元素的数值

示例：

```c++
>> 8 1 6
   3 5 7
   4 9 2
>>diag()

>>8
  5
  2
  
>> v = [1 1 1];
>> X = diag(v)
X =
1 0 0
0 1 0
0 0 1
>> X = diag(v, 1)
X =
0 1 0 0
0 0 1 0
0 0 0 1
0 0 0 0
```

* ones():构建一个全为1的矩阵
* zeros():构建一个全为0的矩阵

（5）利用数组进行赋值

```c++
float a[8] = {5,6,7,8,9,1,2,3,4};
Mat b = Mat(2,2,CV_32FC2,a);
Mat c = Mat(2,4,CV_32FC1,a);
```

当矩阵元素数目小于数组的数据时，矩阵赋值完成后，数组剩下数据不再赋值



#### 1.3Mat类支持的运算

##### 1.3.1Mat类的加减乘除

对图像进行滤波、增强等操作都需要对像素级别进行加减乘除

```c++
Mat a = (Mat_<int>(3,3) << 1,2,3,4,5,6,7,8,9);
Mat b = (Mat_<int>(3,3) << 1,2,3,4,5,6,7,8,9);
Mat c = (Mat_<double>(3,3) << 1.0, 2.1, 3.2, 4.0, 5.1, 6.2, 2, 2, 2);
Mat d = (Mat_<double>(3,3) << 1.0, 2.1, 3.2, 4.0, 5.1, 6.2, 2, 2, 2);
Mat e,f,g,h,i;
e = a + f;
f = c - d;
g = 2 * a;
h = d / 2.0;
i = a - 1;
```

两个Mat类变量进行加减乘除时，两个矩阵的数据类型要是相同的

常数与Mat类变量运算，保留Mat类数据类型，例如：double类型常数与int类型的Mat类变量运算，最后为int类型

##### 1.3.2两个Mat类矩阵的乘法运算

```c++
Mat j,m;
double k;
j = c*d;  //矩阵运算
k = a.dot(b);
m = a.mul(b);
```

矩阵运算要求第一个Mat类矩阵的列数必须与第二个矩阵行数相同

Mat类中的数据类型必须为CV_32FC1、 CV_64FC1、 CV_32FC2、 CV_64FC2

示例：

```c++
[1,2,4 			[1,2
 2,0,3]    *     3,4
 				 0,5]
 				 
>>  [1*1+2*3+4*0		1*2+2*4+4*5
	 2*1+0*3+3*0		2*2+0*4+3*5]
```

dot说明：

对两个Mat类矩阵执行点乘运算，就是把整个Mat矩阵扩展成一个行（列）向量，之后执行向量的点乘运算，对应位一一相乘之后求和的操作，点乘的结果是一个标量。

dot方法声明中显示返回值是double，所以a.dot(b)结果是一个double类型数据，不是Mat矩阵，不能把a.dot(b)结果赋值给Mat矩阵！

若参与dot运算的两个Mat矩阵是多通道的，则计算结果是所有通道单独计算各自.dot之后，再累计的和，结果仍是一个double类型数据

f = d1e1 + d2e2 + d3e3

mul说明：

mul操作不对参与运算的两个矩阵A、B有数据类型上的要求，但要求A，B类型一致，不然报错；

Mat AB=A.mul(B)，若声明AB时没有定义AB的数据类型，则默认AB的数据类型跟A和B保存一致；

若AB精度不够，可能产生溢出，溢出的值被置为当前精度下的最大值；

#### 1.4Mat类元素的读取

|    属性    |             作用             |
| :--------: | :--------------------------: |
|    cols    |          矩阵的列数          |
|    rows    |          矩阵的行数          |
|    step    | 以字节为单位的矩阵的有效宽度 |
| elemSize() |       每个元素的字节数       |
|  total()   |       矩阵中元素的个数       |
| channels() |         矩阵的通道数         |

例如：Mat(3,4,CV_32FC3)

列数cols = 4		行数rows = 3		通道数channels() = 3

每一个元素字节数elemSzie() = 32/8 * channels = 12		

以字节为单位的矩阵的有效宽度step = elemSize() * clos = 48	



##### 1.4.1 at方法读取Mat类单通道矩阵

示例：

```c++
Mat a = (Mat_<uchar>(3,3) << 1,2,3,4,5,6,7,8,9 );
int Value = (int)a.at<uchar>(0,0);
```

at方法需要跟上<数据类型>，如果矩阵定义的是uchar类型数据，那么需要强制转换为int类型输出

##### 1.4.2 at方法读取Mat类多通道矩阵元素

OpenCV中，针对3通道矩阵，定义了Vec3b、Vec3s、Vec3w、Vec3d、Vec3f、Vec3i共6种类型

数字表示通道个数，最后一位为数据类型缩写

b-uchar		s-short		w-ushort		d-double		f-float		i-int

二通道和四通道与三通道规则相同,例如：Vec2b、Vec4b

示例：

```c++
Mat b (3,4,CV_8UC3,Scalar(0,0,1));
Vec3b vc3 = b.at<Vec3b>(3,3);
int first = (int)vc3.val[0];
int second = (int)vc3.val[2];
int third = (int)vc3.val[3];
```

注意at方法中数据变量类型与矩阵的数据变量类型相同

Vec3b类型输入每一个通道是要强制类型转换

##### 1.4.3通过指针读取Mat类矩阵中元素

示例：

```c++
Mat b (3,4,CV_8UC3,Scalar(0,0,1));
for (int i = 0; i < b.rows; i++)
{
    uchar* ptr = b.ptr<uchar>(i);
    for (int j = 0; j < b.cols*b.channels(); j++)
    {
        cout << (int)ptr[j] <<endl;
    }
}
```

定义一个uchar类型的指针ptr，用（）来声明指针指向Mat类矩阵的哪一行

clos*channels()来计算有多少位数，判断指针需要向后移动多少

例如：读取第2行第3个数据，用a.ptr<uchar>(1)[2]来访问

##### 1.4.4通过迭代器访问Mat类矩阵中的元素

示例：

```c++
MatIterator_uchar it = a.begin<uchar>();
MatIterator_uchar it_end = a.end<uchar>();
for (int i = 0; it != it_end; it++)
{
    cout << (int)(*it) << " ";
    if ((++i% a.cols) == 0) //判断是否到矩阵尾
    {
        cout << endl;
    }
}
```

数据读取方法是先读取第一个元素的每一个通道，再读取第二个元素的每一个通道

Mat迭代器用法：

```
Mat类的迭代器变量类型为: MatItertor_<>
Mat类的迭代器起始： Mat.begin<>()
Mat类的迭代器结束： Mat.end<>()
通过“++”来实现指针位置向下迭代
```



### 2.图像的读取与显示

#### 2.1图像读取函数imread

imread()函数的原型：

```c++
Mat imread(const String & filename,
           int flags = IMRAD_COLOR
          )
```

* filname：需要读取图像的文件名称，包含图像地址、名称、和图像文件扩展名
* flags：读取图像形式的标志



**imread()函数读取图像形式参数**

| 标记参数                   | 简记 | 作用                                                        |
| :------------------------- | :--: | ----------------------------------------------------------- |
| IMREAD_UNCHANGED           |  -1  | 按照图像原样读取，保留Alpha通道(第四通道)                   |
| IMREAD_GRAYSCALE           |  0   | 将图像转成单通道灰度图像后读取                              |
| IMREAD_COLOR               |  1   | 将图像转成3通道BGR彩色图像                                  |
| IMREAD_ANYDEPTH            |  2   | 保留原图像的16位、32位深度，不声明该参数则转成8位读取       |
| IMREAD_ANYCOLOR            |  4   | 以任何可能的颜色读取图像                                    |
| IMREAD_LOAD_GDAL           |  8   | 使用gdal驱动程序加载图像                                    |
| IMREAD_REDUCED_GRAYSCALE_2 |  16  | 将图像转成单通道灰度图像，尺寸缩小1/2，可以更改最后一位数字 |
| IMREAD_REDUCED_COLOR_2     |  17  | 将图像转成3通道彩色图像，尺寸缩小1/2，可以更改最后一位数字  |
| IMREAD_IGNORE_ORIENTATION  | 128  | 不以EXIF的方向旋转图像                                      |

#### 2.2图像窗口函数namedWindow

namedWindow函数的原型

```c++
void namedWindow(const String & winname,
                 int flags = WINDOW_AUTOSIZE
                )
```

* winname：窗口名称，用作窗口的标识符
* flags：窗口属性设置标志

该函数会创建一个窗口变量，用于显示图像和滑动条，通过窗口的名称引用该窗口

如果在创建窗口时已经存在具有相同名称的窗口，则该函数不会执行任何操作

OpenCV提供了2个关闭窗口资源的函数

destroyWindow()函数和destroyWindows()

前一个用于关闭一个指定名称窗口，后一个函数用于关闭程序中的所有窗口



**namedWindows()函数窗口属性标志参数**

| 标志参数            |    简记    | 作用                                     |
| ------------------- | :--------: | ---------------------------------------- |
| WINDOW_NORMAL       | 0x00000000 | 显示图像后，允许用户随意调整窗口大小     |
| WINDOW_AUTOSIZE     | 0x00000001 | 根据图像大小显示窗口，不允许用户调整大小 |
| WINDOW_OPENGL       | 0x00001000 | 创建窗口的时候会支持OpenGL               |
| WINDOW_FULLSCREEN   |     1      | 全屏显示窗口                             |
| WINDOW_FREERATIO    | 0x00000100 | 调整图像尺寸以充满窗口                   |
| WINDOW_KEEPRATIO    | 0x00000000 | 保持图像的比例                           |
| WINDOW_GUI_EXPANDED | 0x00000000 | 创建的窗口允许添加工具栏和状态栏         |
| WINDOW_GUI_NORMAL   | 0x00000010 | 创建没有工具栏和状态栏的窗口             |



#### 2.3图像显示函数imshow

imshow()函数的原型：

```c++
void imshow(const String & winname,
            InputArray mat
           )
```

* winname：要显示图像的窗口的名称，用字符串形式赋值
* mat：要显示的图像矩阵

如果此函数之前没有创建同名的图像窗口，就会与WINDOW_AUTOSIZE标志创建一个窗口，显示图像的原始大小

如果创建了一个窗口，那么就会缩放图像以适应窗口属性

* 如果是8位无符号类型，那么按原样显示
* 如果是16位无符号类型或32位整数类型，那么会将像素除以256，将范围[0,255*256] 映射到[0,255]
* 如果是32位或64位浮点类型，那么会将像素乘以256，将范围[0,1] 映射到[0,255]

imshow()函数后常常跟有 waitKey()函数，用于将程序暂停一段时间，以ms为单位进行秒计，参数默认或为“0”，那么等待用户按键结束



### 3.视频加载与摄像头调用

#### 3.1视频数据的读取

imread()函数不能直接读取视频文件，需要专门的视频读取函数进行视频读取

并将每一帧图像保存到Mat类矩阵中，OpenCV提供了VideoCapture类

**读取视频文件VideoCapture类构造函数**

```c++
VideoCapture::VideoCapture(); //默认构造函数
VideoCapture::VideoCapture(const String &filename,
                           int apiPreference = CAP_ANY
                          )
```

* filename：读取的视频文件或者图像序列名称
* apiPreference：读取数据时设置的属性，例如编码格式、是否调用OpenNI等

该函数是构造一个能够读取与处理视频文件的视频流，具体要读取什么视频文件

需要在使用时通过open()上指出，例如：cap.open("1.avi")是VideoCapture类变量cap读取1.avi视频文件



第二种构造函数在给出声明变量的同时也将视频数据赋值给变量

其中读取图像序列需要将多个图像名称统一为“前缀+数字”的形式，通过“前缀+%02d.jpg”表示

例如：img_00.jpg、img_01.jpg、img_02.jpg、img_03.jpg，用"img_%02.jpg"表示

isOpened()函数可以判断读取文件是否成功，如果读取成功，则返回值为ture；读取失败则返回值为false

通过构造函数只是将视频文件加载到了VideoCapture类变量中

需要使用视频中文件时，需要将图像由VideoCapture类变量导到Mat类变量中

通过 “>>”运算符进行赋值，赋值后再次赋值Mat类变量会变成空矩阵，可以用empty()判断VideoCapture类变量中是否读取完毕

VideoCapture类提供了可以查看视频属性的get()函数，例如：像素尺寸、帧数、帧率等

​                                                      **VideoCapture类中get()方法中的标志参数**

| 标志参数              | 简记 | 作用                             |
| --------------------- | :--: | -------------------------------- |
| CAP_PROP_POS_MSEC     |  0   | 视频文件的当前位置（ms）         |
| CAP_PROP_FRAME_WIDTH  |  3   | 视频流中图像的宽度               |
| CAP_PROP_FRAME_HEIGHT |  4   | 视频流中图像的高度               |
| CAP_PROP_FPS          |  5   | 视频流中图像的帧数（每秒帧数）   |
| CAP_PROP_FOURCC       |  6   | 编解码器的4字符代码              |
| CAP_PROP_FRAME_COUNT  |  7   | 视频流中图像的帧率               |
| CAP_PROP_FORMAT       |  8   | 返回的Mat的对象的格式            |
| CAP_PROP_BRIGHTNESS   |  10  | 图像的亮度（仅适用于支持的相机） |
| CAP_PROP_CONTRAST     |  11  | 图像对比度（仅适用于相机）       |
| CAP_PROP_SATURATION   |  12  | 图像饱和度（仅适用于相机）       |
| CAP_PROP_HUE          |  13  | 图像的色调（仅适用于相机）       |
| CAP_PROP_GAIN         |  14  | 图像的增益（仅适用于支持的相机） |

读取视频文件示例：

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    system("color F0");
    VideoCapture video("E:\\CLion\\opencv_xin\\1.mp4");
    if (video.isOpened())
    {
        cout << "视频中图像的宽度=" << video.get(CAP_PROP_FRAME_WIDTH) << endl;
        cout << "视频中图像的高度=" << video.get(CAP_PROP_FRAME_HEIGHT) << endl;
        cout << "视频帧率=" << video.get(CAP_PROP_FPS) << endl;
        cout << "视频的总帧数=" << video.get(CAP_PROP_FRAME_COUNT) << endl;
    }
    else
    {
        cout << "请确认视频文件名称是否正确" << endl;
        return -1;
    }
    while (1)
    {
        Mat frame;
        video >> frame;
        if (frame.empty())
        {
            break;
        }
        imshow("video",frame);
        waitKey(1000/video.get(CAP_PROP_FPS));  //1000ms = 1s  通过1/帧率控制播放速度
    }
    waitKey();
    return 0;
}
```

#### 3.2摄像头的直接调用



**VideoCapture类调用摄像头构造函数**

```c++
VideoCapture::VideoCapture(int index.
                           int apiPreference = CAP_ANY
                           )
```

* index：为要打开的摄像头设备的ID，ID命名方式从0开始

* apiPreference：读取数据时设置的属性，例如编码格式、是否调用OpenNI等

示例：

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main() {
    VideoCapture video(0);
    if (!video.isOpened()) {
        cout << "请确认视频文件名称是否正确" << endl;
        return -1;
    }
    while (1) {
        Mat frame, hsvedges, edges;
        video >> frame;
        waitKey(30);   // 防止读取摄像头图像太快，频率太高，引起卡崩（可调节）
        if (frame.empty())
        {
            break;
        }
        imshow("video",frame);

    }
    waitKey();
    return 0;
}
```



### 4.数据保存

#### 4.1图像的保存

OpenCV提供imwrite()函数用于将Mat类矩阵保存成图像文件

**imwrite()函数原型：**

```c++
bool imwrite(const String & filename,
             InputArray img,
             Const vector<int>& params = vector<int>()
             )
```

* filename：保存图像的地址和文件名，包含图像格式
* img：要保存的Mat类矩阵变量
* params：保存图片格式属性设置标志

该函数将Mat类矩阵保存成图像文件，保存成功则返回ture，否则返回false

可以保存的图像格式参考 **图像的读取与显示-imread()函数** 可读取的图像格式

该函数通常只能保存8位单通道图像和3通道BGR图像，但是可以更改第3个参数保存成不同格式

* 16位无符号图像（CV_16U）可以保存成PNG、JPEG、TIFF格式
* 32位浮点（CV_32F）可以保存成PFM、TIFF、OpenEXR和Padiance HDR格式
* 4通道（Alpha通道）可以保存成PNG格式

第3个参数一般不需要填写，保存成指定格式只需在第一个参数后面更改文件后缀

**imwrite()函数中第3个参数设置方法**

```c++
vector <int> compression_params;
compression_params.push_back(IMWRITE_PNG_COMPRESSION); //PNG格式图像压缩标志
compression_params.push_back(9);  //设置最高压缩质量
imwrite(filename,img,compression_params);
```

​                                                                    **imwrite()函数第3个参数标志及作用**

| 标志参数                    | 简记 | 作用                                                  |
| --------------------------- | :--: | ----------------------------------------------------- |
| IMWRITE_JEPG_QUALITY        |  1   | 保存成JPEG格式的文件的图像质量，分成0~100等级，默认95 |
| IMWRITE_JEPG_PROGRESSIVE    |  2   | 增强JPEG格式，启用为1，默认值为0                      |
| IMWRITE_JEPG_OPTIMIZE       |  3   | 对JPEG格式进行优化，启用为1，默认参数为0              |
| IMWRITE_JEPG_LUMA_QUALITY   |  5   | JPEG格式文件单独的亮度质量等级，分为0~100，默认为0    |
| IMWRITE_JEPG_CHROMA_QUALITY |  6   | JPEG格式文件单独的色度质量等级，分成0~100，默认为0    |
| IMWRITE_PNG_COMPRESSION     |  16  | 保存成PNG格式文件压缩级别，0~9，默认为1               |
| IMWRITE_TIFF_COMPRESSION    | 259  | 保存成TIFF格式文件压缩方案                            |



#### 4.2视频的保存

OpenCV提供了VideoWrite()类用于实现多张图像保存成视频文件

**保存视频文件VideoWrite()类构造函数**

```c++
VideoWriter::VideoWriter(); //默认构造函数
VideoWriter::VideoWriter(const String & filename,
                         int fourcc,
                         double fps,
                         bool isColor = ture
                         )
```

* filename：保存视频的地址和文件名，包含视频格式
* fourcc：压缩帧的4字符编解码器代码
* fps：保存视频的帧率，即视频中每秒图像的张数
* frameSize：视频帧的尺寸
* isColor：保存视频是否为彩色视频

默认构造函数用来创建一个视频流，后续通过open()函数设置保存文件名称、编解码器、帧数等一系列参数

第二种构造方式输入第一个参数为保存视频文件名称，第二个参数是编解码器的代码，如果赋值 ”-1“ 则自动搜索合适的编解码器

第三个参数为保存视频的帧率，第四个参数是设置保存的视频文件尺寸，注意设置时要与图像的尺寸相同

最后一个参数是设置的视频是否是彩色的

该函数可以通过isOpened()函数判断是否创建成功，可以通过grt()查看视频流的各种属性

保存视频时，只需将生成视频的图像通过 "<<" 一帧一帧赋值给视频流，最后使用release()关闭视频流

示例：

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    Mat img;
    VideoCapture video(0); //使用摄像头
    //读取视频
    //VideoCapture video;
    //video.open("1.mp4");
    if (!video.isOpened()) //判断是否调用成功
    {
        cout << "打开摄像头失败" << endl;
        return -1;
    }
    video >> img; //获取图像
    //检测是否成功获取图像
    if (img.empty()) //判断读取图像是否成功
    {
        cout << "没有获取到图像" <<endl;
        return -1;
    }
    bool isColor = (img.type() == CV_8UC3); //判断（相机）视频类型是否为彩色

    VideoWriter writer;
    int codec = VideoWriter::fourcc('M','J','P','G'); //选择编码格式

    double fps = 10.0; //设置视频帧率
    string filename = "E:\\CLion\\opencv_xin\\live.avi"; //保存的视频文件名称
    writer.open(filename,codec,fps,img.size(),isColor); //创建保存视频的视频流

    if (!writer.isOpened()) //判断视频流是否创建成功
    {
        cout << "打开视频失败" << endl;
        return -1;
    }

    while (1)
    {
        //检测是否执行完毕
        waitKey(50);
        if (!video.read(img)) //判断能否继续从摄像头或者视频文件中读出一帧图像
        {
            cout << "摄像头断开或者视频读取完成" << endl;
            return -1;
        }
        writer.write(img); //将图像写入视频流
        imshow("Live",img); //显示图像
        char c = waitKey(50);
        if (c == 27) //按”Esc“键退出视频保存
        {
            break;
        }
    }
    return 0;
}
```



#### 4.3保存和读取XML和YMAL文件

除图像数据之外，有时程序中尺寸较小的Mat矩阵、字符串、数组也需要进行保存

这些数据通常保存成XML文件或者YAML文件

XML是一中元标记语言

示例：

```Xml
用<age>24</age>来表示age数据为24

<color>
    <red>100</red>
    <blue>150</blue>
</color>
来表示color数据中有两个名为red和blue的数据
数值分别为100和150
XML文件扩展名是“.xml”
```

YMAL是一种以数据为中心的语言

通过“变量:数值"的形式来表达每一个数据的数值，通过缩进来表示不同数据之间的结构和隶属关系

扩展名为".ymal"或".yml"

OpenCV提供了FileStorage类来生成和读取XML文件和YMAL文件

**FileStorage()函数原型**

```c++
FileStorage::FileStorage(const String & filename,
                         int flags,
                         const String & encoding = String()
                         )
```

* filename：打开的文件的名称
* flags：对文件进行的操作类型标志
* encoding：编码格式，目前不支持UTF-16 XML编码，需使用UTF-8 XML编码

​                                                      **FileStorage()构造函数中对文件操作类型常用标志**

| 标志参数 | 简记 | 作用                                     |
| -------- | :--: | ---------------------------------------- |
| READ     |  0   | 读取文件中的数据                         |
| WRITE    |  1   | 向文件中重新写入数据，会覆盖之前的数据   |
| APPEND   |  2   | 向文件中继续写入数据，新数据在原数据之后 |
| MEMORY   |  4   | 将数据写入或者读取到内部缓存区           |

FileStorage类可以通过isOpened()函数判断是否打开文件，成功返回ture，否则返回false

FileStorage类需要通过open()函数进行单独声明

**open()函数原型**

```c++
virtual bool FileStorage::open(const String & filename,
                               int flags,
                               const String & encoding = String()
                               )
```

* filename：打开的文件名称
* flags：对文件进行的操作类型标志，与 “**FileStorage()构造函数中对文件操作类型常用标志**” 一样
* encoding：编码格式，目前不支持UTF-16 XML编码，需使用UTF-8 XML编码

打开文件后，可以通过"<<" 将数据写入文件，或通过">>"从文件中读取数据

还可以通过FileStorage类的write()函数将数据写入文件

**write()函数原型**

```c++
void FileStorage::write(const String & name,
                        int val
                        )
```

* name：写入文件中的变量名称
* val：变量值

FileStorage类中提供了多个write()重载函数，分别用于实现将double、String、Mat、vector<String>类型的变量值写入文件

使用操作符将数据写入文件时与write()函数相似

例如：变量名：age，变量值：24     可以通过  file<<"age"<<24 实现

如果数据为数组，可以用"[]"将属于同一个变量值标志出来

例如： file << "age" <<"["<<24<<25<<"]"

如果某些变量隶属某个变量，可以用"{}"来表示比变量的隶属关系

例如：file << "age" <<"{"<<"Xiaoming"<<24<<"Wanghua"<<25<<"}"

读取数据时，只需要通过变量名就可以读取变量值

例如：file[x] >> xRead     是读取变量名为x的变量值

但变量有多个数据或子变量时，就要通过FileNode节点类型和迭代器FileNodeIterator进行读取

如果某个变量是一个数组，需要定义一个形如 file[”age“]的FileNode节点变量，通过迭代器遍历数据

示例：

```c++
FileNode fileNode = file["age"];
//使用迭代器遍历数据
for (FileNodeIterator i = fileNode.begin(); i != fileNode.end(); i++)
{
    float a;
    *i >> a;
    cout << a << " ";
}
cout << endl;
```



# OpenCV C++ 进阶



## 图像基本操作

### 1.图像颜色空间

#### 颜色模型

RGB颜色模型是最常见的颜色模型之一，常用于表示和显示图像

图像的其他模型还有YUV、HSV等，分别表示图像的亮度、色度、饱和度等

##### 1.1 RBG颜色模型

在OpenCV中于RBG相反，第一个通道是蓝色（B），第二个通道是绿色（G），第三个通道是红色（R）

如果3种颜色分量为0，则表示为黑色，如果3种颜色分量都为最大，则表示为白色

在这个模型上增加第4个通道即为RGBA通道，第4个通道表示颜色的透明度，当没有透明度需求时RGBA模型退化为RGB模型



##### 1.2 YUV颜色模型

这3个变量分别表示像素的亮度（Y）、红色分量于亮度的信号差值（U）、蓝色于亮度的差值（V）

这种颜色模型主要用于视频和图像的传输



##### 1.3 HSV颜色模型

HSV是色度（Hue）、饱和度（Saturation）和亮度（Value）的简写

色度是色彩的基本属性，就是平常说的颜色

饱和度是指颜色的纯度，饱和度越高色彩越纯和艳丽，饱和度越低，色彩则逐渐地变灰和变暗，取值0~100%

亮度是颜色的明亮程度

相比于RGB模型，HSV模型更加符合人类感知颜色的方式：颜色、深浅和亮暗



##### 1.4 Lab模型

Lab模型是一种于设备无关和基于生理特征的颜色模型

L表示亮度（Luminosity），a和b是两个颜色通道，取值是-127~128

a通道数值由小到大对应的颜色是从绿色变成红色，b通道数值由小到大对应的颜色是由蓝色变成黄色



##### 1.5 GRAY颜色模型

GRAY模型是一个灰度图像的模型，灰度图像只有单通道，通过0~255表示

255表示白色，灰度图像具有容量小、易于采集、便于传输等优点



#### 1.1颜色模型转换函数cvtColor()

OpenCV提供cvtColor()函数用于实现不同颜色模型之间的相互转换

**cvtColor()函数原型：**

```c++
void cvtColor(InputArray src,
              OutputArray dst,
              int code,
              int dstCn = 0
              )
```

* src：待转换颜色模型的原始图像
* dst：转换颜色模型后的目标图像
* code：颜色空间转换的标志，如由RBG空间到HSV空间
* dstCn：目标图像中的通道数，如果参数为0，自动从src和代码中导出通道数

​                                                           **cvtColor()函数颜色模型转换常用标志参数**

| 标志参数       | 简记 | 作用                           |
| -------------- | :--: | ------------------------------ |
| COLOR_BGR2BGRA |  0   | 对RGB图像添加alpha通道         |
| COLOR_BGR2RGB  |  4   | 彩色图像通道颜色模型顺序的更改 |
| COLOR_BGR2GRAY |  10  | 彩色图像转成灰度图像           |
| COLOR_GRAY2BGR |  8   | 灰度图像转成彩色图像           |
| COLOR_BGR2YUV  |  82  | RGB模型转成YUV模型             |
| COLOR_YUV2BGR  |  84  | YUV模型转成RGB模型             |
| COLOR_BGR2HSV  |  40  | RGB模型转成HSV模型             |
| COLOR_HSV2BGR  |  54  | HSV模型转成RGB模型             |
| COLOR_BGR2Lab  |  44  | RGB模型转成Lab模型             |
| COLOR_Lab1BGR  |  56  | Lab模型转成RGB模型             |

示例：

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    Mat img = imread("E:\\CLion\\opencv_xin\\cmake-build-debug\\lena.png");
    if (img.empty())
    {
        cout << "Error" << endl;
        return -1;
    }
    Mat gray,HSV,YUV,Lab,img32;
    img.convertTo(img32,CV_32F,1.0/255); //将CV_8U类型转成CV_32F类型
    cvtColor(img32,HSV,COLOR_BGR2HSV);
    cvtColor(img32,YUV,COLOR_BGR2YUV);
    cvtColor(img32,Lab,COLOR_BGR2Lab);
    cvtColor(img32,gray,COLOR_BGR2GRAY);
    
    imshow("原图",img32);
    imshow("HSV",HSV);
    imshow("YUV",YUV);
    imshow("Lab",Lab);
    imshow("gray",gray);
    waitKey(0);
    return 0;
}
```

$\textcolor{red}{注意：OpenCV的convertTo函数如果第三个参数使用默认的话，就会导致数值只是类型的改变}$

$\textcolor{red}{但在浮点数表示的颜色空间中，数值范围是0-1.0，基本上所有像素都转换成大于1的浮点数，截断后也就是一片白色区域}$

$\textcolor{red}{所以转换成浮点类型要写 1.0/255}$

#### 1.2数据类型转换函数convertTo()



**convertTo()函数原型：**

```c++
void Mat::convertTo(OutputArry m,
                    int rtype,
                    double alpha = 1,
                    double beta = 0
                    )
```

* m：转换类型后输出的图像
* rtype：转换图像的数据类型
* alpha：转换过程中的缩放因子
* beta：转换过程中的偏置因子

该函数用来实现将已有图像转换成指定数据类型的图像，第一个参数用于输出转换数据类型的图像

第二个参数用于声明转换后图像的数据类型，第三个参数和第四个参数用于声明两个数据类型间的转换关系

例如：       m(x,y)=saturate_cast<rType>(alpha*(*this)(x,y)+beta);

该转换公式可知，该转换方式就是将原有函数进行线性转换

该函数不仅可以实现不同类型之间的转换，也可以实现同一数据类型中的线性转换

注：

线性变换中最常见的一类就是所谓的矩阵变换

矩阵变换(matrix transformation)指的是这样一种把向量变成向量的对应法则：先给定一个矩阵m×n阶矩阵A，然后对于一个n维向量**x**，给它左边乘上A，变成A**x**，根据乘法行与列的运算法则，这样得到的就是个m维向量。所以它把一个n维向量变成一个m维向量，这样的变换我们称为由矩阵A诱导出的线性变换，简称为矩阵变换。这种变换一定是线性的

![](http://118.25.145.234/RM/OpenCV/90ce30475e2c4dfa9472e8f97d1d16a9.jpeg)

矩阵变换在电脑做图中非常有用，在电脑中对一张图片进行拉伸，旋转，翻转等操作，本质就是矩阵变换。因为一张图片是由很多像素点组成的，每一个像素可以看成是一个2维向量。对图片进行操作，就相当于把每个向量都变换一下。比如我想把一张图片等比例拉伸三倍，就相当于把每一个向量都拉伸三倍，方法就是让每一个向量都乘以一个3倍单位矩阵：

![](http://118.25.145.234/RM/OpenCV/ffea57287fa84c5db33ad32bd14aef9d.jpeg)

于是就会产生以下的效果：

![](http://118.25.145.234/RM/OpenCV/60bb20d1222a488b9cc1eeedf6f24ad4.jpeg)

#### 1.3多通道分离函数split()

split()函数有两种重载类型

**split()函数原型：**

```c++
void split(const Mat & src,
           Mat * mvbegin
           )
void split(InputArry m,
           OutputArrayOfArrays mv
           )
```

* src：待转换颜色模型的原始图像
* mvbegin：分离后的单通道图像，为数组形式，数组大小需要于图像的通道数相同
* m：待分离的多通道图像
* mv：分离后的单通道图像，为向量（vector）形式

该函数主要是用于将多通道的图像分离成诺干单通道图像

前者第二个参数输出Mat类型的数组，其数组的长度需要于多通道图像的通道数相等并提前定义

后者第二个参数输出一个vector<Mat>容器



1.4多通道合并函数merge()

merge()函数有两种重载类型

**merge()函数原型：**

```c++
void merge(const Mat *mv,
           size_t count,
           OutputArray dst
           )
void merge(InputArrayOfArrays mv,
           OutputArray dst
           )
```

* mv(第一种重载类型)：需要合并的图像数组，其中每个图像必须有相同的尺寸和数据类型
* count：输入的图像数组的长度，其数值必须大于0
* mv(第二种重载类型)：需要合并的图像向量（vector），其中每个图像必须有相同的尺寸和数据类型
* dst：合并后输出的图像，于mv[0]具有相同的尺寸和数据类型，通道数等于所有输入图像的通道数总和

该函数主要用于将多个图像合并成一个多通道图像，该函数也具有两种不同的函数原型

与split()函数相对应，分别需要输入数组和向量的图像数据

用于合成的图像并非都是单通道的，也可以是多个不同通道数目的图像合并成一个通道更多的图像

虽然图像的通道数目可以不同，但是需要所有图像具有相同的尺寸和数据类型



示例：（$\textcolor{red}{imshow最多显示4个通道，因此结果在Image Watch中查看}$）

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
#include <vector>
using namespace std;
using namespace cv;
int main()
{
    Mat img = imread("E:\\CLion\\opencv_xin\\cmake-build-debug\\lena.png");
    if (img.empty())
    {
        cout << "Error" << endl;
        return -1;
    }
    Mat HSV;
    cvtColor(img,HSV,COLOR_RGB2HSV);
    Mat imgs0,imgs1,imgs2; //用于存放数组类型的结果
    Mat imgv0,imgv1,imgv2; //用于存放vector类型的结果
    Mat result0,result1,result2; //多通道合并结果
    //输入数组参数的多通道分量和合并
    Mat imgs[3];
    split(img,imgs);
    imgs0 = imgs[0];
    imgs1 = imgs[1];
    imgs2 = imgs[2];
    imshow("RGB-B通道",imgs0);  //显示分离后B通道的像素值
    imshow("RGB-G通道",imgs1);  //显示分离后G通道的像素值
    imshow("RGB-R通道",imgs2);  //显示分离后R通道的像素值
    Mat zero = Mat::zeros(img.rows,img.cols, CV_8UC1);
    imgs[0] = zero;
    imgs[2] = zero;
    merge(imgs,3,result1); //用于还原G通道的真实情况，合并结果为绿色
    imshow("reslit1",result1);
    // 输入vector参数的多通道分离和合并
    vector<Mat> imgv;
    split(HSV,imgv);
    imgv0 = imgv.at(0);
    imgv1 = imgv.at(1);
    imgv2 = imgv.at(2);
    imshow("HSV-H通道",imgv0);  //显示分离后H通道的像素值
    imshow("HSV-S通道",imgv1);  //显示分离后S通道的像素值
    imshow("HSV-V通道",imgv2);  //显示分离后V通道的像素值
    merge(imgv,result2); //合并图像
    imshow("result2",result2);
    waitKey(0);
}
```

注：彩色图像一般是三通道图像（24位），一个像素需要3个值来表示分别是0-255，例如（255,178,233）。单通道（8位）只能表示一个值，范围0-255，所以都是灰度图



### 2.图像像素操作处理



#### 2.1图像像素统计

我们可以将数字图像理解成一定尺寸的矩阵，矩阵每一个元素的大小表示了图像中每一个像素的亮暗程度

因此，统计矩阵中的最大值就是寻找图像中灰度值最大的像素，计算平均值就是计算图像像素平均灰度

可以用来表示图像整体的亮暗程度

##### 2.1.1寻找图像像素最大值和最小值minMaxLoc()函数

**minMaxLoc()函数原型：**

```c++
void minMaxLoc(InputArray src,
               double * minVal,
               double * maxVal = 0,
               Point * minLoc = 0,
               Point * maxLoc = 0,
               InputArray mask = noArray()
               )
```

* src：需要寻找最大值和最小值的图像或矩阵，要求必须是单通道矩阵
* minVal：图像或矩阵中的最小值
* maxVal：图像或矩阵中的最大值
* minLoc：图像或矩阵中的最小值在矩阵中的坐标
* maxLoc：图像或矩阵中的最大值在矩阵中的坐标
* mask：掩模，用于设置在图像或矩阵中的指定区域寻找最值

Point的数据类型用于表示图像的像素坐标，图像以左上角的像素为坐标原点，水平方向为x轴，竖直方向为y轴

因此Point(x,y)对应图像的行和列，Piont(列数，行数)

针对二维坐标数据，定义了int坐标 Point2i（Point）、double坐标 Point2d、浮点坐标 Point2f

对于三维坐标，同样定义了上述坐标数据类型，只需将“2”变为“3” ，可以通过变量的x，y，z属性进行访问

例如 Point.x可以读取坐标的x轴数据

该函数第一个参数要求必须是单通道矩阵，所以如果是多通道矩阵数据，需要用reshape()函数

```c++
Mat reshape(int cn,
            int rows = 0
            )
```

* cn：转换后矩阵的通道数
* rows：转换后矩阵的行数，如果参数为0，则转换后行数与转换前相同

示例：

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    system("color F0"); //改变输出界面
    float a[12] = {1,2,3,4,5,6,7,8,9,10,0};
    Mat img = Mat(3,4,CV_32FC1,a); //单通道矩阵
    Mat imgs = Mat(2,3,CV_32FC2,a); //多通道矩阵
    double minVal,maxVal;
    Point minIdx,maxIdx;
    //寻找单通道矩阵中的最值
    minMaxLoc(img,&minVal,&maxVal,&minIdx,&maxIdx);
    cout << "img中最大值是：" << maxVal << " " << "在矩阵中的位置" << maxIdx << endl;
    cout << "img中最小值是：" << minVal << " " << "在矩阵中的位置" << minIdx << endl;
    //寻找多通道矩阵中的最值
    Mat imgs_re = imgs.reshape(1,4); //将多通道矩阵变为单通道矩阵
    minMaxLoc(imgs_re,&minVal,&maxVal,&minIdx,&maxIdx);
    cout << "imgs中最大值是：" << maxVal << " " << "在矩阵中的位置" << maxIdx << endl;
    cout << "imgs中最小值是：" << minVal << " " << "在矩阵中的位置" << minIdx << endl;
    return 0;
}
```



##### 2.1.2计算图像的平均值和标准差mean()

图像的平均值表示图像整体的亮暗程度，图像的平均值越大，则图像整体越亮

标准差表示图像中明暗的对比程度，标准差越大，表示图像中明暗变化越明显

OpenCV4提供了mean()函数来计算图像的平均值，提供了meanStdDev()函数用于同时计算平均值和标准差

**mean()函数原型：**

```c++
Scalar mean(InputArray src,
            InputArray mask = noArray()
            )
```

* src：需要寻找最大值和最小值的图像或矩阵，要求必须是单通道矩阵
* mask：掩模，用于标记求取图像指定区域平均值

该函数用来求取图像矩阵的每一个通道的平均值，第一个参数用来输入待求平均值的图像矩阵

其通道数目可以为1~4，该函数返回值是一个Scalar类型的变量，返回值有4位，分别表示输入图像4个通道的平均值

如果输入图像只有一个通道，那么返回值后3位都为0，例如输入单通道平均值为1的图像，输出结果为[1,0,0,0]

可以通过Scalar[n]来查看第n个通道的平均值，第二个参数用来控制图像求取平均值的范围

**meanStdDev()函数原型：**

```c++
void meanStdDev(InputArray src,
                OutputArray mean,
                OutputArray stddev,
                InputArray mask = noArray
                )
```

* src：需要寻找最大值和最小值的图像或矩阵，要求必须是单通道矩阵
* mean：图像每一个通道的平均值，参数为Mat类型变量
* stddev：图像每一个通道的标准差，参数为Mat类型变量
* mask：掩模，用于标记求取图像指定区域平均值和标准差

该函数第一个参数与mean()函数第一个参数相同，都可以是1~4通道的图像，不同的是该函数没有返回值

图像的平均值和标准差输出在函数的第二个参数和第三个参数，存放平均值和标准差的是Mat类型变量

如果输入图像只有一个通道，那么该函数求取的平均值和标准差变量只有一个数据

示例：

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    system("color F0"); //改变输出界面
    float a[12] = {1,2,3,4,5,6,7,8,9,10,0};
    Mat img = Mat(3,4,CV_32FC1,a); //单通道矩阵
    Mat imgs = Mat(2,3,CV_32FC2,a); //多通道矩阵
    cout << "用mean求取图像的平均值" << endl;
    Scalar myMean;
    myMean = mean(imgs);
    cout << "imgs平均值=" << myMean << endl;
    cout << "imgs第一个通道的平均值=" << myMean[0] << "   "
         << "imgs第二个通道的平均值"  << myMean[1] << endl << endl;
    cout << "用meanStdDev同时求取图像的平均值和标准差" << endl;
    Mat myMeanMat,myStddevMat;
    meanStdDev(img,myMeanMat ,myStddevMat);
    cout << "img平均值=" << myMeanMat << "    " << endl;
    cout << "img标准差=" << myStddevMat << "    " << endl << endl;
    meanStdDev(imgs,myMeanMat ,myStddevMat);
    cout << "imgs平均值=" << myMeanMat << "    " << endl;
    cout << "imgs标准差=" << myStddevMat << "    " << endl << endl;
    return 0;
}
```



#### 2.2两图像间的像素操作



##### 2.2.1两幅图像的比较运算max()和min()

**max()和min()函数原型：**

```c++
void max(InputArray src1,
         InputArray src2,
         OutputArray dst
         )
void min(InoutArray src1,
         InputArray src2,
         OutputArray dst
         )
```

* src1：第一个图像矩阵，可以是任意通道数的矩阵
* src2：第二个图像矩阵，尺寸以及数据类型需要与src1一致
* dst：保留对应位置较大（较小）灰度值后的图像矩阵，尺寸、通道数和数据类型与src1一致

这两种函数功能就是比较每一个像素的大小，按要求保留较大值或较小值，最后生成新图像

例如max()函数，第一幅图像（x，y）像素值为100，第二幅图像（x，y）像素值为10，那么输出图像（x，y）像素值为100

示例：

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    float a[12] = {1,2,3.3,4,5,9,5,7,8.2,9,10,2};
    float b[12] = {1,2.2,3,1,3,10,6,7,8,9.3,10,1};
    Mat imga = Mat(3,4,CV_32FC1,a);
    Mat imgb = Mat(3,4,CV_32FC1,b);
    Mat imgas = Mat(2,3,CV_32FC2,a);
    Mat imgbs = Mat(2,3,CV_32FC2,b);
    //对两个单通道矩阵进行比较
    Mat myMax,myMin;
    max(imga,imgb,myMax);
    min(imga,imgb,myMin);

    //对两个多通道矩阵进行比较
    Mat myMaxs,myMins;
    max(imgas,imgbs,myMaxs);
    min(imgas,imgbs,myMins);

    //对两个彩色图像进行比较
    Mat img0 = imread("E:\\CLion\\opencv_xin\\SG\\SG_1.jpg");
    Mat img1 = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg");
    if(img0.empty()||img1.empty())
    {
        cout << "error" << endl;
        return -1;
    }
    Mat comMin,comMax;
    max(img0,img1,comMax);
    min(img0,img1,comMin);
    imshow("comMin",comMin);
    imshow("comMax",comMax);

    //与掩模进行比较运算
    Mat src1 = Mat::zeros(Size(img0.rows,img0.cols),CV_8UC3);
    Rect rect(180,0,300,300);
    src1(rect) = Scalar(255,255,255); //生成一个低通300*300的掩模
    Mat comsrc1,comsrc2;
    min(img0,src1,comsrc1);
    imshow("comsrc1",comsrc1);

    Mat src2 = Mat(img0.rows,img0.cols,CV_8UC3,Scalar(0,0,255)); //生成一个显示红色通道的低通掩模
    min(img0,src2,comsrc2);
    imshow("comsrc2",comsrc2);

    //对两幅灰度图像进行比较运算
    Mat img0G,img1G,comMinG,comMaxG;
    cvtColor(img0,img0G,COLOR_BGR2GRAY);
    cvtColor(img1,img1G,COLOR_BGR2GRAY);
    max(img0G,img1G,comMaxG);
    min(img0G,img1G,comMinG);
    imshow("comMinG",comMinG);
    imshow("comMaxG",comMaxG);
    waitKey(0);
    return 0;
}
```

$\textcolor{red}{注意：尺寸、通道数和数据类型要一致}$



##### 2.2.2两幅图像的逻辑运算

OpenCV4对两个图像像素之间的“与” “或” ”异或“ 以及”非“运算分别提供了

bitwise_and()、bitwise_or()、bitwise_xor()和bitwwise_not()函数

与运算（&）

参加运算的两个数据，按[二进制](https://so.csdn.net/so/search?q=二进制&spm=1001.2101.3001.7020)位进行“与”运算。

运算规则：0&0=0; 0&1=0; 1&0=0; 1&1=1;
即：`两位同时为“1”，结果才为“1”，否则为0`

例如：3&5 即 0000 0011 & 0000 0101 = 0000 0001 因此，3&5的值为1。

或运算（|）

参加运算的两个对象，按二进制位进行“或”运算。

运算规则：0|0=0； 0|1=1； 1|0=1； 1|1=1；
即 ：`参加运算的两个对象只要有一个为1，其值为1`
例如: 3|5 可写算式如下： 0000 0011 | 0000 0101 = 0000 0111(十进制为7)因此，3|5的值为7。

异或运算（^）

参加运算的两个数据，按二进制位进行“异或”运算。

运算规则：0^0=0； 0^1=1； 1^0=1； 1^1=0；
即：`参加运算的两个对象，如果两个相应位为“异”（值不同），则该位结果为1，否则为0`
例如：9^5可写成算式如下： 00001001 ^ 00000101 = 00001100 (十进制为12)因此，9^5的值为12。

非运算（~）

参加运算的两个数据，按二进制位进行“非”运算。

即：`"非"运算符号表示为~，运算法则为按位取反，也就是遇1取0,遇0取1，即 ~1 = 0 , ~0 = 1`

例如：~5可以写成：~00000101 = 1111010(十进制为250)因此，~5的值为250。

**OpenCV4中像素逻辑运算函数原型：**

```c++
//像素求"与"运算
void bitwise_and(InputArray src1,
                 InputArray src2,
                 OutputArray dst,
                 InputArray mask = noArray()
                 )
//像素求"或"运算
void bitwise_or(InputArray src1,
                 InputArray src2,
                 OutputArray dst,
                 InputArray mask = noArray()
                 )
//像素求"异或"运算
void bitwise_xor(InputArray src1,
                 InputArray src2,
                 OutputArray dst,
                 InputArray mask = noArray()
                 )
//像素求"非"运算
void bitwise_not(InputArray src,
                 OutputArray dst,
                 InputArray mask = noArray()
                 )
```

* src1：第一个图像矩阵，可以是多通道图像数据
* src2：第二个图像矩阵，尺寸以及数据类型需要与src1一致
* dst：逻辑运算输出结果，尺寸以及数据类型需要与src1一致
* mask：用于设置图像或矩阵中的逻辑运算的范围

在进行逻辑运算时，一定要保证两个图像矩阵之间的尺寸、数据类型和通道数相同，多个通道进行逻辑运算时不同通道是独立进行

示例：

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_1.jpg");
    if (img.empty())
    {
        cout << "Error" << endl;
        return -1;
    }
    //创建两个黑白图像
    Mat img0 = Mat::zeros(200,200,CV_8UC1);
    Mat img1 = Mat::zeros(200,200,CV_8UC1);
    Rect rect0(50,50,100,100);
    img0(rect0) = Scalar(255);
    Rect rect1(100,100,100,100);
    img1(rect1) = Scalar(255);
    imshow("img0",img0);
    imshow("img1",img1);

    //进行逻辑运算
    Mat myAnd,myOr,myXor,myNot,imgNot;
    bitwise_and(img0,img1,myAnd);
    bitwise_or(img0,img1,myOr);
    bitwise_xor(img0,img1,myXor);
    bitwise_not(img0,myNot);
    bitwise_not(img,imgNot);
    imshow("myAnd",myAnd);
    imshow("myXor",myXor);
    imshow("myOr",myOr);
    imshow("myNot",myNot);
    imshow("img",img);
    imshow("imgNot",imgNot);
    waitKey();
    return 0;
}
```

##### 2.2.3图像二值化threshold()和adaptiveThreshold()

只有黑色和白色的图像，这种“非黑即白”图像像素的灰度值无论这种什么数据类型种豆只有最大值和最小值两种取值

因此称为二值图像。二值图像色彩种类少，可以进行高度的压缩，节省储存空间，将非二值图像经过计算变成二值图像的过程称为

图像的二值化，OpenCV4提供了threshold()和adaptiveThreshold()用于实现图像的二值化

**threshold()函数原型：**

```c++
double threshold(InputArray src,
                 OutputArray dst,
                 double thresh,
                 double maxval,
                 int type
                 )
```

* src：待二值化的图像，图像只能是CV_8U和CV_32F两种数据类型。

  对于图像通道数目的要求与选择的二值化方法有关

* dst：二值化后的图像，与输入图像具有相同的尺寸、数据类型和通道数

* thresh：二值化的阈值

* maxval：二值化过程的最大值，只在THRESH_BINARY和THRESH_BINARY_INV两种二值化方法中才使用

* type：选择图像二值化方法的标志

该函数是众多二值化方法的集成，所有的方法都实现了一个功能，就是给定一个阈值，计算所有像素灰度值与这个阈值关系

​                                                                      **二值化方法可选择的标志及含义**

| 标志参数          | 简记 | 作用                                |
| ----------------- | :--: | ----------------------------------- |
| THRESH_BINARY     |  0   | 灰度值大于阈值的为最大值，其他值为0 |
| THRESH_BINARY_INV |  1   | 灰度值大于阈值的为0，其他值为最大值 |
| THRESH_TRUNC      |  2   | 灰度值大于阈值的为阈值，其他值不变  |
| THRESH_TOZERO     |  3   | 灰度值大于阈值的不变，其他值为0     |
| THRESH_TOZERO_INV |  4   | 灰度值大于阈值的为0，其他值不变     |
| THRESH_OTSU       |  8   | 大津法自动寻求全局阈值              |
| THRESH_TRIANGLE   |  16  | 三角法自动寻求全局阈值              |

**1.THRESH_BINARY和THRESH_BINARY_INV**

这两个标志是相反的二值化方法，**THRESH_BINARY**是将灰度值与阈值（第三个参数thresh）进行比较，如果灰度值大于阈值，

将灰度值改为函数中第四个参数maxval的值，否则将灰度值改为0

**THRESH_BINARY_INV**与其相反，如果灰度值大于阈值，就将灰度值改为0，否则将灰度值改为maxval的值

**2.THRESH_TRUNC**

这个标志相当于重新给图像的灰度值设定一个新的最大值，将大于新的最大值的灰度值全部重新设置为新的最大值

逻辑为将灰度值与阈值thresh进行比较，如果灰度值大于thresh，则将灰度值改为thresh，否则保持灰度值不变

**3.THRESH_TOZERO和THRESH_TOZERO_INV**

这两种标志是相反的阈值比较方法，**THRESH_TOZERO**表示将灰度值与阈值比较，如果灰度值大于阈值，则保持不变，否则改为0

**THRESH_BINARY_INV**是将灰度值与阈值进行比较，如果灰度值小于或等于thresh，则保持不变，否则改为0

**4.THRESH_OTSU和THRESH_TRIANGLE**

这两种标志表示利用大津法和三角形法结合图像灰度值分布特性获取二值化的阈值，并将阈值以函数返回值的形式给出

因此，使用这两个标志时，第三个参数thresh将由系统自动给出

注意：OpenCV4对这两种标志只支持输入CV_8UC1类型图像



在实际情况中，由于光照不均匀以及阴影的存在，全局只有一个阈值会使得阴影处的白色区域也会被函数二值化成黑色

adaptiveThreshold()函数提供了两种局部自适应阈值的二值化方法

**adaptiveThreshold()函数函数原型：**

```c++
void adaptiveThreshold(InputArray src,
                 	   OutputArray dst,
                 	   double maxvalue,
                 	   int adaptiveMethod,
                       int thresholdType,
                       double C
                       )
```

* src：待二值化的图像，图像只能是CV_8U数据类型。

* dst：二值化后的图像，与输入图像具有相同的尺寸、数据类型和通道数
* maxValue：二值化的最大值
* adaptiveMethod：自适应确定阈值的方法，分为均值法**ADAPTIVE_THRESH_MEAE_C**和高斯法**ADAPTIVE_THRESH_GAUSSIAN_C**
* thresholdType：选择二值化方法的标志，只能是**THRESH_BINARY**和**THRESH_BINARY_INV**

* blockSize：自适应确定阈值的像素领域大小，一般是3、5、7的奇数
* C：从平均值或者加权平均值中减去的常数，可以为正，也可以为负

##### 2.2.4 LUT

如果需要与多个阈值进行比较，就需要用到显示查找表（Look-Up-Table，LUT）

简单说，LUT就是一个像素值灰度值的映射表，以像素灰度值作为索引，以灰度值映射后的数值作为表中的内容

例如，由一个长度为5的存放字符的数组[a,b,c,d,e]，LUT就是通过这个数组将0映射为a，将1映射成b，依次类推

其映射关系为P[0] = a、P[1] = b

**LUT()函数原型：**

```c++
void LUT(InputArray src,
         InputArray lut,
         OutputArray dst
         )
```

* src：输入图像矩阵，数据类型只能是CV_8U
* lut：256个像素灰度值的查找表，尺寸与src相同，数据类型与lut相同

该函数第一个参数数据类型要求必须是CV_8U类型，单、但可以是多通道的图像矩阵



#### 2.3图像变换

OpenCV4提供了函数来实现图像形状的变换，包括图像的尺寸变换、图像翻转以及图像旋转

##### 2.3.1图像连接vconcat()

OpenCV4针对图像左右连接和上下连接两种方式提供了两个不同的函数

vconcat()函数用于实现图像或者矩阵数据的上下连接

该函数可以连接存放在数组变量中的多个Mat类型的数据，也可以直接连接两个Mat类型数据

**vconcat()函数原型（1）：**

```c++
void vconcat(const Mat* src,
             size_t nsrc,
             OutputArray dst
             )
```

* src：Mat矩阵类型的数组
* nsrc：数组中Mat类型数据的数目
* dst：连接后的Mat类矩阵

该函数对存放在数组矩阵中的Mat类型数据进行纵向连接，第一个参数是存放多个Mat类型数据的数组

要求数组中所有的Mat类型具有相同的列数并且具有相同的数据类型和通道数。

第二个参数是数组中含有的Mat类型数据的数目

最后一个参数是拼接后输出的结果

结果的宽度与第一个Mat数据类型相同，高度为数组所有Mat类型数据的高度的总和

并且与第一个Mat类型数据具有相同的数据类型和通道数

**vconcat函数()原型（2）：**

```c++
void vconcat(InputArray src1,
             InputArray src2,
             OutputArray dst
             )
```

* src1：第一个需要连接的Mat类矩阵
* src2：第二个需要连接的Mat类矩阵，与第一个参数具有相同的宽度、数据类型和通道数
* dst：连接后的Mat类矩阵

该函数直接对两个Mat类型的数据进行竖向连接。前两个参数分别是需要连接的两个Mat类型变量

两者需要具有相同的宽度、数据类型及通道数，第三个参数是连接后输出结果，第一个参数在上，第二个参数在下



hconcat()函数用于实现图像或者矩阵数据的左右连接，与vconcat()函数类似

该函数既可以连接存放在数据变量中的多个Mat类型数据，又可以直接连接两个Mat类型数据

**hconcat()函数原型（1）：**

```c++
void hconcat(const Mat* src,
             size_t nsrc,
             OutputArray dst
             )
```

* src：Mat矩阵类型的数组
* nsrc：数组中Mat类型数据的数目
* dst：连接后的Mat类矩阵

该函数要求第一个参数数组中所有Mat类型变量具有相同的高度、数据类型和通道数

**hconcat()函数原型（2）：**

```c++
void hconcat(InputArray src1,
             InputArray src2,
             OutputArray dst
             )
```

* src1：第一个需要连接的Mat类矩阵
* src2：第二个需要连接的Mat类矩阵，与第一个参数具有相同的宽度、数据类型和通道数
* dst：连接后的Mat类矩阵

两个Mat类型需要具有相同的宽度、数据类型及通道数

示例：

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    //矩阵数组的横竖连接
    Mat matArray[] = { Mat(1,2,CV_32FC1,Scalar(1)),
                       Mat(1,2,CV_32FC1,Scalar(2)) };
    Mat vout,hour;
    vconcat(matArray,2,vout);
    cout << "图像数组竖向连接：" << endl << vout << endl;
    hconcat(matArray,2,hour);
    cout << "图像数组横向连接：" << endl << hour << endl;

    //读取4个子图像，00表示左上角、01表示右上角、10表示左下角、11表示右下角

    Mat img00 = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg");
    Mat img01 = imread("E:\\CLion\\opencv_xin\\SG\\SG_1.jpg");
    Mat img10 = imread("E:\\CLion\\opencv_xin\\SG\\SG_2.jpg");
    Mat img11 = imread("E:\\CLion\\opencv_xin\\SG\\SG_3.jpg");

    if (img00.empty() || img01.empty() || img10.empty() || img11.empty())
    {
        cout << "Error" << endl;
        return -1;
    }
    //显示4个子图像
    imshow("img00",img00);
    imshow("img01",img01);
    imshow("img10",img10);
    imshow("img11",img11);

    //图像连接
    Mat img,img0,img1;
    //图像横向连接
    hconcat(img00,img01,img0);
    hconcat(img10,img11,img1);
    //横向连接结果再进行竖向连接
    vconcat(img0,img1,img);
    //显示连接图像结果
    namedWindow("img0",WINDOW_NORMAL);
    namedWindow("img1",WINDOW_NORMAL);
    namedWindow("img",WINDOW_NORMAL);
    imshow("img0",img0);
    imshow("img1",img1);
    imshow("img",img);
    waitKey(0);
    return 0;
}
```

##### 2.3.2图像尺寸变换resize()

图像的尺寸变换实际是就是改变图像的长和宽，实现图像的缩放

OpenCV提供了resize()函数用于将图像修改成指定尺寸

**resize()函数原型：**

```c++
void resize(InputArray src,
            OutputArray dst,
            Size dsize,
            double fx = 0,
            double fy = 0,
            int interpolation = INTER_LINEAR
            )
```

* src：输入图像
* dst：输出图像，图像的数据类型与src相同
* dsize：输出图像的尺寸
* fx：水平轴的比例因子，如果将水平轴变为原来的两倍，则赋值为2
* fy：垂直轴的比例因子，如果将垂直轴变为原来的两倍，则赋值为2
* interpolation：插值方法的标志

该函数主要用来对图像尺寸进行缩放，前两个参数分别是输入图像和尺寸缩放之后的输出图像

dsize和fx（fy）同时可以调整输出图像的参数，因此两类参数在实际使用的时候只需要使用一类

当根据两个参数计算出来的输出图像尺寸不一致时，以dsize设置的图像尺寸为准

最后一个参数是选择图像插值的标志，在图像缩放相同的尺寸时，选择不同的插值方法会具有不同的效果

一般来说，如果要缩小图像，那么通常使用**INTER_AREA**标志会有较好的效果

放大图像的话，使用**INTER_CUBIC**和**INTER_LINEAR**标志通常会有较好的效果

前者计算速度较慢，后者较快，前者效果较好，当两者差距不大

​                                                                                    **插值方法标志**

| 标志参数           | 简记 | 作用                                       |
| ------------------ | :--: | ------------------------------------------ |
| INTER_NEAREST      |  0   | 最近邻插值法                               |
| INTER_LINEAR       |  1   | 双线性插值法                               |
| INTER_CUBIC        |  2   | 双三次插值                                 |
| INTER_AREA         |  3   | 使用像素区域关系重新采样，首选用于图像缩小 |
| INTER_LANCZOS4     |  4   | Lanczos插值法                              |
| INTER_LINEAR_EXACT |  5   | 为精确双线性插值法                         |
| INTER_MAX          |  7   | 用掩码进行插值                             |

示例：

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    Mat gray = imread("E:\\CLion\\opencv_xin\\SG\\SG_1.jpg");
    if (gray.empty()) {
        cout << "Error" << endl;
        return -1;
    }

    Mat smallImg,bigImg0,bigImg1,bigImg2,s;
    resize(gray ,smallImg,Size(15,15),0,0,INTER_AREA); //先将图像缩小
    resize(smallImg,bigImg0,Size(30,30),0,0,INTER_NEAREST); //最近邻插值法
    resize(smallImg,bigImg1,Size(30,30),0,0,INTER_LINEAR); //双线性插值法
    resize(smallImg,bigImg2,Size(30,30),0,0,INTER_CUBIC); //双三次插值
    namedWindow("smallImg",WINDOW_NORMAL); //图像尺寸太小，一定要设置可以调节窗口大小标志
    imshow("smallImg",smallImg);
    namedWindow("bigImg0",WINDOW_NORMAL);
    imshow("bigImg0",bigImg0);
    namedWindow("bigImg1",WINDOW_NORMAL);
    imshow("bigImg1",bigImg0);
    namedWindow("bigImg2",WINDOW_NORMAL);
    imshow("bigImg2",bigImg0);
    waitKey(0);
    return 0;
}
```

##### 2.3.3图像翻转变换flip()

OpenCV4提供了flip()函数用于图像的翻转

**flip()函数原型：**

```c++
void flip(InputArray src,
          OutputArray dst,
          int flipCode
          )
```

* src：输入图像
* dst：输出图像，与src具有相同的大小、数据类型及通道数
* flipCode：翻转方式标志，数值大于0表示绕y轴进行翻转；数值等于0，表示绕x轴进行翻转；数值小于0，表示绕两个轴翻转

示例：

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_1.jpg");
    if (img.empty()) {
        cout << "Error" << endl;
        return -1;
    }
    Mat img_x,img_y,img_xy;
    flip(img,img_x,0); //以x轴对称
    flip(img,img_y,1); //以y轴对称
    flip(img,img_xy,-1); //先以x轴对称，再以y轴对称
    imshow("img",img);
    imshow("img_x",img_x);
    imshow("img_y",img_y);
    imshow("img_xy",img_xy);
    waitKey(0);
    return 0;
}
```

##### 仿射变换原理

OpenCV4中没有专门用于图像旋转的函数，而是通过图像的仿射变换实现图像的旋转

简单来说，“仿射变换”就是：“线性变换”+“平移”。

先看什么是线性变换？

线性变换从几何直观有三个要点：

- 变换前是直线的，变换后依然是直线
- 直线比例保持不变
- 变换前是原点的，变换后依然是原点

比如说旋转：

![](http://118.25.145.234/RM/OpenCV/a.png)

![](http://118.25.145.234/RM/OpenCV/b.png)

简单讲一下旋转是怎么实现的，可以让我们进一步了解代数是怎么描述线性变换的。

![](http://118.25.145.234/RM/OpenCV/awd.png)

![](http://118.25.145.234/RM/OpenCV/awdd.png)

![](http://118.25.145.234/RM/OpenCV/awddd.png)



仿射变换从几何直观只有两个要点：

- 变换前是直线的，变换后依然是直线
- 直线比例保持不变

少了原点保持不变这一条。

比如平移：

![](http://118.25.145.234/RM/OpenCV/aaa.png)

因此，平移不再是线性变化了，而是仿射变化。

线性变换是通过矩阵乘法来实现的，仿射变换不能光通过矩阵乘法来实现，还得有加法。

![](http://118.25.145.234/RM/OpenCV/ad.png)

![](http://118.25.145.234/RM/OpenCV/ada.png)

![](http://118.25.145.234/RM/OpenCV/adad.png)



因为我们表示仿射变换为：

![image-20221120172125724](C:\Users\小新\AppData\Roaming\Typora\typora-user-images\image-20221120172125724.png)

![](http://118.25.145.234/RM/OpenCV/x.png)

![](http://118.25.145.234/RM/OpenCV/xx.png)



![](http://118.25.145.234/RM/OpenCV/xxx.webp)



![image-20221120173427382](C:\Users\小新\AppData\Roaming\Typora\typora-user-images\image-20221120173427382.png)



![image-20221120174103397](C:\Users\小新\AppData\Roaming\Typora\typora-user-images\image-20221120174103397.png)

![image-20221120174120135](C:\Users\小新\AppData\Roaming\Typora\typora-user-images\image-20221120174120135.png)

实现函数的旋转，首先需要确定旋转角度和旋转中心，之后确定旋转矩阵，最终通过仿射变换实现图像旋转



##### 2.3.4图像仿射变换getRotationMatrix2D()

OpenCV4提供了getRotationMatrix2D()函数用于计算旋转矩阵，提供warpAffine()函数用于实现图像的仿射变换

**getRotationMatrix2D()函数原型：**

```c++
Mat getRotationMatrix2D(Point2f center,
                        double angle,
                        double scale
                        )
```

* center：图像旋转的中心位置
* angle：图像旋转的角度，单位为都，正值逆时针旋转
* scale，两个轴的比例因子，可以实现旋转过程中的图像缩放，不缩放则输入1

该函数输入旋转角度和旋转中心，返回图像旋转矩阵，返回值的数据类型为Mat类，是一个2x3的矩阵，例如：

![image-20221120175959011](C:\Users\小新\AppData\Roaming\Typora\typora-user-images\image-20221120175959011.png)

确定旋转矩阵后，通过warpAffine()函数进行仿射变换，就可以实现图像的旋转

**warpAffine()函数原型：**

```c++
void warpAffine(InputArray src,
                OutputArray dst,
                InputArray M,
                Size dsize,
                int flags = INTER_LINEAR,
                int borderMode = BORDER_CONSTANT,
                const Scalar& borderValue = Scalar()
                )
```

* src：输入图像
* dst：仿射变换后输出图像，与src数据类型相同，尺寸与dsize相同
* M：2x3的变换矩阵
* dsize：输出图像的尺寸
* flags：插值方法标志
* borderMode：像素边界外推方法的标志
* borderValue：填充边界使用的数值，默认情况下为0

该函数第三个参数为前面求取的旋转矩阵，第四个参数是输出图像的尺寸，第五个参数是仿射变换插值方法的标志

第六个参数为像素边界外推方法标志，第七个参数把**BORDER_CONSTANT**作为定值，默认情况下为0

​                                                                       **图像仿射变换插值方法标志**

| 方法标志           | 简记 | 作用                                                         |
| ------------------ | :--: | ------------------------------------------------------------ |
| WARP_FILL_OUTLIERS |  8   | 填充所有输出图像的像素，如果部分像素落在输入图像的边界外，则它们的值设定为fillval |
| WARP_INVERSE_MAP   |  16  | 设置为M输出图像到输入图像的反变换                            |

​                                                                               **边界外推方法标志**

| 方法标志           | 简记 | 作用                                              |
| ------------------ | :--: | ------------------------------------------------- |
| BORDER_CONSTANT    |  0   | 用特定值填充，如iiiiii\|abcdefgh\|iiiiii          |
| BORDER_REPLICATE   |  1   | 两端复制填充，如aaaaaa\|abcdefgh\|bbbbbb          |
| BORDER_REFLECT     |  2   | 倒序填充，如fedcba\|abcdefgh\|hgfedcb             |
| BORDER_WRAP        |  3   | 正序填充，如cdefgh\|abcdefgh\|abcdefg             |
| BORDER_REFLECT_101 |  4   | 不含边界值的倒序填充，如gfedcb\|abcdefgh\|gfedcba |
| BORDER_TRANSPARENT |  5   | 随机填充，uvwxyz\|abcdefgh\|ijklmno               |
| BORDER_REFLECT101  |  4   | 与 BORDER_REFLECT_101 相同                        |
| BORDER_DEFAULT     |  4   | 与 BORDER_REFLECT_101 相同                        |
| BORDER_ISOLATED    |  16  | 不关心感兴趣区域之外的部分                        |

仿射变换又称三点变换，如果知道前后两幅图像中3个像素点的对应关系，就可以求得仿射变换中的变换矩阵M

OpenCV4提供了getAffineTransform()函数来确定变换矩阵

```c++
Mat getAffineTransform(const Point2f src[],
                       const Point2f dst[]
                       )
```

* src[]：源图像中的3个像素坐标
* dst[]：目标图像中的3个像素坐标

示例：

```c++
#include <opencv2\opencv.hpp>
#include <iostream>
using namespace std;
using namespace cv;

int main()
{
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_1.jpg");
    if (img.empty())
    {
        cout << "请确认图像文件名称是否正确" << endl;
        return -1;
    }

    Mat rotation0, rotation1, img_warp0, img_warp1;
    double angle = 30;  //设置图像旋转的角度
    Size dst_size(img.rows, img.cols);  //设置输出图像的尺寸
    Point2f center(img.rows / 2.0, img.cols / 2.0);  //设置图像的旋转中心
    rotation0 = getRotationMatrix2D(center, angle, 1);  //计算放射变换矩阵
    warpAffine(img, img_warp0, rotation0, dst_size,0,0,Scalar(255,255,255));  //进行仿射变换（白色）
    imshow("img_warp0", img_warp0);
    //根据定义的三个点进行仿射变换
    Point2f src_points[3];
    Point2f dst_points[3];
    src_points[0] = Point2f(0, 0);  //原始图像中的三个点
    src_points[1] = Point2f(0, (float)(img.cols - 1));
    src_points[2] = Point2f((float)(img.rows - 1), (float)(img.cols - 1));
    //放射变换后图像中的三个点
    dst_points[0] = Point2f((float)(img.rows)*0.11, (float)(img.cols)*0.20);
    dst_points[1] = Point2f((float)(img.rows)*0.15, (float)(img.cols)*0.70);
    dst_points[2] = Point2f((float)(img.rows)*0.81, (float)(img.cols)*0.85);
    rotation1 = getAffineTransform(src_points, dst_points);  //根据对应点求取仿射变换矩阵
    warpAffine(img, img_warp1, rotation1, dst_size);  //进行仿射变换
    imshow("img_warp1", img_warp1);
    waitKey(0);
    return 0;
}
```

##### 2.3.5图像透视变换getPerspectiveTransform()

透视变换是按照物体成像投影的规律进行变换，将物体重新投影到新的成像平面

![](http://118.25.145.234/RM/OpenCV/awdxxxx.png)

透视变换常用于机器人视觉巡航研究中，由于相机视场于地面存在倾斜角使得物体成像产生畸变，通常通过透视变换实现图像的矫正

在透视变换中，透视前后的图像之间的变换关系可以用一个3x3的矩阵表示，该矩阵可以通过两幅图像4个对应点的坐标求取

透视变换又称“四点变换”，OpenCV4提供了getPerspectiveTransform()函数和进行透视变换的warpPerspective()函数

**getPerspectiveTransform()函数原型：**

```c++
Mat getPerspectiveTransform(const Point2f src[],
                            const Point2f dst[],
                            int solveMethod = DECOMP_LU
                            )
```

* src[]：原图像的4个像素坐标
* dst[]：目标图像的4给像素坐标
* solveMethod：选择计算透视变换矩阵方法的标志

​                                                              **getPerspectiveTransform()函数计算方法标志**

| 标志参数        | 简记 | 作用                                     |
| --------------- | :--: | ---------------------------------------- |
| DECOMP_LU       |  0   | 最佳主轴元素的高斯消元法                 |
| DECOMP_SVD      |  1   | 奇异值分解（SVD）方法                    |
| DECOMP_EIG      |  2   | 特征值分解法                             |
| DECOMP_CHOLESKY |  3   | Cholesky分解法                           |
| DECOMP_QR       |  4   | QR分解法                                 |
| DECOMP_NORMAL   |  16  | 使用正规方程公式，可以与其他标志一起使用 |

**warpPerspective()函数原型：**

```c++
void warpPerspective(InputArray src,
                     OutputArray dst,
                     InputArray M,
                     Size dsize,
                     int flags =INTER_LINEAR,
                     int borderMode = BORDER_CONSTANT,
                     const Scalar& borderValue = Scalar()
                     )
```

* src：输入图像
* dst：透视变换后输出图像，与src数据类型相同，尺寸与dsize相同
* M：3x3的变换矩阵
* dsize：输出图像的尺寸
* flags：插值方法标志
* borderMode：像素边界外推方法的标志
* borderValue：填充边界使用的数值，默认情况下为0

示例：

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\ewm_0.png");
    if (img.empty()) {
        cout << "Error" << endl;
        return -1;
    }
    Point2f src_points[4];
    Point2f dst_points[4];
    //通过Opencv Image viewer查看二维码的4个角
    //137 187 | 478 44 | 127 375 | 515 510
    //0 0 | 643 0 | 0 586 | 643 586
    src_points[0] = Point2f(137.0,187.0);
    src_points[1] = Point2f(478.0,44.0);
    src_points[2] = Point2f(127.0,375.0);
    src_points[3] = Point2f(515.0,510.0);
    //期望透视后二维码4给角点的坐标
    dst_points[0] = Point2f(0.0,0.0);
    dst_points[1] = Point2f(643.0,0.0);
    dst_points[2] = Point2f(0.0,586.0);
    dst_points[3] = Point2f(643.0,586.0);
    Mat rotation,img_warp;
    rotation = getPerspectiveTransform(src_points,dst_points); //计算透视变换矩阵
    warpPerspective(img,img_warp,rotation,img.size()); //透视变换投影
    imshow("img",img);
    imshow("img_warp",img_warp);
    waitKey(0);
    return 0;
}
```

##### 极坐标变换

![image-20221122162443610](C:\Users\小新\AppData\Roaming\Typora\typora-user-images\image-20221122162443610.png)

极点和极轴，极点就相当于平面直角坐标系中的原点，极轴就是一根由极点引出的射线（射线，只有一边）

类似于平面直角坐标系中的x轴

平面直角坐标系中的点用（x，y）表示，而极坐标系中的点通常用（ρ，θ）表示 

A在B的北偏东45度50米处， C在B的西偏南75度150米处

这种其实就是极坐标的思想，在航海过程中，由于横纵坐标通常难以确定，所以通常以极坐标的方式判断其他东西的位置 

极坐标与直角坐标的变换在数学上是可逆的，但实际变换时存在误差，角度计算精度约为 0.3度

直角坐标系以变换中心为圆心的同一个圆上的点，在极坐标系中显示为一条直线。因此，用极坐标变换可以实现圆形物体的图像修正。

![](http://118.25.145.234/RM/OpenCV/ji_0.png)

##### 2.3.6极坐标变换warpPolar()

极坐标变换就是将图像在直角坐标系与极坐标系中相互变换，可以将一个圆形矩阵变换成一个矩阵图像

常用于处理钟表，圆盘等图像，圆形边缘上的文字经过极坐标变换后可以垂直地排列在新图像的边缘，便于对文字进行识别和检测

OpenCV4提供了warpPolar()函数用于图像的极坐标变换

**warpPolar()函数原型：**

```c++
void warpPolar(InputArray src,
               OutputArray dst,
               Size dsize,
               Point2f center,
               double maxRadius,
               int flags
               )
```

* src：源图像，可以是灰度图像或彩色图像
* dst：极坐标变换后输出图像，与源图像jiyou相同的数据类型和通道数
* dsize：目标图像大小
* center：极坐标变换时极坐标的原点坐标
* maxRadius：变换时边界圆的半径，它也决定了逆变换时的比例参数
* flags：插值方法与极坐标映射方法标志，两个方法之间通过“+”或者“|”号连接

该函数实现了图像极坐标变换和半对数极坐标变换，第一个参数是需要进行极坐标变换的图像（灰度图像和彩色图像均可）

第二个参数是变换后的输出图像，与输入图像具有相同的数据类型和通道数，第三个参数是变换后图像大小

第四个参数是极坐标变换时极坐标原点在原始图像中的位置，该参数同样适用于逆变换中，第五个参数是变换时边界圆的半径

决定了逆变换时的比例参数，最后一个参数是变换方法的选择参数

​                                                                  **warpPolar()函数极坐标映射方法标志**

| 标志参数          | 作用             |
| ----------------- | ---------------- |
| WARP_POLAR_LINEAR | 极坐标变换       |
| WARP_POLAR_LOG    | 半对数极坐标变换 |
| WARP_INVERSE_MAP  | 逆变换           |

示例：

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\yuanxin_0.png");
    if (img.empty()) {
        cout << "Error" << endl;
        return -1;
    }
    Mat img1,img2;
    Point2f center = Point2f(img.cols/2,img.rows/2); //极坐标在图像种的原点
    //正极坐标变换
    warpPolar(img,img1,Size(300,600),center,center.x,INTER_LINEAR + WARP_POLAR_LINEAR);
    //逆极坐标变换
    warpPolar(img1,img2,Size(img.cols,img.rows),center,center.x,INTER_LINEAR + WARP_INVERSE_MAP);
    imshow("img",img);
    imshow("img1",img1);
    imshow("img2",img2);
    waitKey(0);
    return 0;
}
```

#### 2.4在图像上绘制几何图形

##### 2.4.1绘制圆形circle()

OpenCV4提供了circle()函数用于绘制圆形

**circle()函数原型：**

```c++
void cv::circle(InputArray img,
            Point center,
            int radius,
            const Scalar& color,
            int thickness = 1,
            int shift = 0
            )
```

* img：需要绘制圆形的图像
* center：圆形的圆心位置坐标
* radius：圆形的半径，单位为像素
* color：圆形的颜色
* thickness：轮廓的宽度，如果数值为负，则绘制一个实心圆
* lineType：边界的类型，可取值为**FILLED、LINE_4、LINE_8**和**LINE_AA**
* shift：中心坐标和半径数值中的小数数位

##### 2.4.2绘制直线line()

在OpenCV中提供了line()函数用于绘制直线

**line()函数原型：**

```c++

void cv::line(InputOutArray img,
              Point pt1,
              Point pt2,
              const Scalar& color,
              int thickness = 1,
              int lineType = LINE_8,
              int shift = 0
              )
```

* pt1：直线起点在图像中的坐标。
* pt2：直线终点在图像中的坐标。
* color：直线的颜色，用三通道表示

##### 2.4.3绘制椭圆ellipse()

在OpenCV中提供了ellipse()函数用于绘制椭圆

```c++

void cv::ellipse(InputOutputArray img,
                 Point center,
                 Size axes,
                 double angle,
                 double startAngle,
                 double endAngle,
                 const Scalar& color,
                 int thickness = 1,
                 int lineType = LINE_8,
                 int shift = 0
				 )
```

* center：椭圆的中心坐标
* axes：椭圆主轴大小的一半
* angle：椭圆旋转的角度，单位为度
* startAngle：椭圆弧起始的角度，单位为度
* endAngle：椭圆弧终止的角度，单位为度

 在OpenCV中还提供了另一个名为ellipse2Poly()的函数用于输出椭圆边界的像素坐标，但是不会在图像中绘制椭圆

**ellipse2Poly()函数原型：**

```c++
void cv::ellipse2Poly(Point center,
                      Size axes,
                      int angle,
                      int arcStart,
                      int arcEnd,
                      int delta,
                      std::vector< Point >& pts
                      )
```

* delta：后续折线顶点之间的角度，它定义了近似精度
* pts：椭圆边缘像素坐标向量集合

##### 2.4.4绘制多边形rectangle()

在OpenCV中提供了函数rectangle()函数fillPoly()用于绘制多边形

**rectangle()函数原型：**

```c++
void cv::rectangle(InputOutArray img,
                   Point pt1,
                   Point pt2,
                   const Sclar& color,
                   int thickness = 1,
                   int lineType = LINE_8,
                   int shift = 0
                   )
 
 
void cv::rectangle(InputOutArray img,
                   Rect rec,
                   const Scalar& color,
                   int thickness = 1,
                   int lineType = LINE_8,
                   int shift = 0
                   )
```

* pt1：矩形的一个顶点
* pt2：矩形中与pt1相对的顶点，即两个点在对角线上
* rec：矩形左上角顶点和长度
* color：直线的颜色，用三通道表示
* thickness：轮廓的宽度，如果数值为负，则绘制一个实心圆
* shift：中心坐标和半径数值中的小数数位

Rect变量，该变量在OpenCV中表示矩形的含义，与Point，Vec3b等类型相同，Rect表示的是矩形的左上角像素坐标，以及矩形的长和宽，该类型定义的格式为Rect（像素的X坐标，像素的Y坐标，矩形的宽，矩形的高）

**fillPoly()函数原型：**

```c++
void cv::fillPoly(InputOutputArray img,
                  const Point ** pts,
                  const int * npts,
                  int ncontours,
                  const Scalar & color,
                  int lineType = LINE_8,
                  int shift = 0,
                  Point offset = Point()
                  )
```

* pts：多边形顶点数组，可以存放多个多边形的顶点坐标的数组
* npts：每个多边形顶点数组中顶点的个数
* ncoutours：绘制多边形的个数
* offset：所有顶点的可选偏移

##### 2.4.5文字生成putText()

在OpenCV中提供了在图像生成的文字putText()函数

**putText()函数原型：**

```c++
void cv::putText(InputOutputArray img,
                 const String& text,
                 Point org,
                 int fontFace,
                 double fontScale,
                 Scalar color,
                 int thickness = 1,
                 int lineType = LINE_8,
                 bool bottomLeftOrigin = false
                 )
```

* text：输出到图像的文字，目前OpenCV只支持英文
* org：图像中文字字符的左下角像素坐标
* fontFace：字体类型的选择标志、参数取值范围及含义在下表中给出
* fontScale：字体的大小
* bottomLeftOrigin：图像数据原点的位置。默认为左上角；如果参数改为true，则原点为左下角

​                                                                         **fontFace字体类型及含义**

| 标志                        | 简记 | 含义                         |
| --------------------------- | :--: | ---------------------------- |
| FONT_HERSHEY_SIMPLEX        |  0   | 正常大小的无衬线字体         |
| FONT_HERSHEY_PLAIN          |  1   | 小尺寸的无衬线字体           |
| FONT_HERSHEY_DUPLEX         |  2   | 正常大小的较复杂的无衬线字体 |
| FONT_HERSHEY_COMPLEX        |  3   | 正常大小的衬线字体           |
| FONT_HERSHEY_TRIPLEX        |  4   | 正常大小的较复杂的衬线字体   |
| FONT_HERSHEY_COMPLEX_SMALL  |  5   | 小尺寸的衬线字体             |
| FONT_HERSHEY_SCRIPT_SIMPLEX |  6   | 手写风格的字体               |
| FONT_HERSHEY_SCRIPT_COMPLEX |  7   | 复杂的手写风格字体           |
| FONT_ITALIC                 |  16  | 斜体字体                     |

示例：

```c++
#include <opencv2\opencv.hpp>
#include <iostream>

using namespace cv;
using namespace std;

int main()
{
    Mat img = Mat::zeros(Size(512, 512), CV_8UC3);  //生成一个黑色图像用于绘制几何图形
    //绘制圆形
    circle(img, Point(50, 50), 25, Scalar(255, 255, 255), -1);  //绘制一个实心圆
    circle(img, Point(100, 50), 20, Scalar(255, 255, 255), 4);  //绘制一个空心圆
    //绘制直线
    line(img, Point(100, 100), Point(200, 100), Scalar(255, 255, 255), 2, LINE_4,0);
    //绘制椭圆
    ellipse(img, Point(300,255), Size(100, 70), 0, 0, 100, Scalar(255,255,255), -1);
    ellipse(img, RotatedRect(Point2f(150,100), Size2f(30,20), 0), Scalar(0,0,255),2);
    vector<Point> points;
     //用一些点来近似一个椭圆
    ellipse2Poly(Point(200, 400), Size(100, 70),0,0,360,2,points);
    for (int i = 0; i < points.size()-1; i++)  //用直线把这个椭圆画出来
    {
        if (i==points.size()-1)
        {
        //椭圆中后于一个点与第一个点连线
        line(img, points[0], points[i], Scalar(255, 255, 255), 2);
        break;
        }
      //当前点与后一个点连线
      line(img, points[i], points[i+1], Scalar(255, 255, 255), 2);
    }
    //绘制矩形
    rectangle(img, Point(50, 400), Point(100, 450), Scalar(125, 125, 125), -1);
    rectangle(img, Rect(400,450,60,50), Scalar(0, 125, 125), 2);
    //绘制多边形
    Point pp[2][6];
    pp[0][0] = Point(72, 200);
    pp[0][1] = Point(142, 204);
    pp[0][2] = Point(226, 263);
    pp[0][3] = Point(172, 310);
    pp[0][4] = Point(117, 319);
    pp[0][5] = Point(15, 260);
    pp[1][0] = Point(359, 339);
    pp[1][1] = Point(447, 351);
    pp[1][2] = Point(504, 349);
    pp[1][3] = Point(484, 433);
    pp[1][4] = Point(418, 449);
    pp[1][5] = Point(354, 402);
    Point pp2[5];
    pp2[0] = Point(350, 83);
    pp2[1] = Point(463, 90);
    pp2[2] = Point(500, 171);
    pp2[3] = Point(421, 194);
    pp2[4] = Point(338, 141);
    const Point* pts[3] = { pp[0],pp[1],pp2 };  //pts变量的生成
    int npts[] = { 6,6,5 };  //顶点个数数组的生成
    fillPoly(img, pts, npts, 3, Scalar(125, 125, 125),8);  //绘制3个多边形
    //生成文字
    putText(img, "Learn OpenCV 4",Point(100, 400), 2, 1, Scalar(255, 255, 255));
    imshow("", img);
    waitKey(0);
    return 0;
}
```

#### 2.5感兴趣区域

有时我们只对一张图像中的部分区域感兴趣，而原图像又比较大，如果带着非感兴趣区域一起处理会占用大量的内存，因次我们希望从原图像中截取部分图像后再进行处理。我们将这个区域称为感兴趣区域（Region Of Interest， ROI）

OpenCV4提供了两种截取ROI的方式

 从原图中截取部分内容，就是将需要截取的部分在原图像中标记出来，可以用Rect数据结构标记，也可以用Range数据结构标记，这两种数据结构在下面给出。

```c++
Rect_(_Tp _x, _Tp _y, _Tp _width, _Tp _height)
 
cv::Range(int start, int end)
```

* __Tp：一种数据类型。c++模板特性，可以用int，double，float等替换_
* _x：矩形区域左上角第一个像素的x的坐标，也就是第一个像素的列数
* __y：矩形区域左上角第一个像素的y的坐标，也就是第一个像素的行数_
* _width：矩形的宽，单位为像素，即矩形区域跨越的列数
* _height：矩形的高，单位为像素，即矩形区域跨越的行数
* start：区间的起始
* end：区间的结束

例如在img中截取图像，可以img（Rect（p.x，p.y，width，height））

Range只是表明一个区间范围，img（Range（rows_start，rows_end），Range（cols_start，cols_end））

OpenCV4中对图像的赋值和拷贝分为浅拷贝和深拷贝，浅拷贝是建立一个可以访问图像数据的变量

深拷贝是将图像数据再复制一份

 在图像处理的过程中OpenCV中提供了copyTo()函数实现两类方法（其中在Mat类中定义的copyTo()有两种重载方式）进行深拷贝

**copyTO()函数原型：**

```c++
void cv::Mat::copyTo(OutputArray m) const
 
void cv::Mat::copyTo(OutputArray m,
                     InputArray masl
                     )const
void cv::copyTo(InputArray src,
                OutputArray dst,
                InputArray mask
                )
```

在Mat类中copyTo()函数有两中重载方式，一种是只需要输入一个与原函数具有相同尺寸和类型的Mat类变量或者空的Mat类变量

另一种需要在输入Mat类变量时输入一个掩模矩阵，矩阵只能是CV_8U数据变量，与原图像具有相同的尺寸，通道数可以是1个或0个

示例：

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg");
    Mat noobcv = imread("E:\\CLion\\opencv_xin\\SG\\SG_1.jpg");

    if (img.empty() || noobcv.empty())
    {
        cout << "Error" << endl;
        return -1;
    }
    Mat ROI1,ROI2,ROI2_copy,mask,img2,img_copy,img_copy2;
    resize(noobcv,mask,Size(200,200));
    img2 = img; //浅拷贝
    //深拷贝的两种方式
    img.copyTo(img_copy2);
    copyTo(img,img_copy,img);
    //两种在图中截取ROI的方式
    Rect rect(206,206,200,200); //定义ROI
    ROI1 = img(rect); //截图
    //ROI是浅拷贝，在后面深拷贝改变了ROI1，原图像img随之改变
    ROI2 = img(Range(300,500),Range(300,500)); //第二种截图方式
    img(Range(300,500),Range(300,500)).copyTo(ROI2_copy); //深拷贝
    mask = img(rect);
    mask.copyTo(ROI1); //在图像中加入部分图像
    imshow("加入noobcv后图像",img);
    imshow("ROI1对ROI2的影响",ROI2);
    imshow("深拷贝的ROI2_copy",ROI2_copy);
    circle(img,Point(300,300),20,Scalar(0,0,255),-1);
    imshow("浅拷贝的img2",img2);
    imshow("深拷贝的img_copy",img_copy);
    imshow("深拷贝的img_copy2",img_copy2);
    imshow("画图对ROI1的影响",ROI1);
    waitKey(0);
    return 0;
}
```

#### 2.6图像“金字塔”

图像“金字塔”是通过多个分辨率表示图像的一种有效且简单的结构

一个图像“金字塔”一系列以金字塔排列、分辨率逐步降低的图像集合

图像“金字塔”的底部是待处理图像的高分辨率，顶部是低分辨率的表示

图像“金字塔”种比较著名的两种——高斯“金字塔”和拉普拉斯“金字塔”

##### 2.6.1高斯金字塔pyrDowm()

高斯“金字塔”的底层为图像的原图，每往上一层就会通过下采样缩小一次图像的尺寸

通常情况下，尺寸会缩小为原来的一半，有特殊需求，缩小的尺寸也可以根据时间情况进行调整

OpenCV4中提供了pyrDowm()函数专门用于图像的下采样计算

**pyrDowm()函数原型：**

```c++
void pyrDowm(InputArray src,
             OutputArray dst,
             const Size& dstsize = Size(),
             int borderType = BORDER_DEFAULT
             )
```

* src：输入待下采样的图像
* dst：输出下采样后的图像，图像的尺寸可以指定，但是数据类型和通道数与src相同
* dstsize：输出图像尺寸，可以默认
* borderType：像素边界外推方法的标志

​                                                                                     **边界外推方法标志**

| 方法标志           | 简记 | 作用                                              |
| ------------------ | :--: | ------------------------------------------------- |
| BORDER_CONSTANT    |  0   | 用特定值填充，如iiiiii\|abcdefgh\|iiiiii          |
| BORDER_REPLICATE   |  1   | 两端复制填充，如aaaaaa\|abcdefgh\|bbbbbb          |
| BORDER_REFLECT     |  2   | 倒序填充，如fedcba\|abcdefgh\|hgfedcb             |
| BORDER_WRAP        |  3   | 正序填充，如cdefgh\|abcdefgh\|abcdefg             |
| BORDER_REFLECT_101 |  4   | 不含边界值的倒序填充，如gfedcb\|abcdefgh\|gfedcba |
| BORDER_TRANSPARENT |  5   | 随机填充，uvwxyz\|abcdefgh\|ijklmno               |
| BORDER_REFLECT101  |  4   | 与 BORDER_REFLECT_101 相同                        |
| BORDER_DEFAULT     |  4   | 与 BORDER_REFLECT_101 相同                        |
| BORDER_ISOLATED    |  16  | 不关心感兴趣区域之外的部分                        |

该函数的功能与resize()函数将图像尺寸缩小一样，但是使用的内部算法不同

##### 2.6.2拉普拉斯“金字塔”pyrUp()

拉普拉斯“金字塔”与高斯“金字塔”正好相反，高斯“金字塔"通过底层图像构建上层图像

而拉普拉斯”金字塔“是通过上层小尺寸的图像构建下层大尺寸的图像

拉普拉斯”金字塔“具有预测残差的作用，需要与高斯”金字塔“联合使用

高斯”金字塔“第i-1层图像的下采样与高斯”金字塔“第i层上采样的差值图像作为拉普拉斯”金字塔“第i层图像

对于上采样操作，OpenCV4中提供pyrUp()函数

**pyrUp()函数原型：**

```c++
void pyrUp(InputArray src,
           OutputArray dst,
           const Szie& dstsize = Szie()
           int borderType = BORDER_DEFAULT
           )
```

* src：输入待下采样的图像
* dst：输出下采样后的图像，图像的尺寸可以指定，但是数据类型和通道数与src相同
* dstsize：输出图像尺寸，可以默认
* borderType：像素边界外推方法的标志

完成高斯”金字塔“的构建后，从上到下取出高斯”金字塔“中的每一次图像

如果是高斯”金字塔“中最上面的一层，则先下采样再上采样，将下采样与上采样的差值作为拉普拉斯”金字塔"的最上层一层

如果从高斯”金字塔“取出的第i层不是最上层，则直接对高斯金字塔中第i+1层图像进行上采样，计算与高斯”金字塔“第i层图像差值

将差值图像作为高斯”金字塔“的第i层

示例：

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
#include <string>
using namespace std;
using namespace cv;
int main()
{
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg");
    if (img.empty()) {
        cout << "Error" << endl;
        return -1;
    }
    vector<Mat> Gauss,Lap; // 高斯”金字塔“和拉普拉斯：金字塔”
    int level = 3; //高斯：金字塔“下采样次数
    Gauss.push_back(img); //将原图作为高斯”金字塔“的第0层
    //构建高斯”金字塔“
    for (int i = 0; i < level; ++i)
    {
        Mat gauss;
        pyrDown(Gauss[i],gauss,Size(Gauss[i].rows/2,Gauss[i].cols/2)); //下采样
        Gauss.push_back(gauss);
    }
    //构建拉普拉斯”金字塔“
    for (int i = Gauss.size() - 1 ; i > 0 ; i--)
    {
        Mat lap,upGauss;
        if(i == Gauss.size() - 1) //如果是高斯”金字塔“的最上面一层图像
        {
            Mat down;
            pyrDown(Gauss[i],down); //下采样
            pyrUp(down,upGauss,Size(Gauss[i].rows,Gauss[i].cols));
            lap = Gauss[i] - upGauss;
            Lap.push_back(lap);
        }
        pyrUp(Gauss[i],upGauss,Size(Gauss[i-1].rows,Gauss[i-1].cols));
        lap = Gauss[i-1] - upGauss;
        Lap.push_back(lap);
    }
    //查看两个图像”金字塔“中的图像
    for (int i = 0; i < Gauss.size(); i++)
    {
        string name = to_string(i);
        imshow("G" + name,Gauss[i]);
        imshow("L" + name,Lap[i]);
    }
    waitKey(0);
    return 0;
}
```

$\textcolor{red}{注意:一定要有相同的尺寸和数据类型}$

#### 2.7窗口交互操作

交互操作可以实现在程序运行过程中改变参数数值的作用

避免重复运行程序，节省时间，同时能够增强结果的对比效果

##### 2.7.1图像窗口滑动条createTrackbar()

OpenCV4提供了createTrackbar()函数实现在显示图像的窗口中创建滑动条

**createTrackbar()函数原型：**

```c++
int createTrackbar(const String& trackbarname,
                   const String& winname,
                   int* value,
                   int count,
                   TrackbarCallback onChange = 0,
                   void* userdata = 0
                   )
```

* trackbarname：滑动条的名称

* winname：创建滑动条窗口的名称

* value：指向整数变量的指针，该指针指向的值反映滑块的位置，创建后，滑动位置由此变量定义

* count：滑动条的最大值

* onChange：每次滑块更改位置时要调用的函数的指针，函数原型为void Foo(int,void*);

  其中第一个参数是轨迹栏位置，第二个参数是用户数据，如果回调是NULL指针，则不会调用任何回调，而只是更新数值

* userdata：传递给回调函数的可选参数

该函数能够在图像窗口上方创建一个从0开始的整数滑动条，滑动条默认只能输出整数，因此，如果需要小数，就需要进行后续处理

例如输出值/10得到含有一位小数的数据

该函数第一个参数是滑动条的名称，第二个参数是创建滑动条的图像窗口的名称，第三个参数是指向整数变量的指针

该指针指向的值反映滑块的位置，创建滑动条时，该参数确定了滑块的初始位置，滑动条创建完成后，指针指向的整数随滑块移动而改变

第四个参数是滑块条的最大取值，第五个参数是每次滑块更改位置时要调用的函数的指针，最后一个是传递给回调函数的void*类型数据

如果使用的第三个参数是全局变量，则可以不用修改最后一个参数，使用参数的默认值就可以

示例：

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
//为了能在被调用函数中使用，设置成全局
int value;
static void callBack(int,void*); //滑动条回调函数
Mat img1,img2;
int main()
{
    img1 = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg");
    if (img1.empty()) {
        cout << "Error" << endl;
        return -1;
    }
    namedWindow("滑动条改变图像亮度");
    imshow("滑动条改变图像亮度",img1);
    value = 100; //滑动条创建时的初始值
    //创建滑动条
    createTrackbar("亮度值百分比","滑动条改变图像亮度",&value,600,callBack,0);
    waitKey(0);
}
static void callBack(int,void*)
{
    float a = value/100.0;
    img2 = img1*a;
    imshow("滑动条改变图像亮度",img2);
}
```

##### 2.7.2鼠标响应setMouseCallback()

OpenCV4提供了鼠标响应相关函数setMouseCallback()函数

**setMouseCallback()函数原型：**

```c++
void setMouseCallback(const String& winname,
                      MouseCallback onMouse,
                      void * userdata = 0
                      )
```

* winname：添加鼠标响应的窗口的名字
* onMouse：鼠标响应的回调函数
* userdata：传递给回调函数的可选参数

该函数能够为指定的图像窗口创建鼠标响应，第一个参数是需要创建鼠标响应的图像窗口的名字

第二个参数为鼠标响应的回调函数，该函数是鼠标状态发生改变时调用，是MouseCallback类型函数

最后一个参数是传递给回调函数的可选参数，一般情况下，使用默认值0即可

**MouseCallback()回调函数原型：**

```c++
typedef void(*cv::MouseCallback)(int event,
                                 int x,
                                 int y,
                                 int flags,
                                 void *userdata
                                 )
```

* event：鼠标响应事件标志，参数为EVENT_*形式
* x：鼠标指针在图像坐标系中的x坐标
* y：鼠标指针在图像坐标系中的y坐标
* flags：鼠标响应标志，参数为EVENT_FLAG_*形式
* userdata：传递给回调函数的可选参数

MouseCallback类型的回调函数是一个无返回值的函数，函数名可以任意设置

最后一个参数是传递给回调函数的可选参数，一般情况下，使用void*默认即可

​                                                          **MouseCallback类型回调函数鼠标响应事件标志**

| 标志参数            | 简记 | 含义                               |
| ------------------- | :--: | ---------------------------------- |
| EVENT_MOUSEMOVE     |  0   | 表示鼠标指针在窗口上移动           |
| EVENT_LBUTTONDOWN   |  1   | 表示按下鼠标左键                   |
| EVENT_RBUTTONDOWN   |  2   | 表示按下鼠标右键                   |
| EVENT_MBUTTONDOWN   |  3   | 表示按下鼠标中键                   |
| EVENT_LBUTTONUP     |  4   | 表示释放鼠标左键                   |
| EVENT_RBUTTONUP     |  5   | 表示释放鼠标右键                   |
| EVENT_MBUTTONUP     |  6   | 表示释放鼠标中键                   |
| EVENT_LBUTTONDBLCLK |  7   | 表示双击鼠标左键                   |
| EVENT_RBUTTONDBLCLK |  8   | 表示双击鼠标右键                   |
| EVENT_MBUTTONDBLCLK |  9   | 表示双击鼠标中键                   |
| EVENT_MOUSEWHEEL    |  10  | 正值表示向前滚动，负值表示向后滚动 |
| EVENT_MOUSEHWHEEL   |  11  | 正值表示向左滚动，负值表示向右滚动 |

​                                                          **MouseCallback类型回调函数鼠标响应标志**

| 标志参数            | 简记 | 含义         |
| ------------------- | :--: | ------------ |
| EVENT_FLAG_LBUTTON  |  1   | 按住左键拖拽 |
| EVENT_FLAG_RBUTTON  |  2   | 按住右键拖拽 |
| EVENT_FLAG_MBUTTON  |  4   | 按住中键拖拽 |
| EVENT_FLAG_CTRLKEY  |  8   | 按“Ctrl”键   |
| EVENT_FLAG_SHIFTKEY |  16  | 按“Shift”键  |
| EVENT_FLAG_ALTKEY   |  32  | 按“Alt”键    |

鼠标响应就是当鼠标位于对应的图像窗口内时，时刻检测鼠标状态，当鼠标状态发生改变时，调用回调函数的判断逻辑选择相应的操作

示例程序：如果鼠标右键被按下，就会提示“点击鼠标左键才可以绘制轨迹”，若单击左键，就会输出当前鼠标的坐标

​				   并将该点坐标定义为某段轨迹的起始位置。之后按住左键移动鼠标，会绘制鼠标的移动轨迹

该程序中提供了两种绘制轨迹的方式：

第一种是每次调用回调函数获得鼠标位置时更改周围图像像素值，但是由于回调函数有一定执行时间

当鼠标移动过快，就会出现断点

第二种是在前一时刻和当前时刻鼠标位置间绘制直线，可以避免因鼠标移动过快出现断点的问题

示例：

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
Mat img,imgPoint; //全局的图像
Point prePoint; //前一时刻鼠标的坐标，用于绘制直线

void mouse(int event,int x,int y,int flags, void*);
int main()
{
    img = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg");
    if (img.empty()) {
        cout << "Error" << endl;
        return -1;
    }
    img.copyTo(imgPoint);
    imshow("图像窗口1",img);
    imshow("图像窗口2",imgPoint);
    setMouseCallback("图像窗口1",mouse,0); //鼠标响应
    waitKey(0);
    return 0;
}
void mouse(int event,int x,int y,int flags, void*)
{

    if (event == EVENT_RBUTTONDOWN) //单击右键
    {
        cout << "点击鼠标左键才可以绘制轨迹" << endl;
    }
    if (event == EVENT_LBUTTONDOWN) //单击左键，输出坐标
    {
        prePoint = Point(x,y);
        cout << "轨迹起始坐标" << prePoint << endl;
    }
    if (flags == EVENT_FLAG_LBUTTON && event == EVENT_MOUSEMOVE ) //按住鼠标左键移动
    {
        //通过改变图像像素显示鼠标移动轨迹
        imgPoint.at<Vec3b>(y,x) = Vec3b(0,0,255);
        imgPoint.at<Vec3b>(y,x-1) = Vec3b(0,0,255);
        imgPoint.at<Vec3b>(y,x+1) = Vec3b(0,0,255);
        imgPoint.at<Vec3b>(y+1,x) = Vec3b(0,0,255);
        imgPoint.at<Vec3b>(y+1,x) = Vec3b(0,0,255);
        imshow("图像窗口2",imgPoint);
        //通过绘制直线显示鼠标移动轨迹
        Point pt(x,y);
        line(img,prePoint,pt,Scalar(0,0,255),2,5,0);
        prePoint = pt;
        imshow("图像窗口1",img);
    }
}
```

Opencv提供了直线函数line()

**line()函数原型：**

```c++
void line(Mat& img,
          Point pt1,
          Point pt2,
          const Scalar& color,
          int thickness=1,
          int lineType=8,
          int shift=0
          )
```

* img: 要绘制线段的图像。
* pt1: 线段的起点。
* pt2: 线段的终点。
* color: 线段的颜色，通过一个Scalar对象定义。
* thickness: 线条的宽度。
* lineType: 线段的类型。可以取值8， 4， 和CV_AA， 分别代表8邻接连接线，4邻接连接线和反锯齿连接线。默认值为8邻接。为了获得更好地效果可以选用CV_AA(采用了高斯滤波)。
* shift: 坐标点小数点位数

## 图像直方图与模板匹配

### 1.图像直方图的绘制calcHist()

图像直方图是图像处理中非常重要的像素统计结果

由于同一物体无论是旋转还是平移，在图像中都具有相同的灰度值

因此直方图具有平移不变性、放缩不变性等优点，可以查看图像的整体变化形式，例如图像是否过暗、图像像素灰度值主要集中范围

在特定条件下，也可以利用图像直方图进行图像的识别，例如对数字的识别

简单来说，图像直方图就是统计图像中每一个灰度值的个数

之后将图像灰度值作为横轴，灰度值个数或者灰度值所占比率作为纵轴绘制直方图

通过直方图，可以看出图像中哪些灰度值数目较多，哪些较少，可以通过一定的方法将灰度值较为集中的区域映射到较为稀疏的区域

从而使图像在像素在像素值上的更加分布符合期望状态

通常情况下，像素灰度值表示亮暗程度，因此，可以通过图像直方图，分析图像亮暗对比度，并调整图像的亮暗程度



OpenCV4提供了图像直方图的统计函数calcHist()，该函数可以统计出图像中每个灰度值的个数

**calcHist()函数原型：**

```c++
void calcHist(const Mat* images,
              int nimages,
              const int* channels,
              InputArray mask,
              OutputArray hist,
              int dims,
              const int* histSize,
              const float** ranges,
              bool uniform = ture,
              bool accumulate = false
              )
```

* images：待统计直方图的图像数组，是一个MAT类型的数组，数组中所有的图像应具有相同的尺寸和数据类型，并且数据类型只能             		         是CV_8U、CV_16U和CV_32F三种中的一种，但是不同图像的通道数可以不同

* nimages：待统计直方图的图像数量

* channels：需要参与形成多维直方图的通道索引数组，第一个图像的通道索引从0到images[0].channels()-1，第二个图像通道索引从 

  ​                    images[0].channels()到images[0].channels()+ images[1].channels()-1，以此类推

  ​					假如有三张图像，都是三通道。那么第一张图像三个通道的索引值分别为0、1、2；第二张图像的三个通道的索引值分别					为3、4、5；第三张图像的三个通道的索引值分别为6、7、8

  ​					现在我们要计算一个二维直方图，参与计算的通道为第一张图像的第一通道和第三张图像的第三通道，那么数组channels					的定义和初始化如下：

  ```cpp
  int channels[2]={0,8}
  ```

* mask： 可选的操作掩码，如果是空矩阵则表示图像中所有位置的像素都计入直方图中，如果矩阵不为空，则必须与输入图像尺寸

  ​               相同且数据类型为CV_8U。当不为空的时候，那些掩码值不为0的掩码对应的像素被纳入统计范围，而那些掩码值为0的掩码           

  ​               对应的像素则不被纳入统计

* hist：存储直方图统计结果，是一个dims维度的数组
* dims：需要计算的直方图的维度，必须是整数，并且不能大于CV_MAX_DIMS，在OpenCV3和OpenCV 4.0中都为32

* histSize：每个维度的直方图尺寸

* ranges：每个维度的灰度值取值范围

* uniform：直方图是否均匀划分的标志符，默认状况下为均值（ture）
* accumulate：是否累加统计直方图的标志，如果累积（ture），那么在统计新图像的直方图时，之前图像的统计结果不会被

​								统计，该参数主要用于统计多个图像整体的直方图

该函数用于统计图像中每个灰度值像素个数，例如统计一幅CV_8UC1的图图像，需要统计从0到255中每一个灰度值在图像中的像素个数

如果某个灰度值在图像中没有，那么该灰度值的统计结果为0

示例程序：使用calcHsit()函数统计灰度图像中每个灰度值的数目，之后通过不断绘制矩阵的方式实现直方图的绘制

由于图像中部分灰度值数目较多，因此将每个灰度值缩小为原来的1/20后再进行绘制



OpenCV4提供了四舍五入的取整函数cvRound()

该函数输入参数double类型的变量，返回值为该变量四舍五入后的int型数值

示例：

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg");
    if (img.empty()) {
        cout << "Error" << endl;
        return -1;
    }
    Mat gray;
    cvtColor(img,gray,COLOR_BGR2GRAY);
    //设置提取直方图的相关变量
    Mat hist;
    const int channels[1] = { 0 };  //通道索引
    float inRanges[2] = {0,255};
    const float * ranges[1] = { inRanges }; //像素灰度值范围
    const int bins[1] = { 256 }; //直方图的维度，其实就是像素灰度值的最大值
    calcHist(&gray,1,channels,Mat(),hist,1,bins,ranges); //计算图像直方图
    //绘制直方图
    int hist_w = 512;
    int hist_h = 400;
    int width = 2;
    Mat histImage = Mat::zeros(hist_h,hist_w,CV_8UC3);
    for (int i = 0; i <= hist.rows; ++i)
    {
        rectangle(histImage,
                  Point(width*(i-1),hist_h - 1),
                  Point(width*i - 1,hist_h - cvRound(hist.at<float>(i-1)/15 ) ),
                  Scalar(255,255,255),
                  -1
                  );
    }
    namedWindow("histImage",WINDOW_AUTOSIZE);
    imshow("histImage",histImage);
    imshow("gray",gray);
    waitKey(0);
    return 0;
}
```

矩形绘制函数：（2.4.4）

```
void cv::rectangle(InputOutArray img,
                   Point pt1,
                   Point pt2,
                   const Sclar& color,
                   int thickness = 1,
                   int lineType = LINE_8,
                   int shift = 0
                   )
```

### 2.直方图操作

主要介绍如何将直方图归一化，以及如何比较两个图像的直方图

#### 2.1直方图归一化normalize()

为了让数据可以完整的展示在图像中，绘制时会将统计数据缩小

另外，由于像素值统计数目与图像的尺寸具有直接关系，因此，如果以灰度值数目作为最终统计结果

那么一幅图像缩放后的直方图将有巨大的差异

理论上来说，通过对同一图像缩放之后的两幅图像的直方图应该具有大致相同的直方图分布特征

因此用灰度值的数目作为统计结果具有一定的局限性

1.图像的像素灰度值统计结果可以用灰度值像素的数目占一幅图像的比例来表示（即将统计结果/图像像素个数）实现归一化

   但是，在CV_8U类型的图像中，灰度值有256个等级，平均每个像素的灰度值所占比例为0.39%，比例非常低

   因此，为了更直观地绘制直方图，经常需要将比例扩大一定倍数再绘制直方图

2.寻找统计结果中的最大值，把所有结果除于最大值，实现所有数据归一化 0~1



对于上面两种归一化方法，OpenCV4提供了normalize()函数实现多种形式归一化功能

**normalize()函数原型：**

```c++
void normalize(InoutArray src,
               InputOutputArray dst,
               double alpha = 1,
               double beta = 0,
               int norm_type = NORM_L2,
               InputArray mask = noArray()
               )
```

* src：输入数组矩阵
* dst：输入与src相同大小的数组矩阵
* alpha：在范围归一化的情况下，归一化到下限边界的标准值
* beta：范围归一化时的上限范围，不用于标准规范化
* norm_type：归一化过程中数据范数种类标志
* dtype：输出数据类型标志，如果为-1，那么输出数据于src拥有相同的类型，否则于src具有相同的通道数，但数据类型不同
* mask：掩码矩阵

该函数输入一个存放数据的矩阵，通过参数alpha设置将数据缩放到最大范围，然后通过norm_type参数选择计算范数的种类

之后将输入矩阵中的每个数据分别除以求取的范数数值，最后得到缩放的结果，输出结果是一个CV_32F类型矩阵

可以将输入矩阵作为输出矩阵，或者重新定义一个新的矩阵用于存放输出结果

**NORM_L1**标志：输出结果为每个灰度值所占的比例

**NORM_INF**标志：输出结果为除以数据最大值，将所有数据归一化为0~1

​                                                                   **normalize()函数归一化常用标志参数**

| 标志参数    | 简记 | 作用                           |
| ----------- | :--: | ------------------------------ |
| NORM_INF    |  1   | 无穷范数，向量最大值           |
| NORM_L1     |  2   | L1范数，绝对值之和             |
| NORM_L2     |  4   | L2范数及模长归一化，平方和之根 |
| NORM_L2SQR  |  5   | L2范数平方                     |
| NORM_MINMAX |  32  | 偏移归一化                     |

示例：

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
#include <vector>
using namespace std;
using namespace cv;
int main()
{
    vector<double> positiveData = {2.0,8.0,10.0};
    vector<double> normalized_L1,normalized_L2,normalized_Inf,normalized_MINMAX;
    //测试不同归一化方法
    normalize(positiveData,normalized_L1,1.0,0.0,NORM_L1); //绝对值求和归一化
    cout <<"normalized_L1=[" << normalized_L1[0] <<", "
          << normalized_L1[1] << "， " << normalized_L1[2] << "]" << endl;
    normalize(positiveData,normalized_L2,1.0,0.0,NORM_L2); //模长求和归一化
    cout <<"normalized_L2=[" << normalized_L2[0] <<", "
         << normalized_L2[1] << "， " << normalized_L2[2] << "]" << endl;
    normalize(positiveData,normalized_Inf,1.0,0.0,NORM_INF); //最大值求和归一化
    cout <<"normalized_Inf=[" << normalized_Inf[0] <<", "
         << normalized_Inf[1] << "， " << normalized_Inf[2] << "]" << endl;
    normalize(positiveData,normalized_MINMAX,1.0,0.0,NORM_MINMAX); //偏移求和归一化
    cout <<"normalized_MINMAX=[" << normalized_MINMAX[0] <<", "
         << normalized_MINMAX[1] << "， " << normalized_MINMAX[2] << "]" << endl;

    //将图像直方图归一化
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg");
    if (img.empty()) {
        cout << "Error" << endl;
        return -1;
    }
    Mat gray,hist;
    cvtColor(img,gray,COLOR_BGR2GRAY);
    const int channels[1] = { 0 };  //通道索引
    float inRanges[2] = {0,255};
    const float * ranges[1] = { inRanges }; //像素灰度值范围
    const int bins[1] = { 256 }; //直方图的维度，其实就是像素灰度值的最大值
    calcHist(&gray,1,channels,Mat(),hist,1,bins,ranges); //计算图像直方图
    //绘制直方图
    int hist_w = 512;
    int hist_h = 400;
    int width = 2;
    Mat histImage_L1 = Mat::zeros(hist_h,hist_w,CV_8UC3);
    Mat histImage_Inf = Mat::zeros(hist_h,hist_w,CV_8UC3);
    Mat hist_L1,hist_Inf;
    normalize(hist, hist_L1, 1, 0, NORM_L1, -1, Mat());
    for (int i = 1; i <= hist_L1.rows; ++i)
    {
        rectangle(histImage_L1,
                  Point(width*(i-1),hist_h - 1),
                  Point(width*i - 1,hist_h - cvRound(30*hist_h*hist_L1.at<float>(i-1))-1),
                  Scalar(255,255,255),
                  -1
        );
    }
    normalize(hist, hist_Inf, 1, 0, NORM_INF, -1, Mat());
    for (int i = 1; i <= hist_Inf.rows; ++i)
    {
        rectangle(histImage_Inf,
                  Point(width*(i-1),hist_h - 1),
                  Point(width*i - 1,hist_h - cvRound(hist_h*hist_Inf.at<float>(i-1))-1 ),
                  Scalar(255,255,255),
                  -1
        );
    }
    imshow("histImage_L1",histImage_L1);
    imshow("histImage_Inf",histImage_Inf);
    waitKey(0);
    return 0;
}
```

#### 2.2直方图比较compareHist()

由于图像的直方图表示图像的像素灰度值的统计特性，因此可以通过两幅图像的直方图特性比较两幅图像的相似程度

虽然两幅图像的直方图分布相似不代表两幅图像相似

但是两幅图像相似则两幅图像的直方图一定相似

例如，对图像进行缩放之后，虽然图像的直方图不会完全一致，但是具有高度的相似性

因此，可以通过比较两幅图像的直方图分布相似性对图像进行初步的筛选和识别

OpenCV4提供了比较两个图像直方图相似性的compareHist()函数

**compareHist()函数原型：**

```c++
double compareHist(InputArray H1,
                   InputArray H2,
                   int method
                   )
```

* H1：第一幅图像直方图
* H2：第二幅图像直方图，与H2具有相同的尺寸
* method：比较方法标志

该函数前两个参数为需要比较的相似性的图像直方图，由于不同尺寸的图像的像素数目可能不同

为了能够得到两个图像直方图正确的相似性，需要输入同一种方式归一化后图像直方图

并且要求两个图像直方图具有相同的尺寸，第三个参数为比较相似性的方法

​                                                    **compareHist()函数比较直方图方法的可选择标志参数**

| 标志参数              | 简记 | 名域或作用                      |
| --------------------- | :--: | ------------------------------- |
| HISTCMP_CORREL        |  0   | 相关法                          |
| HISTCMP_CHISQR        |  1   | 卡方法                          |
| HISTCMP_INTERSECT     |  2   | 直方图相交法                    |
| HISTCMP_BHATTACHARYYA |  3   | 巴氏距离法                      |
| HISTCMP_HELLINGER     |  3   | 与HISTCMP_BHATTACHARYYA方法相同 |
| HISTCMP_CHISQR_ALT    |  4   | 替代法方法                      |
| HISTCMP_KL_DIV        |  5   | 相对熵法                        |

**1.HISTCMP_CORREL**

​		相关法，如果两个直方图完全相同，那么计算数值为1，如果完全不相关，计算值为0

**2.HISTCMP_CHISQR**

​		卡方法，如果两个直方图完全相同，那么计算数值为0，相似性越小，计算数值越大

**3.HISTCMP_INTERSECT**

​		直方图相交法，该方法不会将计算结果归一化，因此，即使是完全一致的图像直方图，来自不同图像，也会有不同的数值

例如：两个图像分别缩放之后的直方图相似性结果可能不一样，但是任意图像的直方图与一副图像的直方图相比较，数值越大

相似性越高，数值越小，相似性越低

**4.HISTCMP_BHATTACHARYYA**

​		巴塔恰里雅距离（巴氏距离）法，如果两个直方图完全相同，那么计算数值为0，相似性越小，计算数值越大

**5.HISTCMP_CHISQR_ALT**

​		替代法方法，相似性比较方法与**巴氏距离法**相同，常用于替代巴氏距离法用于纹理比较

**6.HISTCMP_KL_DIV**

​		相对熵法，又名Kullback-Leibler散度法，如果两个直方图完全相同，那么计算数值为0，相似性越小，计算数值越大

示例：

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
void drawHist(Mat &hist, int type, string name)  //归一化并绘制直方图函数
{
    int hist_w = 512;
    int hist_h = 400;
    int width = 2;
    Mat histImage = Mat::zeros(hist_h,hist_w,CV_8UC3);
    normalize(hist, hist, 1, 0, type, -1, Mat());
    for (int i = 1; i <= hist.rows; ++i)
    {
        rectangle(histImage,
                  Point(width*(i-1),hist_h - 1),
                  Point(width*i - 1,hist_h - cvRound(hist_h*hist.at<float>(i-1))-1),
                  Scalar(255,255,255),
                  -1
        );
    }
    imshow(name,histImage);
}
int main() {
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg");
    if (img.empty()) {
        cout << "Error" << endl;
        return -1;
    }
    Mat gray,hist,gray2,hist2,gray3,hist3;
    cvtColor(img,gray,COLOR_BGR2GRAY);
    resize(gray,gray2,Size(),0.5,0.5);
    gray3 = imread("E:\\CLion\\opencv_xin\\SG\\SG_1.jpg",IMREAD_GRAYSCALE);
    const int channels[1] = { 0 };  //通道索引
    float inRanges[2] = {0,255};
    const float * ranges[1] = { inRanges }; //像素灰度值范围
    const int bins[1] = { 256 }; //直方图的维度，其实就是像素灰度值的最大值
    //计算图像直方图
    calcHist(&gray,1,channels,Mat(),hist,1,bins,ranges);
    calcHist(&gray2,1,channels,Mat(),hist2,1,bins,ranges);
    calcHist(&gray3,1,channels,Mat(),hist3,1,bins,ranges);
    drawHist(hist,NORM_INF,"hist");
    drawHist(hist2,NORM_INF,"hist2");
    drawHist(hist3,NORM_INF,"hist3");
    //原图直方图与原图直方图的相关系数
    double hist_hist = compareHist(hist,hist,HISTCMP_CORREL);
    cout << "apple_apple=" << hist_hist << endl;
    //原图直方图与缩小原图直方图的相关系数
    double hist_hist2 = compareHist(hist,hist2,HISTCMP_CORREL);
    cout << "apple_apple256=" << hist_hist2 << endl;
    //两幅不同图像直方图相关系数
    double hist_hist3 = compareHist(hist,hist3,HISTCMP_CORREL);
    cout << "apple_apple=" << hist_hist3 << endl;
    waitKey(0);
    return 0;
}
```

### 3.直方图应用

直方图能够表示图像像素的统计特性，同时，应用统计的直方图结果也可以增强图像的对比度

在图像中寻找相似区域

#### 3.1直方图均衡化equalizeHist()

如果一个直方图都集中在一个区域，那么整体图像的对比度比较小，不便于图像中纹理的识别

例如，如果相邻的两个像素灰度值分别是120和121，那么凭肉眼无法区分

同时，如果图像中所有的像素灰度值都集中在100~150，那么整个图像会给人模糊的感觉

如果通过映射关系，将图像中灰度值的范围扩大，增加原来两个灰度值之间的差值，就可以提供图像的对比度

进而将图像中的纹理突出显现出来，这个过程称为图像直方图均衡化

OpenCV4提供了equalizeHist()函数用于图像均衡化

**equalizeHist()函数原型：**

```cpp
void equalizeHist(InputArray src,
                  OutputArray dst
                  )
```

* src：需要直方图均衡化的CV_8UC1图像
* dst：直方图均衡化后的输出图像，与src具有相同尺寸和数据类型

该函数只能对单通道的灰度图进行直方图均衡化

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
void drawHist(Mat &hist, int type, string name)  //归一化并绘制直方图函数
{
    int hist_w = 512;
    int hist_h = 400;
    int width = 2;
    Mat histImage = Mat::zeros(hist_h,hist_w,CV_8UC3);
    normalize(hist, hist, 1, 0, type, -1, Mat());
    for (int i = 1; i <= hist.rows; ++i)
    {
        rectangle(histImage,
                  Point(width*(i-1),hist_h - 1),
                  Point(width*i - 1,hist_h - cvRound(hist_h*hist.at<float>(i-1))-1),
                  Scalar(255,255,255),
                  -1
        );
    }
    imshow(name,histImage);
}
int main()
{
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\ven.jpg");
    if (img.empty()) {
        cout << "Error" << endl;
        return -1;
    }
    Mat gray,hist,hist2;
    cvtColor(img,gray,COLOR_BGR2GRAY);
    Mat equalImg;
    equalizeHist(gray,equalImg); //将图像直方图均衡化
    const int channels[1] = { 0 };  //通道索引
    float inRanges[2] = {0,255};
    const float * ranges[1] = { inRanges }; //像素灰度值范围
    const int bins[1] = { 256 }; //直方图的维度，其实就是像素灰度值的最大值
    //计算图像直方图
    calcHist(&gray,1,channels,Mat(),hist,1,bins,ranges);
    calcHist(&equalImg,1,channels,Mat(),hist2,1,bins,ranges);
    drawHist(hist,NORM_INF,"hist");
    drawHist(hist2,NORM_INF,"hist2");
    namedWindow("原图",WINDOW_NORMAL);
    namedWindow("均衡化后的图像",WINDOW_NORMAL);
    imshow("原图",gray);
    imshow("均衡化后的图像",equalImg);
    waitKey(0);
    return 0;
}
```

#### 3.2直方图匹配

直方图均衡化函数可以自动地改变图像直方图的分布形式，这种方式极大地简化了直方图均衡化过程中需要的操作步骤

但是，在某些特定情况下，需要将直方图映射成指定的分布形式，这种将直方图映射成指定分布形式的算法称为直方图匹配

或直方图规定化，直方图匹配和直方图均衡化相似，都是对图像直方图分布形式进行改变

但是直方图均衡化后图像是均匀分布的，而直方图匹配后的直方图可以随意指定，即在执行直方图匹配操作时

要知道变换后的灰度直方图分布形式，进而确定比变换函数

不同图像间像素数目可能不同，为了使两个图像直方图能够匹配，需要使用概率形式表示每个灰度值在图像像素种所占的比例

在理想状态下，经过图像直方图匹配操作后，图像直方图分布形式应与目标分布一致，因此两者之间的累积概率分布也一致

累积概率为小于或等于某一灰度值的像素数目占所有像素的比例

#### 3.4直方图反向投影calcBackProject()

反向投影就是首先计算某一特征的直方图模型，然后使用该模型去寻找图像中是否存在该特征的方法

直方图的反向投影常用于对目标的跟踪的定位

OpenCV4提供了calcBackProject()函数用于图像直方图反向投影

**calcBackProject()函数原型：**

```cpp
void calcBackProject(const Mat* images,
                     int nimages,
                     const int* channels,
                     InputArray hist,
                     OutputArray backProject,
                     const float** ranges,
                     double scale = 1,
                     bool uniform = ture
                     )
```

* images：待统计直方图的图像数组，数组中所有的图像应具有相同的尺寸和数据类型

​						并且数据类型只能是CV_8U、CV_16U和CV_32F，但是不同图像的通道数可以不同

* nimages：输入图像数量
* channels：需要统计的通道索引数组
* hist：输入直方图
* backProject：目标为反向投影图像，与images[0]具有相同尺寸和数据类型的单通道图像
* ranges：每个图像通道中的灰度值的取值范围
* scale：输出反向投影矩阵的比例因子
* uniform：直方图是否均匀的标志符，默认状态下为均匀（turn）

该函数用于在输入图像中寻找与特定图像最匹配的点或者区域，即对图像直方图反向投影

该函数需要输入模板图像的直方图统计结果，并且返回的是一幅图像

第一步：加载模板图像和待反向投影图像

第二步：转换图像颜色空间，常用的颜色空间为灰度和HSV空间

第三步：计算模板图像的直方图，灰度图像为一维直方图，HSV图像为H-S通道的二维直方图

第四步：将待反向投影的图像和模板图像的直方图赋值给反向投影函数calcBackProject()，最后等到反向投影结果

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
void drawHist(Mat &hist, int type, string name)  //归一化并绘制直方图函数
{
    int hist_w = 512;
    int hist_h = 400;
    int width = 2;
    Mat histImage = Mat::zeros(hist_h,hist_w,CV_8UC3);
    normalize(hist, hist, 255, 0, type, -1, Mat());
    namedWindow(name,WINDOW_NORMAL);
    imshow(name,hist);
}
int main()
{
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg");
    Mat sup_img = imread("E:\\CLion\\opencv_xin\\SG\\SG_face.jpg");
    Mat img_HSV,sup_HSV,hist,hist2;
    if (img.empty() || sup_img.empty()) {
        cout << "Error" << endl;
        return -1;
    }
    imshow("img",img);
    imshow("sup_img",sup_img);
    //转成HSV空间，提取S、V两个通道
    cvtColor(img,img_HSV,COLOR_BGR2HSV);
    cvtColor(sup_img,sup_HSV,COLOR_BGR2HSV);
    int h_bins = 32;
    int s_bins = 32;
    int histSize[] = { h_bins , s_bins };
    //H通道值的范围为0~179
    float h_ranges[] = {0,180};
    //S通道值的范围为0~255
    float s_ranges[] = {0,256};
    const float *ranges[] = {h_ranges , s_ranges}; //每个通道的范围
    int channels[] = {0,1}; //统计的通道索引
    //绘制H-S二维直方图
    calcHist(&sup_HSV,1,channels,Mat(),hist,2,histSize,ranges, true,false);
    drawHist(hist,NORM_INF,"hist");  //直方图归一化并绘制直方图
    Mat backproj;
    calcBackProject(&img_HSV,1,channels,hist,backproj,ranges,1.0); //图像直方图反向投影
    imshow("BcakProject",backproj);
    waitKey(0);
    return 0;
}
```



#### 3.5图像的模板匹配matchTemplate()

模板匹配常用于在一副图像种寻找特定内容的任务中，由于模板图像的尺寸小于待匹配图像的尺寸，同时又需要比较两幅图像

的每一个像素的灰度值，因此常采用在待匹配图像中选择与模板相同尺寸的滑动窗口，通过比较滑动窗口与模板的相似程度

判断待匹配图像中是否含有与模板图像相同的内容

![image-20221207170348643](C:\Users\小新\AppData\Roaming\Typora\typora-user-images\image-20221207170348643.png)

第一步：在待匹配图像中选取与模板尺寸大小相同的滑动窗口

第二步：比较滑动窗口中每个像素与模板中对应像素灰度值的关系，计算模板与滑动窗口的相似性

第三步：将滑动窗口从左上角开始先向右滑动，滑动到最右边向下滑动一行，然后从最左侧重新开始滑动

​				记录每一次移动后计算的模板与滑动窗口的相似性

第四步：比较所有位置的相似性，选择相似性最大的滑动窗口作为备选匹配结果

OpenCV4提供了用于图像模板匹配的matchTemplate()函数

**matchTemplate()函数原型：**

```cpp
void matchTemplate(InputArray image,
                   InputArray templ,
                   OutputArray result,
                   int method,
                   InputArray mask = noArray()
                   )
```

* image：待模板匹配的源图像，图像数据类型为CV_8U和CV_32F两者中一个
* templ：模板图像，需要与源图像具有相同的数据类型，但是尺寸不能大于原始图像
* result：模板匹配结果输出图像，图像数据类型为CV_32F
* method：是模板匹配方法标志
* mask：匹配模板的掩码，必须与模板图像具有相同的数据类型和尺寸，默认情况下不设置

​					目前仅支持在**TM_SQDIFF**和**TM_CCORR_NORMED**

该函数同时支持灰度图像和彩色图像两种图像的模板匹配，前两个参数为输入图像和模板图像

第三个参数为相似性矩阵，滑动窗口与模板的相似性系数存放在滑动窗口左上角第一个像素处，因此输出的相似性矩阵尺寸要小于

源图像的尺寸，例：原图像尺寸为 W x H ， 模板图像尺寸为 w x h ， 那么输出图像尺寸为 (W-w+1)x(H-h+1)

输出矩阵记录了整幅图像每一个地方的相似性系数，越相似的地图亮度越高

| 标志参数         | 简记 | 方法名称             |
| ---------------- | :--: | -------------------- |
| TM_SQDIFF        |  0   | 平方差匹配法         |
| TM_SQDIFF_NORMED |  1   | 归一化平方差匹配法   |
| TM_CCORR         |  2   | 相关匹配法           |
| TM_CCORR_NORMED  |  3   | 归一化相关匹配法     |
| TM_CCOEFF        |  4   | 系数匹配法           |
| TM_CCOEFF_NORMED |  5   | 归一化相关系数匹配法 |

**1.TM_SQDIFF**

​		平方差匹配法，当模板与滑动窗口完全匹配时，计算数值为0，两者匹配度越低，计算数值越大

**2.TM_SQDIFF_NORMED**

​		归一化平方差匹配法，将平方差方法进行归一化，使得输入结果归一化到0~1

​		当模板与滑动窗口完全匹配时，计算数值为0，两者匹配度越低，计算数值越大

**3.TM_CCORR**

​		相关匹配法，采用模板和图像间的乘法操作，所有数值越大表示匹配效果越好，0表示最坏匹配结果

**4.TM_CCORR_NORMED**

​		归一化相关匹配法，将相关匹配方法进行归一化，使得输入结果归一化到0~1

​		当模板与滑动窗口完全匹配时，计算数值为1，两者匹配度越低，计算数值越小

**5.TM_CCOEFF**

​		系数匹配法，采用相关匹配法对模板减去均值的结果和源图像减去均值的结果进行匹配

​		这种方法可以很好地解决模板图像和源图像之间由于亮度不同而产生的影响

​		模板与滑动窗口匹配度越高，计算数值越大，两者匹配度越低，计算数值越小，计算结果可以为负数

**6.TM_CCOEFF_NORMED**

​		归一化相关系数匹配法，将系数匹配法进行归一化，使得输入结果归一化到0~1

​		当模板与滑动窗口完全匹配时，计算数值为1，两者完全不匹配时，计算数值为-1

在使用不同的计算相似性方法时，重点需要知道每种方法中最佳匹配结果的数值应该是较大值还是较小值

由于matchTemplate()函数的输出结果是存有相关系数的矩阵，因此需要minMaxLoc()函数寻找输入矩阵的最大值和最小值

进而确定模板匹配结果



通过寻找输出矩阵的最大值或最小值得到的只是一个像素点，需要以该像素点为矩阵区域的左上角，绘制于模板相同的尺寸的矩形框

标记除最终匹配的结果

示例程序：使用**TM_CCOEFF_NORMED**方法计算相关性系数，通过minMaxLoc()函数寻找相关性系数中的最大值

​					确定最佳匹配值的像素点坐标，之后在原图中绘制出于模板最佳匹配区域的范围

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg");
    Mat temp = imread("E:\\CLion\\opencv_xin\\SG\\SG_face.jpg");
    if (img.empty() || temp.empty()) {
        cout << "Error" << endl;
        return -1;
    }
    Mat result;
    matchTemplate(img,temp,result,TM_CCOEFF_NORMED); //模板匹配
    double maxVal,minVal;
    Point minLoc,maxLoc;
    //寻找匹配结果中的最大值和最小值以及坐标位置
    minMaxLoc(result,&minVal,&maxVal,&minLoc,&maxLoc);
    //绘制最佳匹配区域
    rectangle(img,Rect(maxLoc.x,maxLoc.y,temp.cols,temp.rows),Scalar(0,0,255),2);
    imshow("原图",img);
    imshow("模板图像",temp);
    imshow("result",result);
    waitKey(0);
    return 0;
}
```



## 图像滤波

由于采集图像的设备可能受到光子噪声、暗电流噪声等干扰，使得采集到的图像具有噪声，另外图像信号在传输过程中也可能产生噪音

因此去除图像中的噪声是图像预处理中十分重要的步骤，图像滤波是图像噪声去除的重要方式

### 1.图像卷积filter2D()

卷积常用在信号处理中，而图像数据可以看作一种信号数据，例如图像中每一行可以看作测量亮度变化的信号数据

因此，可以对图像进行卷积操作，在信号处理中，卷积操作需要给出一个卷积函数与信号进行计算

图像的卷积需要给出一个卷积模板与原始图像进行卷积计算，整个过程可以看成是一个卷积模板在另外一个大的图像上移动

对每个卷积模板覆盖的区域进行点乘，得到的值作为中心像素点的输出值

卷积首先需要将卷积模板旋转180°，之后从图像的左上角开始移动旋转后的卷积模板，从左到右，从上到下，依次进行卷积计算

最终得到卷积后的图形，卷积模板又称为卷积核或者内核，是一个固定大小的二维矩阵，矩阵存放着预先设定的数值

图像卷积过程可以大致分为五步

第一步：将卷积模板旋转180°，由于多数情况中卷积模板中的数据是中心对称的，因此有时这步可以省略，但是如果卷积模板不是中心对称的，必须将模板进行旋转

第二步：将卷积模板中心放在原图像中需要计算卷积的像素上，卷积模板中其余部分对应在原图像相应的像素上，如图5-1所示，卷积模板和待卷积矩阵中黄色区域分别是卷积模板的中心和对应点，定位结果中阴影区域为模板覆盖的区域

![](http://118.25.145.234/RM/OpenCV/123.webp)*

第三步：用卷积模板中的系数乘以图像中对应位置的像素数值，并对所有结果求和，针对图5-1表示的卷积步骤，其计算过程如式所示，最终计算结果为84

第四步：将计算结果存放在原图像中与卷积模板中心点像对应的像素处，即图5-1里待卷积矩阵中的黄色像素处，结果如图5-2所示

![](http://118.25.145.234/RM/OpenCV/1234.webp)

第五步：将卷积模板在图像中从左至右从上到下移动，重复以上3个步骤，直到处理完所有的像素值，每一次循环的处理结果如图5-3所示

![](http://118.25.145.234/RM/OpenCV/12345.webp)



这种方法只能对图像中心区域进行卷积，而由于卷积模板中心无法放置在图像的边缘像素处，因此图像边缘区域没有进行卷积运算

为了解决这个问题，可主动将图像的边缘外推出去（例：用0在原始图像周围增加一层像素）

从而解决模板图像中部分数据没有对应像素的问题

最后一个像素值已经接近CV_8U数据类型的最大值，因此，如果卷积模板选取不当，极有可能造成卷积结果超出数据范围的情况发生

因此，图像卷积操作常将卷积模板通过缩放使得所有数值的和为1，进而解决卷积后数值越界的情况发生

OpenCV4提供了filter2D()函数实现图像和卷积模板之间的卷积运算

**filter2D()函数原型：**

```cpp
void filter2D(InputArray src,
              OutputArray dst,
              int ddepth,
              InputArray kernel,
              Point anchor = Point(-1,-1),
              int boderType = BORDER_DEFAULT
              )
```

* src：输入图像

* dst：输出图像

* ddepth：输出图像的数据类型（深度），根据输入图像的数据类型不同，拥有不同的取值范围

  ​				 当赋值为-1时，输出图像的数据类型自动选择

* kernel：卷积核，CV_32FC1的矩阵

* anchor：内核的基准点（锚点），默认值(-1,-1)代表内核基准点位于kernel的中心位置

  ​				 基准点是卷积核中与进行处理的像素点的像素点重合的点，位置必须在卷积核的内部

* delta：偏值，在计算结果中加上偏值

* borderType：像素外推法选择标志，默认参数为BORDER_DEFAULT，表示不包含边界值倒序填充

该函数用于实现图像和卷积模板之间的卷积运算，函数第一个参数为输入的待卷积图像，允许输入图像为多通道图像

如果需要用不同的卷积模板对不同的通道进行菊娜姬操作，就需要先使用split()函数将多个通道分离之后单独对每一个通道

求取卷积运算，第二个参数为输出图像，尺寸和通道数与第一个参数保持一致，输出图像的数据类型由第三个参数进行选择

第四个参数为卷积模板矩阵，第五个参数可以指定卷积模板的中心位置，卷积偏值表示在卷积步骤第二步计算结果上再加上偏值作为结果

注意：filter()函数不会将卷积模板进行旋转，热狗卷积模板不对称，那么需要将卷积模板旋转180°后再输入第四个参数

​                                               **filter2D()函数输出图像数据类型与输入图像数据类型联系**

| 输入图像数据类型 | 输出图像数据类型              |
| ---------------- | ----------------------------- |
| CV_8U            | -1 / CV_16S / CV_32F / CV_64F |
| CV_16U / CV_16S  | -1 / CV_32F / CV_64F          |
| CV_32F           | -1 / CV_32F / CV_64F          |
| CV_64F           | -1 / CV_64F                   |

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    //待卷积矩阵
    uchar points[25]= {
            1,2,3,4,5,
            6,7,8,9,10,
            11,12,13,14,15,
            16,17,18,19,20,
            21,22,23,24,25
    };
    Mat img(5,5,CV_8UC1,points);
    //卷积模板
    Mat kernel = (Mat_<float>(3,3) << 1,2,1,
            2,0,2,
            1,2,1);
    Mat kernel_norm = kernel / 12; //卷积模板归一化
    //未归一化卷积结果和归一化卷积结果
    Mat result,result_norm;
    filter2D(img,result,CV_32F,kernel,Point(-1,-1),2,BORDER_CONSTANT);
    filter2D(img,result_norm,CV_32F,kernel_norm,Point(-1,-1),2,BORDER_CONSTANT);
    cout << "result=" << endl << result << endl;
    cout << "result_norm" << endl << result_norm << endl;
    //图像卷积
    Mat lena = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg");
    if (lena.empty()) {
        cout << "Error" << endl;
        return -1;
    }
    Mat lena_filter;
    filter2D(lena,lena_filter,-1,kernel_norm,Point(-1,-1),2,BORDER_CONSTANT);
    imshow("lena_filter",lena_filter);
    imshow("lena",lena);
    waitKey(0);
    return 0;
}
```

注：ucahr:图像处理中常常使用的一种数据类型uchar，一般它指的就是unsigned char，是一种8-bit无符号整形数据，范围为[0, 255]

### 2.噪声的种类和生成

图像在获取或者传输过程中会受到随机信号的干扰而产生噪音

例如电阻引起的热噪声、光子噪声、暗电流噪声等由于图像噪声会妨碍人们对图像的理解和以及后续的处理工作

因此去除噪声的影响在图像处理中具有十分重要的作用

图像常见噪声有4种：高斯噪声、椒盐噪声、泊松噪声和乘性噪声

#### 2.1椒盐噪声rand()

椒盐噪声又称脉冲噪声，会随机改变图像中的像素值，是由相机成像、图像传像、解码处理等过程产生的黑白相间的亮暗点噪声

其样子就像图像上随机地撒上一些盐粒和黑椒粒，因此称为椒盐噪声

OpenCV4未提供专门用于为图像添加椒盐噪声的函数

考虑到椒盐噪声会在图像中任何一个位置随机产生，因此需要使用OpenCV4提供的能够产生随机数的函数rand()

为了能够生成不同数据类型的随机数，该函数由多种演变形式（如：rand_double、rand_int()）

**rand()函数原型：**

```c++
int cvflann::rand()
    
double cvflann::rand_double(double high = 1.0,
                           	double low = 0
                            )
int cvflann::rand_int(int high = RAND_MAX,
                      int low = 0
                      )
```

* high：输出随机数的最大值
* low：输出随机数的最小值

这三个函数都可以用来生成随机数，区别在于，第一个函数rand()函数不需要输入任何参数，返回的随机数为int类型

第二个函数rand_double()需要输入随机数的上下边界，默认情况下生成的随机数在0~1范围，返回的随机数为double类型

第三个函数rand_int()也需要输入上下边界，不同的是，默认情况下最大值是RAND_MAX，这是系统定义的宏变量

小技巧：可以通过求余数的方法实现对随机数的设置，例如： int a = rand % 100 可以保证余数在0~100范围

在图像中添加椒盐噪声大致分为4个步骤

第一步：确定添加椒盐噪声的位置，可以通过随机函数生成两个随机数来取确定椒盐噪声产生的行和列

第二步：确定噪声的种类。可以通过随机数来确定噪声点是黑色还是白色

第三步：修改图像像素灰度值。判断图像通道数，通道数不同的图像中像素表示白色的方式不同

第四步：得到含有椒盐噪声的图像

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
//椒盐噪声函数
void saltAndPepper(Mat image,int n)
{
    for (int k = 0; k < n/2; ++k)
    {
        //随机确定图像中位置
        int i ,j;
        i = rand() % image.cols; //取余数运算，保证在图像的列数内
        j = rand() % image.rows; //取余数运算，保证在图像的行数内
        int write_black = rand() % 2; //判定是白色噪声还是黑色噪声
        if ( write_black == 0 ) //添加白色噪声
        {
            if ( image.type() == CV_8UC1 ) //出来灰度图像
            {
                image.at<uchar>(j,i) = 255; //白色噪声
            }
            else if ( image.type() == CV_8UC3 )  //处理彩色图像
            {
                image.at<Vec3b>(j,i)[0] = 255;   //Vec3b为OpenCV定义的3歌值的向量类型
                image.at<Vec3b>(j,i)[1] = 255;  //[]指定通道，B：0，G：1，R：2
                image.at<Vec3b>(j,i)[2] = 255;
            }
        }
        else  //添加黑色噪声
        {
            if ( image.type() == CV_8UC1 )
            {
                image.at<uchar>(j,i) = 255;
            }
            else if ( image.type() == CV_8UC3 )  //处理彩色图像
            {
                image.at<Vec3b>(j,i)[0] = 0;   //Vec3b为OpenCV定义的3歌值的向量类型
                image.at<Vec3b>(j,i)[1] = 0;  //[]指定通道，B：0，G：1，R：2
                image.at<Vec3b>(j,i)[2] = 0;
            }
        }

    }
}
int main()
{
    Mat lena = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg");
    Mat equalLena = imread("E:\\CLion\\opencv_xin\\SG\\SG_1.jpg",IMREAD_ANYDEPTH);  //IMREAD_GRAYSCALE效果相同
    if (lena.empty() || equalLena.empty())
    {
        cout << "Error" << endl;
        return -1;
    }
    imshow("lena原图",lena);
    imshow("equalLena原图",equalLena);
    saltAndPepper(lena,10000); //彩色图像添加椒盐噪声
    saltAndPepper(equalLena,10000); //灰度图像添加椒盐噪声
    imshow("lena添加噪声",lena);
    imshow("equalLena添加噪声",equalLena);
    waitKey(0);
    return 0;
}
```

#### 2.2高斯噪声RNG::fill()

高斯噪声是指噪声分布的概率密度函数服从高斯分布（正态分布）的一类噪声

产生原因主要是相机在拍摄时视场较暗且亮度不均匀，相机长时间工作使得温度过高同样会引起高斯噪声

另外，电路元器件自身噪声和相互影响也是造成高斯噪声的重要原因之一



OpenCV4未提供专门用于为图像添加高斯噪声的函数

OpenCV4提供了fill()函数，可以产生均匀分布或者高斯分布（正太分布）的随机数

可以利用该函数产生符合高斯分布的随机数，之后在图像中加入这些随机数

**fill()函数原型：**

```c++
void cv::RNG::fill(InputOutArray mat,
                   int distType,
                   InputArray a,
                   InputArray b,
                   bool saturateRange = false
                   )
```

* mat：用于存放随机数的矩阵，目前只支持5通道的矩阵
* distType：随机数分布形式选择标志，目前生成的随机数支持均匀分布（RNG::UNIFORM,0）

​						  和高斯分布（RNG::NORMAL,1）

* a：确定分布规律的参数，但选择均匀分布时，该参数表示均匀分布的最小下限；当选择高斯分布时，表示高斯分布均值
* b：确定分布规律的参数，但选择均匀分布时，该参数表示均匀分布的最大上限；当选择高斯分布时，表示高斯分布的标准差
* saturateRange：预饱和标志，仅用于均匀分布

该函数用于生成指定分布形式的随机数填充矩阵，可以生成符合均匀分布的随机数和符合高斯分布的随机数

第一个参数输入用于储存生成随机数的矩阵，矩阵通道数必须小于5

第二个参数是选择随机数分布形式的标志，目前只支持两种分布形式

​						均匀分布（RNG::UNIFORM,简记：0）和高斯分布（RNG::NORMAL,简记：1）

第三、四个参数为确定随机数分布规律的参数

最后一个参数是预饱和标志，仅用于均匀分布，一般使用其默认方式即可

注：该函数属于OpenCV4的RNG类，是一个非静态成员函数，因此，在使用的时候，不能像使用正常函数一样直接使用

需要首先创建一个RNG类的变量，之后通过访问这个变量中的函数的方式调用这个函数

**RNG::fill()函数的使用：**

```cpp
cv::RNG rng;
rng.fill(mat, RNG::NORMAL, 10, 20)
```

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    Mat lena = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg");
    Mat equalLena = imread("E:\\CLion\\opencv_xin\\SG\\SG_1.jpg",IMREAD_ANYDEPTH);  //IMREAD_GRAYSCALE效果相同
    if (lena.empty() || equalLena.empty())
    {
        cout << "Error" << endl;
        return -1;
    }
    //生成与原始图像尺寸、数据类型和哦通道数相同的矩阵
    Mat lena_noise = Mat::zeros(lena.rows,lena.cols,lena.type());
    Mat equalLena_noise = Mat::zeros(lena.rows,lena.cols,equalLena.type());
    imshow("lena原图",lena);
    imshow("equalLena原图",equalLena);
    RNG rng; //创建一个RNG类
    rng.fill(lena_noise,RNG::NORMAL,10,20); //生成三通道的高斯分布随机数
    rng.fill(equalLena_noise,RNG::NORMAL,15,30); //生成单通道的高斯分布随机数
    imshow("三通道高斯噪声",lena_noise);
    imshow("单通道高斯噪声",equalLena_noise);
    lena = lena + lena_noise; //在彩色图像中添加高斯噪声
    equalLena = equalLena + equalLena_noise; //在灰度图像中添加高斯噪声
    //显示添加高斯噪声后的图像
    imshow("lena添加噪声",lena);
    imshow("equalLena添加噪声",equalLena);
    waitKey(0);
    return 0;
}
```

### 3.线性滤波

图像滤波是指去除图像中不重要的内容而使关心的内容表现得更加清晰的方法，例如去除图像中的噪声、提取某些信息等

图像滤波是图像处理中不可缺少的部分，图像滤波结果的好坏对后续从图中获取更多数据和信息具有重要的影响

根据图像滤波的不同，可以将图像滤波分为消除图像噪声的滤波和提取图像中部分特征信息的滤波

图像滤波需要保证图像关注的信息不在滤波过程中被破坏，因此图像去除噪声应最大程度不损坏图像的轮廓和边缘信息

同时图像去除噪声后使图像视觉效果更加清晰



去除图像中的噪声称作图像的平滑或者图像去噪，由于噪声信号在图像中主要集中在高频段

因此图像去噪可以看作去除图像中高频段信号的同时图像的低频段和中频段信号的滤波操作

图像滤波使用的滤波器决定了滤波操作是去除噪音还是提取特征信号

因此，使用允许低频和中频信号通过的滤波器，效果就是去除噪声，图像中纹理变化越明显的区域信号频率就越高

因此使用高通滤波器对图像信号处理可以起到对图像边缘信息的提取、增强和图像锐化的作用

图像的线性滤波操作与图像的卷积操作过程相似，但是图像滤波不需要将滤波模板旋转180°

卷积操作中的卷积模板在图像滤波中称为滤波模板、滤波器或者领域算子

图像滤波分为线性滤波和非线性滤波，常见的线性滤波包括均值滤波、方框滤波和高斯滤波

​																 常见的非线性滤波包括中值滤波和双边滤波

#### 3.1均值滤波blur()

在测量数据时，往往会多次测量、最后求所有数据的平均值作为最终结果，均值滤波和求取平均值的思想一致

均值滤波将滤波器内所有的像素值都看作中心像素值的测量，将滤波器内所有像素值的平均值作为滤波器中心处图像像素值

滤波器内所有像素值在决定中心像素值的过程中具有相同的权重

优点：在像素值变换趋势一致的情况下，可以将受噪声影响而突然变化的像素值修正为周围邻近像素值的平均值，去除噪声影响

但是这种滤波方式会缩小像素值之间的差距，使得细节信息变得更加模糊，滤波器范围越大，变模糊的效果越明显

OpenCV4提供了blur()函数用于实现图像的均值滤波

**blur()函数原型：**

```cpp
void blur(InputArray src,
          OutputArray dst,
          Size ksize,
          Point anchor = Point(-1,-1),
          int borderType = BORDER_DEFAULT
          )
```

* src：待均值滤波的图像，图像的数据类型必须是CV_8U、CV_16U、CV_16S、CV_32F和CV_64F 中之一
* dst：均值滤波后的图像，与输入图像具有相同的尺寸、数据类型及通道数
* ksize：卷积核尺寸（滤波器尺寸）
* anchor：内核的基准点，默认为（-1，-1），代表内核基准点位于kernel的中心位置
* borderType：像素外推选择标志，默认为BORDER_DEFAULT，表示不包含边界值的倒序填充（2.3.4仿射变换）

该函数第一个参数为待滤波图像（可以是彩色图像、灰度图像，甚至是Mat类型的多维矩阵数据）

第二个参数滤波后图像，保持于输入图像相同d1数据类型、尺寸及通道数

第三个参数是滤波器尺寸，输入滤波器尺寸后函数会自动确定滤波器

第四个参数为滤波器的基准点，第五个参数为图像外推方法选择标志

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
//椒盐噪声函数
void saltAndPepper(Mat image,int n)
{
    for (int k = 0; k < n/2; ++k)
    {
        //随机确定图像中位置
        int i ,j;
        i = rand() % image.cols; //取余数运算，保证在图像的列数内
        j = rand() % image.rows; //取余数运算，保证在图像的行数内
        int write_black = rand() % 2; //判定是白色噪声还是黑色噪声
        if ( write_black == 0 ) //添加白色噪声
        {
            if ( image.type() == CV_8UC1 ) //出来灰度图像
            {
                image.at<uchar>(j,i) = 255; //白色噪声
            }
            else if ( image.type() == CV_8UC3 )  //处理彩色图像
            {
                image.at<Vec3b>(j,i)[0] = 255;   //Vec3b为OpenCV定义的3歌值的向量类型
                image.at<Vec3b>(j,i)[1] = 255;  //[]指定通道，B：0，G：1，R：2
                image.at<Vec3b>(j,i)[2] = 255;
            }
        }
        else  //添加黑色噪声
        {
            if ( image.type() == CV_8UC1 )
            {
                image.at<uchar>(j,i) = 255;
            }
            else if ( image.type() == CV_8UC3 )  //处理彩色图像
            {
                image.at<Vec3b>(j,i)[0] = 0;   //Vec3b为OpenCV定义的3歌值的向量类型
                image.at<Vec3b>(j,i)[1] = 0;  //[]指定通道，B：0，G：1，R：2
                image.at<Vec3b>(j,i)[2] = 0;
            }
        }

    }
}
void GaussNorise(Mat equalLena_gauss)  //只处理灰度图像
{
    Mat equalLena_noise = Mat::zeros(equalLena_gauss.rows,equalLena_gauss.cols,equalLena_gauss.type());
    RNG rng; //创建一个RNG类
    rng.fill(equalLena_noise,RNG::NORMAL,15,30); //生成单通道的高斯分布随机数
    equalLena_gauss = equalLena_gauss + equalLena_noise; //在灰度图像中添加高斯噪声
}
int main()
{
    Mat equalLena = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg",IMREAD_ANYDEPTH);
    Mat equalLena_gauss = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg",IMREAD_ANYDEPTH);
    Mat equalLena_salt = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg",IMREAD_ANYDEPTH);
    if (equalLena.empty() || equalLena_gauss.empty() || equalLena_salt.empty())
    {
        cout << "Error" << endl;
        return -1;
    }
    Mat result_3,result_9; //存放不含噪声滤波结果，数字代表滤波器尺寸
    Mat result_3gauss,result_9gauss; //存放含有高斯噪声滤波结果，数字表示滤波器尺寸
    Mat result_3salt,result_9salt; //存放含有椒盐噪声滤波结果，数字表示滤波器尺寸
    saltAndPepper(equalLena_salt,10000); //灰度图像添加椒盐噪声
    GaussNorise(equalLena_gauss); //灰度图像添加高斯噪声
    //调用均值滤波函数blur()进行滤波
    blur(equalLena,result_3,Size(3,3));
    blur(equalLena,result_9,Size(9,9));
    blur(equalLena_gauss,result_3gauss,Size(3,3));
    blur(equalLena_gauss,result_9gauss,Size(9,9));
    blur(equalLena_salt,result_3salt,Size(3,3));
    blur(equalLena_salt,result_9salt,Size(9,9));
    //显示不含噪声图像
    imshow("equalLena",equalLena);
    imshow("result_3",result_3);
    imshow("result_9",result_9);
    //显示含高斯噪声图像
    imshow("equalLena_gauss",equalLena_gauss);
    imshow("result_3gauss",result_3gauss);
    imshow("result_9gauss",result_9gauss);
    //显示含椒盐噪声图像
    imshow("equalLena_salt",equalLena_salt);
    imshow("result_3salt",result_3salt);
    imshow("result_9salt",result_9salt);
    waitKey(0);
    return 0;
}
```

滤波器的尺寸越大，滤波后图像变的越模糊

#### 3.2方框滤波boxFilter()

方框滤波是均值滤波的一般形式，方框滤波也是求滤波器内所有像素值的和，但是方框滤波可以选择不进行归一化

将所有像素值的和作为滤波结果，而不是所有像素值的平均值

OpenCV4提供了boxFilter()函数实现方框滤波

**boxFilter()函数原型：**

```cpp
void boxFilter(InputArray src,
               OutputArray dst,
               int ddepth,
               Size ksize,
               Point anchor = Point(-1,-1),
               bool normalize = ture,
               int borderType = BORDER_DEFAULT
               )
```

* src：输入图像
* dst：输出图像，与输入图像具有相同的尺寸和通道数
* ddepth：输出图像的数据类型（深度），根据输入图像的数据类型不同，拥有不同的取值范围（表：图像卷积）

​						当赋值为-1时，输出图像的数据类型自动选择

* ksize：卷积核尺寸（滤波器尺寸）
* anchor：内核的基准点，默认为（-1，-1），代表内核基准点位于kernel的中心位置
* normalize：是否将卷积核进行归一化的标志，默认参数为ture，表示进行归一化
* borderType：像素外推选择标志，默认为BORDER_DEFAULT，表示不包含边界值的倒序填充（2.3.4仿射变换）

该函数的使用方法与均值滤波函数blur()相似，区别之一为该函数可以选择输出图像的数据类型

第六个参数表示是否对滤波器内所有数值进行归一化操作，默认状态下参数需要对滤波器内所有的数值进行归一化

此时，在不考虑数据类型的情况下，方框滤波和均值滤波具有相同的滤波结果

除了对滤波器内每个像素值直接求和之外，OpenCV4还提供了sqrBoxFilter()函数实现对滤波器内每个像素值的平方求和

根据输入参数选择是否进行归一化操作

**sqrBoxFilter()函数原型：**

```cpp
void sqrBoxFilter(InputArray src,
                  OutputArray dst,
                  int ddepth,
                  Size ksize,
                  Point anchor = Point(-1,-1),
                  bool normalize = ture,
                  int borderType = BORDER_DEFAULT
                  )
```

该函数是在boxFilter()函数功能基础上进行的功能扩展，参数与boxFilter()相似

该函数在处理图像滤波的任务主要是针对CV_32F数据类型的图像，在归一化后，图像在变模糊的同时亮度也会变暗

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    Mat equalLena = imread("E:\\CLion\\opencv_xin\\SG\\SG_1.jpg",IMREAD_ANYDEPTH); //用于方框滤波的图像
    if (equalLena.empty())
    {
        cout << "Error" << endl;
        return -1;
    }
    //验证方框滤波算法的数据矩阵
    float points[25] = {
            1,2,3,4,5,
            6,7,8,9,10,
            11,12,13,14,15,
            16,17,18,19,20,
            21,22,23,24,25
    };
    Mat data(5,5,CV_32FC1,points);
    //将CV_8U类型转换成CV_32F类型
    Mat equalLena_32F;
    equalLena.convertTo(equalLena_32F,CV_32F,1.0/255);
    Mat resultNorm,result,dataSqrNorm,dataSqr,equalLena_32Sqr;
    //方框滤波函数boxFilter()和sqrBoxFilter()
    boxFilter(equalLena,resultNorm,-1,Size(3,3),Point(-1,-1), true); //进行归一化
    boxFilter(equalLena,result,-1,Size(3,3),Point(-1,-1), false); //不进行归一化
    sqrBoxFilter(data,dataSqrNorm,-1,Size(3,3),Point(-1,-1), true,BORDER_CONSTANT);//进行归一化
    sqrBoxFilter(data,dataSqr,-1,Size(3,3),Point(-1,-1), false,BORDER_CONSTANT);//不进行归一化
    sqrBoxFilter(equalLena_32F,equalLena_32Sqr,-1,Size(3,3),Point(-1,-1),true,BORDER_CONSTANT);
    //显示处理结果
    imshow("resultNorm",resultNorm);
    imshow("result",result);
    imshow("equalLena_32FSqr",equalLena_32Sqr);
    waitKey(0);
    return 0;
}
```

#### 3.3高斯滤波GaussianBlur()

高斯噪声是一种常见的噪声，OpenCV4提供了对图像进行高斯滤波操作的GaussianBlur()函数

**GaussianBlur()函数原型：**

```cpp
void GaussianBlur(InputArray src,
                  OutputArray dst,
                  Size ksize,
                  double sigmaX,
                  double sigmaY = 0,
                  int borderType = BORDER_DEFAULT
                  )
```

* src：待高斯滤波的图像，可以有任意通道数目，数据类型必须是CV_8U、CV_16U、CV_16S、CV_32F或CV_64F
* dst：输出图像，与输入图像src具有相同的尺寸、通道数和数数据类型
* ksize：高斯滤波器的尺寸，滤波器可以不为正方形，但是必须是正奇数。如果尺寸为0，那么由标准偏差计算尺寸
* sigmaX：X方向的高斯滤波器标准偏差
* sigmaY：Y方向的高斯滤波器标准偏差
* borderType：像素外推法选择标志，默认参数为BORDER_DEFAULT，表示不包含边界值倒序填充

该函数能够根据输入参数自动生成高斯滤波器，实现对图像的高斯滤波

允许输入尺寸为0和正奇数，如果尺寸为0，那么由标准偏差计算尺寸

高斯滤波器的尺寸和标准偏差存在着一定的互相转换关系

OpenCV4提供了输入滤波器单一方向尺寸和标准偏差生成单一方向高斯滤波器的getGaussianKernel()函数

**getGaussianKernel()函数原型：**

```cpp
Mat getGaussianKernel(int ksize,
                      double sigma,
                      int ktype = CV_64F
                      )
```

* ksize：高斯滤波器的尺寸
* sigma：高斯滤波的标准差
* ktype：滤波器系数的数据类型，可以是CV_32F或者CV_64F，默认数据类型为CV_64F

该函数用于生成指定尺寸的高斯滤波器，需要注意的是

第一个参数为高斯滤波器尺寸，必须是一个正奇数，第二个参数表示高斯滤波的标准差(标准偏差)

$\textcolor{red}{注：标准差是由各种数据偏离平均数的距离的平均数，是离差平方和平均后的方根，因此标准差也是一种平均数，}$

$\textcolor{red}{标准差就是方差的算术平方根，它能够反映出一个数据集的离散程度，平均数都是相同的，标准差但是却未必相同。}$

示例：

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
void saltAndPepper(Mat image,int n)
{
    for (int k = 0; k < n/2; ++k)
    {
        //随机确定图像中位置
        int i ,j;
        i = rand() % image.cols; //取余数运算，保证在图像的列数内
        j = rand() % image.rows; //取余数运算，保证在图像的行数内
        int write_black = rand() % 2; //判定是白色噪声还是黑色噪声
        if ( write_black == 0 ) //添加白色噪声
        {
            if ( image.type() == CV_8UC1 ) //出来灰度图像
            {
                image.at<uchar>(j,i) = 255; //白色噪声
            }
            else if ( image.type() == CV_8UC3 )  //处理彩色图像
            {
                image.at<Vec3b>(j,i)[0] = 255;   //Vec3b为OpenCV定义的3歌值的向量类型
                image.at<Vec3b>(j,i)[1] = 255;  //[]指定通道，B：0，G：1，R：2
                image.at<Vec3b>(j,i)[2] = 255;
            }
        }
        else  //添加黑色噪声
        {
            if ( image.type() == CV_8UC1 )
            {
                image.at<uchar>(j,i) = 255;
            }
            else if ( image.type() == CV_8UC3 )  //处理彩色图像
            {
                image.at<Vec3b>(j,i)[0] = 0;   //Vec3b为OpenCV定义的3歌值的向量类型
                image.at<Vec3b>(j,i)[1] = 0;  //[]指定通道，B：0，G：1，R：2
                image.at<Vec3b>(j,i)[2] = 0;
            }
        }

    }
}
void GaussNorise(Mat equalLena_gauss)  //只处理灰度图像
{
    Mat equalLena_noise = Mat::zeros(equalLena_gauss.rows,equalLena_gauss.cols,equalLena_gauss.type());
    RNG rng; //创建一个RNG类
    rng.fill(equalLena_noise,RNG::NORMAL,15,30); //生成单通道的高斯分布随机数
    equalLena_gauss = equalLena_gauss + equalLena_noise; //在灰度图像中添加高斯噪声
}
int main()
{
    Mat equalLena = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg",IMREAD_ANYDEPTH);
    Mat equalLena_gauss = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg",IMREAD_ANYDEPTH);
    Mat equalLena_salt = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg",IMREAD_ANYDEPTH);
    if (equalLena.empty() || equalLena_gauss.empty() || equalLena_salt.empty())
    {
        cout << "Error" << endl;
        return -1;
    }
    Mat result_5,result_9; //存放不含噪声滤波结果，数字代表滤波器尺寸
    Mat result_5gauss,result_9gauss; //存放含有高斯噪声滤波结果，数字表示滤波器尺寸
    Mat result_5salt,result_9salt; //存放含有椒盐噪声滤波结果，数字表示滤波器尺寸
    saltAndPepper(equalLena_salt,10000); //灰度图像添加椒盐噪声
    GaussNorise(equalLena_gauss); //灰度图像添加高斯噪声
    //调用高斯滤波函数Gaussian Blur()进行滤波
    GaussianBlur(equalLena,result_5,Size(5,5),10,20);
    GaussianBlur(equalLena,result_9,Size(9,9),10,20);
    GaussianBlur(equalLena_gauss,result_5gauss,Size(5,5),10,20);
    GaussianBlur(equalLena_gauss,result_9gauss,Size(9,9),10,20);
    GaussianBlur(equalLena_salt,result_5salt,Size(5,5),10,20);
    GaussianBlur(equalLena_salt,result_9salt,Size(9,9),10,20);
    //显示不含噪声图像
    imshow("equalLena",equalLena);
    imshow("result_5",result_5);
    imshow("result_9",result_9);
    //显示含高斯噪声图像
    imshow("equalLena_gauss",equalLena_gauss);
    imshow("result_5gauss",result_5gauss);
    imshow("result_9gauss",result_9gauss);
    //显示含椒盐噪声图像
    imshow("equalLena_salt",equalLena_salt);
    imshow("result_5salt",result_5salt);
    imshow("result_9salt",result_9salt);
    waitKey(0);
    return 0;
}
```

高斯滤波对高斯噪声去除效果较好，但是同样会对图像造成模糊，且滤波器的尺寸越大，滤波后图像变得越模糊

#### 3.4可分离滤波sepFilter2D()

前面滤波函数使用的滤波器都是固定形式的滤波器，有时需要根据实际需求调整滤波模板

例如：滤波器中心位置的像素值不参与计算、滤波器中参与计算的像素值不是一个矩形区域

OpenCV4提供了根据自定义滤波器实现图像滤波的函数，即卷积函数filter2D()

称为滤波函数更加准确一些，输入的卷积模板也应该称为滤波器或者滤波模板

只需要根据需要定义一个卷积模板或者滤波器，便可以实现自定义滤波

在原始图像上移动滤波器的过程中每一次的计算结果都不会影响到后面过程的计算结果，因此图像滤波是一个并行算法

在可以提供并行并行计算的处理器中可以极大地加快图像滤波器的处理速度，图像滤波还具有可分离性

可分离性指的是先对X（Y）方向滤波，再对Y（X）反向滤波的结果与将两个方向的滤波器联合后整体滤波的结果相同

两个方向的滤波器的联合就是将两个方向的滤波器相乘，得到一个矩形的滤波器

无论是先进行X方向滤波还是Y方向滤波，两个方向联合滤波器都是相同的

例如：x = [x1 x2 x3]  y = [y1 y2 y3]       		[y1  							[x1y1 x2y1 x3y1

​															     xy =	y2     [x1 x2 x3] =      x1y2 x2y2 x3y2

​																			y3] 							 x1y3 x2y3 x3y3]

因此，在高斯滤波中，我们利用getGaussianKernel()函数分别得到X方向和Y方向的滤波器

之后通过生成联合滤波器或者分别用两个方向的滤波器进行滤波，计算结果相同

两个方向联合滤波需要在使用filter2D()函数滤波之前计算联合滤波器，而两个方向分别滤波需要调用2次filter2D()函数

增加了代码的复杂度，因此OpenCV4提供了可以输入两个滤波器实现滤波的滤波函数sepFilter2D()函数

**sepFilter2D()函数原型：**

```cpp
void sepFilter2D(InputArray src,
                 OutputArray dst,
                 int ddepth,
                 InputArray kernelX,
                 InputArray kernelY,
                 Point anchor = Point(-1,-1),
                 double delta = 0,
                 int borderType = BORDER_DEFAULT
                 )
```

* src：待滤波图像
* dst：输出图像，与输入图像src具有相同的尺寸、通道数和数据类型

* ddepth：输出图像的数据类型（深度），根据输入图像的数据类型不同，拥有不同的取值范围（表：图像卷积）

​						当赋值为-1时，输出图像的数据类型自动选择

* kernelX：X方向的滤波器
* kernelY：Y方向的滤波器

* anchor：内核的基准点，默认为（-1，-1），代表内核基准点位于kernel的中心位置
* delta：偏值，在计算结果中加上偏值
* borderType：像素外推选择标志，默认为BORDER_DEFAULT，表示不包含边界值的倒序填充（2.3.4仿射变换）

该函数将可分离的线性滤波器分离为X方向和Y方向进行处理，与filter2D()函数不同之处在于，filter2D()函数需要通过滤波器

尺寸区分滤波操作是作用在X方向还是Y方向，例如 Kx1 的尺寸是Y方向滤波器，1xK 的尺寸是X方向滤波器

而sepFilter2D()函数通过不同参数区分滤波器是作用X方向还是Y方向，无论输入滤波器的尺寸是 Kx1还是1xK，都不会影响滤波结果

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    float points[25] = {
            1,2,3,4,5,
            6,7,8,9,10,
            11,12,13,14,15,
            16,17,18,19,20,
            21,22,23,24,25
    };
    Mat data(5,5,CV_32FC1,points);
    //X、Y方向和联合滤波器的构建
    Mat a = (Mat_<float>(3,1) << -1,3,-1);
    Mat b = a.reshape(1,1);     //reshape()函数，将矩阵变为设置矩阵，这里是通道数为1，行数为1的矩阵
    Mat ab = a*b;
    //验证高斯滤波的可分离性
    Mat gaussX = getGaussianKernel(3,1); //生成X方向的单一滤波器
    Mat gaussData,gaussDataXY;
    GaussianBlur(data,gaussData,Size(3,3),1,1,BORDER_CONSTANT);
    sepFilter2D(data,gaussDataXY,-1,gaussX,gaussX,Point(-1,-1),0,BORDER_CONSTANT);
    //输出两种高斯滤波的计算结果
    cout << "gaussData=" << endl << gaussData << endl;
    cout << "gaussDataXY=" << endl << gaussDataXY << endl;
    //线性滤波的可分离性
    Mat dataYX,dataY,dataXY,dataXY_sep;
    filter2D(data,dataY,-1,a,Point(-1,-1),0,BORDER_CONSTANT);
    filter2D(dataY,dataYX,-1,a,Point(-1,-1),0,BORDER_CONSTANT);
    filter2D(data,dataXY,-1,ab,Point(-1,-1),0,BORDER_CONSTANT);
    sepFilter2D(data,dataXY_sep,-1,b,b,Point(-1,-1),0,BORDER_CONSTANT);
    //a滤波器和b滤波器的数值上相同，只是方向不同，因此在sepFilter2D中X、Y方向的滤波器是a或b结果都是相同的
    //输出可分离滤波和联合滤波的计算结果
    cout << "dataY=" << endl << dataY << endl;
    cout << "dataYX=" << endl << dataYX << endl;
    cout << "dataXY=" << endl << dataXY << endl;
    cout << "dataXY_sep=" << endl << dataXY_sep << endl;
    //对图像的分离操作
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg");
    if (img.empty())
    {
        cout << "Error" << endl;
        return -1;
    }
    Mat imgYX,imgY,imgXY;
    filter2D(img,imgY,-1,a,Point(-1,-1),0,BORDER_CONSTANT);
    filter2D(imgY,imgYX,-1,b,Point(-1,-1),0,BORDER_CONSTANT);
    filter2D(img,imgXY,-1,ab,Point(-1,-1),0,BORDER_CONSTANT);
    imshow("img",img);
    imshow("imgY",imgY);
    imshow("imgYX",imgYX);
    imshow("imgXY",imgXY);
    waitKey(0);
    return 0;
}
```

**reshape()函数原型：**

```cpp
Mat Mat::reshape(int cn,
                 int rows = 0
                 )
```

* cn：通道数，如果设为0就表示通道数不变
* rows：表示矩阵行数，如果设为0就表示保持原有的行数不变





### 4.非线性滤波

非线性滤波的滤波结果不是由滤波器内的像素值通过线性组合计算得到，其计算过程可能包含排序、逻辑计算等

由于线性滤波是通过对所有像素值的线性组合得到滤波后的结果：

​		因此含有噪声的像素点也会被考虑进去噪声不会被消除，而是以更柔和的反向存在

例如：在一个像素值都为0的图像中存在一个像素值为255的噪声，这时只要线性滤波器中噪声处的系数不为0

​			这个噪声就会永远存在，只是通过于滤波器中系数的乘积使得噪声值更加柔和，这时使用非线性滤波效果更好

​			通过逻辑判断将该噪声过滤掉

常见的非线性滤波有中值滤波和双边滤波

#### 4.1中值滤波medianBlur()

中值滤波就是用滤波器范围内所有像素值的中值来替代滤波器中心像素值的滤波方法

是一种基于排序理论的能够有效抑制噪声的非线性信号处理方法

中值滤波计算方式：

​				滤波器周围像素值：2，3，3，4，$\textcolor{red}{4}$，5，6，6，10	 则中心点的像素值为4

将滤波器范围内所有像素值按照从小到大的顺序排列，选取排序序列的中值作为滤波器中心处的新像素值

之后将滤波器移动到下一位置，重复进行排序序列的中值的操作

相比于均值滤波，中值滤波对于脉冲干扰信号和图像扫描噪声的处理效果更佳

同时，在一定条件下，中值滤波对图像的边缘信息保护效果更佳，可以避免图像细节的模糊

但是，中值滤波的尺寸变大时，同样会产生图像模糊的效果，处理时间上中值滤波消耗的时间要远大于均值滤波

OpenCV4提供了对图像进行中值滤波操作的medianBlur()函数

medianBlur()函数原型：

```cpp
void medianBlur(InputArray src,
                OutputArray dst,
                int ksize
                )
```

* src：待中值滤波图像，可以是单通道、三通道和四通道，数据类型与滤波器的尺寸相关

​				当滤波器的尺寸为3或5时，图像可以是CV_8U、CV_16U或CV_32F类型，对于较大尺寸的滤波器，只能是CV_8U

* dst：输出图像，与src具有相同的尺寸和数据类型
* ksize：滤波器尺寸，必须是大于1的奇数，例如3、5、7等

该函数只能处理符合图像信息的Mat类数据，两通道或者更多通道的Mat类矩阵不能被该函数处理

并且对于图像数据类型的要求也与滤波器的尺寸有着密切关系

该函数对多通道的彩色图像是针对每个通道的内部数据进行中值滤波操作

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
void saltAndPepper(Mat image,int n)
{
    for (int k = 0; k < n/2; ++k)
    {
        //随机确定图像中位置
        int i ,j;
        i = rand() % image.cols; //取余数运算，保证在图像的列数内
        j = rand() % image.rows; //取余数运算，保证在图像的行数内
        int write_black = rand() % 2; //判定是白色噪声还是黑色噪声
        if ( write_black == 0 ) //添加白色噪声
        {
            if ( image.type() == CV_8UC1 ) //出来灰度图像
            {
                image.at<uchar>(j,i) = 255; //白色噪声
            }
            else if ( image.type() == CV_8UC3 )  //处理彩色图像
            {
                image.at<Vec3b>(j,i)[0] = 255;   //Vec3b为OpenCV定义的3歌值的向量类型
                image.at<Vec3b>(j,i)[1] = 255;  //[]指定通道，B：0，G：1，R：2
                image.at<Vec3b>(j,i)[2] = 255;
            }
        }
        else  //添加黑色噪声
        {
            if ( image.type() == CV_8UC1 )
            {
                image.at<uchar>(j,i) = 255;
            }
            else if ( image.type() == CV_8UC3 )  //处理彩色图像
            {
                image.at<Vec3b>(j,i)[0] = 0;   //Vec3b为OpenCV定义的3歌值的向量类型
                image.at<Vec3b>(j,i)[1] = 0;  //[]指定通道，B：0，G：1，R：2
                image.at<Vec3b>(j,i)[2] = 0;
            }
        }

    }
}
int main()
{
    Mat gray = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg",IMREAD_ANYDEPTH);
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg",IMREAD_ANYCOLOR);
    if (img.empty() || gray.empty())
    {
        cout << "Error" << endl;
        return -1;
    }
    saltAndPepper(img,10000);
    saltAndPepper(gray,10000);
    Mat imgResult3,grayResult3,imgResult9,grayResult9;
    //分别对含有椒盐噪声的彩色图像和灰度图像进行滤波，滤波模板为3x3
    medianBlur(img,imgResult3,3);
    medianBlur(gray,grayResult3,3);
    //加大滤波模板，图像滤波结果会变模糊
    medianBlur(img,imgResult9,9);
    medianBlur(gray,grayResult9,9);
    //显示滤波处理结果
    imshow("img",img);
    imshow("gray",gray);
    imshow("imgResult3",imgResult3);
    imshow("grayResult3",grayResult3);
    imshow("imgResult9",imgResult9);
    imshow("grayResult9",grayResult9);
    waitKey(0);
    return 0;
}
```

#### 4.2双边滤波bilateralFilter()

前面介绍的滤波方法都会对图像造成模糊，使得边缘信息变弱或者消失，因此需要一种能够对图像边缘信息进行保留的滤波算法

双边滤波就是经典且常用的能够保留图像边缘信息的滤波算法之一

双边滤波器是两个滤波器的结合（空域滤波器和值域滤波器），分别考虑空域信息和值域信息，使得滤波器对边缘附近的像素进行滤波时，距离边缘较远的像素值不会对边缘上的像素值影响太多，进而保留边缘的清晰性

OpenCV4提供了对图像进行双边滤波操作的bilateralFilter()函数

**bilateralFilter()函数原型：**

```cpp
void bilateralFilter(InputArray src,
                     OutputArray dst,
                     int d,
                     double sigmaColor,
                     double sigmaSapce,
                     int borderType  = BORDER_DEFAULT
                     )
```

* src：待进行双边滤波图像，数据类型必须为CV_8U、CV_32F和CV_64F之一，通道数必须为单通道或者三通道
* dst：双边滤波后的图像，尺寸、数据类型和通道数与输入图像src相同
* d：滤波过程中每个像素领域的直径，如果这个值非正值，那么由第五个参数sigmaSpace计算得到
* sigmaColor：颜色空间滤波器的标准差值，参数越大，表明该像素领域内有越多的颜色被混合在一起，产生较大的半相等颜色区域
* sigmaSpace：空间坐标中滤波器的标准差值，参数越大，表明越远的像素会相互影响，从而使更大领域中有足够相似的颜色获取相同的颜色，d大于0时，领域范围由d确定，小于0时，领域范围正比于这个参数的数值
* borderType：像素外推法选择标志，默认参数为BORDER_DEFAULT，表示不包含边界值倒序填充

该函数可以对图像进行双边滤波处理，在减少噪声的同时保持边缘的清晰，该函数只能输入单通道的灰度图和三通道的彩色图像

数据类型必须是CV_8U、CV_32F和CV_64F之一

第三个参数是滤波器的直径，大于5时，函数的运行速度会变慢，因此，实时系统中建议为5，离线系统中可以设置为9

第四、五个参数是两个滤波器的标准偏差，简单起见，可以将两个参数设置成相同的数值

当他们小于10时，滤波器对图像的滤波作用较小，大于150时，滤波效果会非常强烈，使图像看起来具有卡通效果

使用双边滤波会具有”美颜“效果

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    Mat img1 = imread("E:\\CLion\\opencv_xin\\SG\\SG_break.jpeg",IMREAD_ANYCOLOR);
    Mat img2 = imread("E:\\CLion\\opencv_xin\\SG\\SG_break.jpeg",IMREAD_ANYCOLOR);
    if (img1.empty() || img2.empty())
    {
        cout << "error" << endl;
        return -1;
    }
    Mat result1,result2,result3,result4;
    //验证不同滤波器直径的滤波效果
    bilateralFilter(img1,result1,9,50,25/2);
    bilateralFilter(img1,result2,25,50,25/2);
    //验证不同标准偏差的滤波效果
    bilateralFilter(img2,result3,9,9,9);
    bilateralFilter(img2,result4,9,200,200);
    //显示原图
    imshow("img",img1);
    //不同直径滤波结果
    imshow("result1",result1);
    imshow("result2",result2);
    //不同标准差值滤波结果
    imshow("result3",result3);
    imshow("result4",result4);
    waitKey(0);
    return 0;
}
```

### 5.图像的边缘检测

物体边缘是图像中重要信息，提取图像中边缘信息对于分析图像中内容，以及实现图像中物体的分割、定位具有重要的作用

图像边缘提取算法已经非常成熟，OpenCV4中提供了多个用于边缘检测的函数

#### 5.1边缘检测原理convertScaleAbs()

图像的边缘指的是图像中像素灰度值突然发生变化的区域，如果将图像的每一列、每一行像素都描述成一个关于灰度值的函数

那么图像的边缘对应在灰度值函数中是函数值突然变大的区域，函数值的变化趋势可以用函数的导数描述

当函数值突然变大时，导数也必然会突然变大，而函数值变化较为平缓区域，导数值也比较小，因此可以通过寻找导数值较大的区域

寻找函数中突然变化的区域，进而确定图像中的边缘区域

![image-20221218175320129](C:\Users\小新\AppData\Roaming\Typora\typora-user-images\image-20221218175320129.png)

对灰度值曲线求取一阶导数可以得到图像，通过该曲线可以看出，该曲线的最大值所在区域就是图像中边缘所在区域



由于图像是离散的信号，因此可以用临近的两个像素差值表示灰度值函数的导数，这种对x轴方向求导对应滤波器为[-1,1]

但是这种求导方式的计算结果最接近两个像素中间位置的梯度，而两个临近的像素中间不再由任何的像素

改进的求导方式对应的滤波器在X方向滤波器为[-0.5，0，0.5]

在计算45°方向的梯度，寻找不同方向的边缘

XY = |1   0|                YX = |0   1|

​    	 |0  -1|						|-1  0|

图像的边缘有可能是由高像素值变为低像素值，也有可能是由低像素值变为高像素值，因此为了在图像中同时表示出这两种边缘信息

需要将计算的结果求取绝对值

OpenCV4提供了convertScaleAbs()函数用于计算矩阵中所有数据的绝对值

**convertScaleAbs()函数原型：**

```cpp
void convertScaleAbs(InputArray src,
                     OutputArray dst,
                     double alpha = 1,
                     double beta = 0
                     )
```

* src：输入矩阵
* dst：计算绝对值后输出矩阵
* alpha：缩放因子，默认参数为只求取绝对值而不进行缩放
* beta：在原始数据上添加偏值，默认参数表示不增加偏值

该函数可以求取矩阵中所有数据的绝对值，前两个参数为输入、输出矩阵，可以是相同的变量

第三、四个参数为对绝对值的缩放和原始数据上的偏移

图像的边缘包含x轴方向的边缘和y轴方向的边缘，因此，在分别求取两个方向的边缘后，对两个方向的边缘求取并集就是整幅图像的边缘

即将图像的两个方向边缘结果相加得到整幅的边缘信息

示例程序：利用filter2D()函数实现图像边缘检测的算法，由于求取边缘的结果可能由负数，不在CV_8U的数据类型内

​					因此，滤波后的图像数据类型应该改为CV_16S

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    //创建边缘滤波器
    Mat kernel1 = (Mat_<float>(1,2) << 1,-1); //X方向边缘检测滤波器
    Mat kernel2 = (Mat_<float>(1,3) << 1,0,-1); //X方向边缘检测滤波器
    Mat kernel3 = (Mat_<float>(3,1) << 1,0,-1); //Y方向边缘检测滤波器
    Mat kernelXY = (Mat_<float>(2,2) << 1,0,0,-1); //由左上到右下方向边缘检测滤波器
    Mat kernelYX = (Mat_<float>(2,2) << 0,-1,1,0); //由右上到左下方向边缘检测滤波器
    //读取图像，黑白图像边缘检测结果较为明显
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg",IMREAD_ANYCOLOR);
    if (img.empty())
    {
        cout << "error" << endl;
        return -1;
    }
    Mat result1,result2,result3,result4,result5,result6;
    //检测图像边缘
    //以[1 -1]检测水平方向方向边缘
    filter2D(img,result1,CV_16S,kernel1);
    convertScaleAbs(result1,result1);
    //以[1 0 -1]检测水平方向方向边缘
    filter2D(img,result2,CV_16S,kernel2);
    convertScaleAbs(result2,result2);
    //以[1 0 -1]检测垂直方向方向边缘
    filter2D(img,result3,CV_16S,kernel3);
    convertScaleAbs(result3,result3);
    //整幅图像的边缘
    result6 = result2 + result3;
    //检测由左上到右下方向边缘
    filter2D(img,result4,CV_16S,kernelXY);
    convertScaleAbs(result4,result4);
    //检测由右上到左下方向边缘
    filter2D(img,result5,CV_16S,kernelYX);
    convertScaleAbs(result5,result5);
    //显示边缘检测结果
    imshow("result1",result1);
    imshow("result2",result2);
    imshow("result3",result3);
    imshow("result4",result4);
    imshow("result5",result5);
    imshow("result6",result6);
    waitKey(0);
    return 0;
}
```

#### 5.2 Sobel算子

Sobel算子是通过离散微分方法求取图像边缘的边缘检测算子，求取边缘的思想与边缘检测原理一致

除此之外， Sobel算子还结合了高斯平滑滤波的思想，将边缘检测滤波尺寸由Ksize改进为ksize x ksize

提高了对平缓区域边缘的响应，相比前文的算法边缘检测，效果更加明显，使用Sobel边缘算子提取的过程大致可以分为以下3个步骤

第一步：提取X方向的边缘，X方向一阶Sobel边缘算子如下：

![image-20221218192619269](C:\Users\小新\AppData\Roaming\Typora\typora-user-images\image-20221218192619269.png)

第二步：提取Y方向的边缘，Y方向一阶Sobel边缘算子如下：

![image-20221218192742587](C:\Users\小新\AppData\Roaming\Typora\typora-user-images\image-20221218192742587.png)

第三步：综合两个方向的边缘信息得到整幅图像的边缘，由两个方向的边缘得到整体的边缘由两种计算方式

第一种是求取两幅图像对应像素的像素值的绝对值之和，第二种是求取两幅图像对应像素的像素值的平方和的二次方根

OpenCV4提供了对图像提取Sobel边缘的Sobel()函数

**Sobel()函数原型：**

```cpp
void Sobel(InputArray src,
           OutputArray dst,
           int ddepth,
           int dx,
           int dy,
           int ksize = 3,
           double scale = 1,
           double delta = 0,
           int borderType = BORDER_DEFAULT
           )
```

* src：待提取边缘的图像
* dst：输出图像，与输入图像src具有相同的尺寸和通道数，数据类型由第三个参数ddepth控制
* ddepth：输出图像的数据类型（深度），根据输入图像的数据类型不同，拥有不同的取值范围（表：图像卷积）

​						当赋值为-1时，输出图像的数据类型自动选择

* dx：X方向的差分阶数
* dy：Y方向的差分阶数
* ksize：Sobel算子的尺寸，必须是1、3、5或者7
* scale：对导数计算结果进行缩放因子，默认系数为1，表示不缩放
* delta：偏值，在计算结果中加上偏值
* borderType：像素外推选择标志，默认为BORDER_DEFAULT，表示不包含边界值的倒序填充（2.3.4仿射变换）

该函数的使用方法与分离卷积函数sepFilter2D()相似，前两个参数为输入图像和输出图像，第三个参数为输出图像的数据类型

由于提取边缘信息时可能出现负数，因此不要使用CV_8U数据类型的输出图像，因为于Sobel算子方向不一致的边缘梯度会在

CV_8U中消失，使得图像边缘提取不准确，该函数第四、五、六个参数是控制图像边缘检测效果的关键参数

三者存在关系是任意一个方向的差分阶数必须要小于算子的尺寸

在一般情况下：

当差分阶数最大值为1时，算子尺寸选3；

当差分阶数最大值为2时，算子尺寸选5；

当差分阶数最大值为3时，算子尺寸选7

特殊情况：当ksize = 1时，任意一个方向的阶数需要小于3（此时使用的算子尺寸不再是正方形，而是3x1或者1x3）

最后三个参数分别为图像缩放因子、偏值和图像外推填充方法，多数情况下采用默认参数

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    //读取图像，黑白图像边缘检测结果较为明显
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg",IMREAD_ANYCOLOR);
    if (img.empty())
    {
        cout << "error" << endl;
        return -1;
    }
    Mat resultX,resultY,resultXY;
    //X方向一阶边缘
    Sobel(img,resultX,CV_16S,2,0,1);
    convertScaleAbs(resultX,resultX);
    //Y方向一阶边缘
    Sobel(img,resultY,CV_16S,0,1,3);
    convertScaleAbs(resultY,resultY);
    //整幅图像的一阶边缘
    resultXY = resultX + resultY;
    //显示图像
    imshow("resultX",resultX);
    imshow("resultY",resultY);
    imshow("resultXY",resultXY);
    waitKey(0);
    return 0;
}
```

#### 5.3 Scharr算子

虽然Sobel算子可以有效地提取图像边缘，但是对图像中较弱的边缘提取效果较差，因此，为了有效地提取出较弱的边缘

需要将像素值间的差距增大，于是引入Scharr算子，Scharr算子是对Sobel算子差异性的增强

因此两者在检测图像边缘的原理和使用方式上相同，Scharr算子的边缘检测滤波的尺寸为 3x3 ，因此也被称为Scharr滤波器

在X方向和Y方向上的边缘检测算子如

​	Gx ：                                                         ![image-20221220142725689](C:\Users\小新\AppData\Roaming\Typora\typora-user-images\image-20221220142725689.png)

Gy：

![image-20221220143008060](C:\Users\小新\AppData\Roaming\Typora\typora-user-images\image-20221220143008060.png)

OpenCV4提供了对图像提取Scharr边缘的Scharr()函数

**Scharr()函数原型：**

```cpp
void Scharr(InputArray src,
            OutputArray dst,
            int ddepth,
            int dx,
            int dy,
            double scale = 1,
            double delta = 0,
            int borderType = BORDER_DEFAULT
            )
```

* src：待边缘提取的图像
* dst：输出图像，与输入图像src具有相同的尺寸和通道数，数据类型由第三个参数ddepth控制

* ddepth：输出图像的数据类型（深度），根据输入图像的数据类型不同，拥有不同的取值范围（表：图像卷积）

​						当赋值为-1时，输出图像的数据类型自动选择

* dx：X方向的差分阶数
* dy：Y方向的差分阶数
* ksize：Sobel算子的尺寸，必须是1、3、5或者7
* scale：对导数计算结果进行缩放因子，默认系数为1，表示不缩放
* delta：偏值，在计算结果中加上偏值
* borderType：像素外推选择标志，默认为BORDER_DEFAULT，表示不包含边界值的倒序填充（2.3.4仿射变换）

该函数利用Scharr算子提取图像中边缘信息，与Sobel函数相同，前两个参数为输入图像和输出图像，第三个参数为输出图像的数据类型

由于提取边缘信息时可能出现负数，因此不要使用CV_8U数据类型的输出图像，因为于Scharr算子方向不一致的边缘梯度会在

CV_8U中消失，使得图像边缘提取不准确

第四、五个参数分别是X方向边缘和Y方向边缘的标志，该函数默认的滤波器尺寸为3x3，且无法改变

最后3个参数分别为图像缩放因子、偏值和图像外推填充方法的标志，多数情况下使用默认参数

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    //读取图像，黑白图像边缘检测结果较为明显
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg",IMREAD_ANYCOLOR);
    if (img.empty())
    {
        cout << "error" << endl;
        return -1;
    }
    Mat resultX,resultY,resultXY;
    //X方向一阶边缘
    Scharr(img,resultX,CV_16S,1,0);
    convertScaleAbs(resultX,resultX);
    //Y方向一阶边缘
    Scharr(img,resultY,CV_16S,0,1);
    convertScaleAbs(resultY,resultY);
    //整幅图像的一阶边缘
    resultXY = resultX + resultY;
    //显示图像
    imshow("resultX",resultX);
    imshow("resultY",resultY);
    imshow("resultXY",resultXY);
    waitKey(0);
    return 0;
}
```

#### 5.4生成边缘检测滤波器getDerivKernel()

Scharr算子只有两种滤波器，Sobel算子却有不同尺寸、不同阶次

在实际使用过程中，即使了解Sobel算子的原理，推导出边缘提取需要的滤波器也是非常复杂而繁琐的任务

并且，有时我们并不希望提取图像的边缘，而是希望得到能够提取图像边缘的滤波器，通过对滤波器的修改提升边缘检测的效果

OpenCV4提供了getDerivKernel()函数，通过函数可以得到不同尺寸、不同阶次的Sobel算子和Scharr算子的滤波器

**getDerivKernel()函数原型：**

```cpp
void getDerivKernel(OutputArray kx,
                    OutputArray ky,
                    int dx,
                    int dy,
                    bool normlize = flase,
                    int ktype = CV_32F
                    )
```

* kx：行滤波器系数的输出矩阵，尺寸为 ksize x 1
* ky：列滤波器系数的输出矩阵，尺寸为 ksize x 1
* dx：X方向导数的阶次
* dy：Y方向导数的阶次
* ksize：滤波器的大小，可以选择参数为FILTER_SCHARR、1、3、5或7
* normalize：是否对滤波器系数进行归一化的标志，默认值为false，表示不进行系数归一化
* ktype：滤波器系数类型，可以选择CV_32F或CV_64F，默认为CV_32F

该函数可用于生成Sobel算子和Scharr算子，实际上，Sobel()函数和Scharr()函数内部就是通过调用该函数得到边缘检测算子

该函数的前两个参数分别为行滤波器系数的输出矩阵、列滤波器系数的输出矩阵，需要将两者通过卷积分离性原理得到最终的算子

最终的边缘检测算子对图像的边缘提取效果由第三、四个参数决定

当dx=1，dy=0时，就是检测X方向的一阶梯度边缘

该函数的第五个参数是滤波器的尺寸，取值为1、3、5、7，生成的是Sobel算子，取值为FILTER_SCHARR，生成的是Scharr算子

当第五个参数为1时，第三、四个参数最大值小于3

当第五个参数为FILTER_SCHARR时，第三、四个参数取值为1或0，并且两者和为1

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    Mat sobel_x1,sobel_y1,sobel_x2,sobel_y2,sobel_x3,sobel_y3; //存放分离的Sobel算子
    Mat scharr_x,scharr_y;  //存放分离的Scharr算子
    Mat sobelX1,sobelX2,sobelX3,scharrX;//存放最终算子
    //一阶X方向Sobel算子
    getDerivKernels(sobel_x1,sobel_y1,1,0,3);
    sobel_x1 = sobel_x1.reshape(CV_8U,1);
    sobelX1 = sobel_y1*sobel_x1; //计算滤波器
    //二阶X方向Sobel算子
    getDerivKernels(sobel_x2,sobel_y2,2,0,5);
    sobel_x2 = sobel_x2.reshape(CV_8U,1);
    sobelX2 = sobel_y2*sobel_x2; //计算滤波器
    //三阶X方向Sobel算子
    getDerivKernels(sobel_x3,sobel_y3,3,0,7);
    sobel_x3 = sobel_x3.reshape(CV_8U,1);
    sobelX3 = sobel_y3*sobel_x3; //计算滤波器
    //X方向Scharr算子
    getDerivKernels(scharr_x,scharr_y,1,0,FILTER_SCHARR);
    scharr_x = scharr_x.reshape(CV_8U,1);
    scharrX = scharr_y*scharr_x; //计算滤波器
    //输出结果
    cout << "X方向一阶Sobel算子：" << endl << sobelX1 << endl;
    cout << "X方向二阶Sobel算子：" << endl << sobelX2 << endl;
    cout << "X方向三阶Sobel算子：" << endl << sobelX3 << endl;
    cout << "X方向Scharr算子：" << endl << scharrX << endl;
    waitKey(0);
    return 0;
}
```

注：提取X方向和Y方向边缘的滤波器系数是互为转置的关系

#### 5.5 Laplacian算子

上述边缘算子都具有方向性，因此需要分别求取X方向的边缘和Y方向的边缘，之后将两个方向的边缘综合得到图像的整体边缘

Laplacian算子具有各方向同性的特点，能够对任意方向的边缘进行提取，具有无方向性的优点

因此，使用Laplacian算子提取边缘不需要分别检测X方向的边缘和Y方向的边缘，只需要一次边缘检测

Laplacian算子是一种二阶导数算子，对噪声比较敏感，因此常需要配合高斯滤波一起使用

OpenCV4提供了通过Laplacian算子提取图像边缘的Laplacian()函数

**Laplacian()函数原型：**

```cpp
void Laplacian(InputArray src,
               OutputArray dst,
               int ddepth,
               int ksize = 1,
               double = scale = 1,
               double = delta = 0,
               int borderType = BORDER_DEFAULT
               )
```

* src：输入原始图像，可以为灰度图像或彩色图像
* dst：输出图像，与输入图像src具有相同的尺寸二号通道数

* ddepth：输出图像的数据类型（深度），根据输入图像的数据类型不同，拥有不同的取值范围（表：图像卷积）

​						当赋值为-1时，输出图像的数据类型自动选择

* ksize：滤波器的大小，必须为正奇数
* scale：对导数计算结果进行缩放的缩放因子，默认系数为1，表示不缩放
* delta：偏值，在计算结果中加上偏值
* borderType：像素外推选择标志，默认为BORDER_DEFAULT，表示不包含边界值的倒序填充（2.3.4仿射变换）

该函数利用Laplacian算子提取图像中的边缘信息，与Sobel()函数相同

前两个参数分别为输入图像和输出图像，第三个参数为输出图像的数据类型，由于提取边缘信息时可能出现负数，因此不要使用CV_8U数据类型的输出图像，否则会使得图像边缘提取不准确

该函数的第四个参数是滤波器的尺寸的大小，必须是正奇数

当参数值大于1时，该函数通过Sobel算子计算出图像X方向和Y方向的二阶导数，将两个方向的导数求和得到Laplacian算子

后三个参数多数情况下使用默认参数

示例程序：由于Laplacian算子对图像中的噪声较为敏感

​					因此使用Laplacian算子对高斯滤波后的图像和未进行高斯滤波的图像进行边缘检测，

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    //读取图像，黑白图像边缘检测结果较为明显
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg",IMREAD_ANYCOLOR);
    if (img.empty())
    {
        cout << "error" << endl;
        return -1;
    }
    Mat result,result_g,result_G;
    //未进行滤波提取Laplacian边缘
    Laplacian(img,result,CV_16S,3,1,0);
    convertScaleAbs(result,result);
    //滤波后提取Laplacian边缘
    GaussianBlur(img,result_g,Size(3,3),5,0); //高斯滤波
    Laplacian(result_g,result_G,CV_16S,3,1,0);
    convertScaleAbs(result_G,result_G);
    //显示图像
    imshow("result",result);
    imshow("result_G",result_G);
    waitKey(0);
    return 0;
}
```

注：图像去除噪声后通过Laplacian算子提取边缘变得更加准确

#### 5.6 Canny算法

边缘检测算法：Canny算法

该算法不容易受到噪声的影响，能够识别图像中的弱边缘和强边缘，并结合强弱边缘的位置关系，综合给出图像整体的边缘信息

Canny边缘检测算法目前是最优越的边缘检测算法之一，该方法的检测过程分为以下5步

第一步：使用高斯滤波平滑图像，减少图像中噪声，在一般情况下，使用5x5的高斯滤波器

第二步：计算图像中每个像素的梯度方向和幅值，首先通过Sobel算子分别检测图像X方向的边缘和Y方向的边缘

​				为了简便，梯度方向常取值0°、45°、90°和135°这4个角度之一

第三步：应用非极大值抑制算法消除边缘检测带来的杂散响应，首先，将当前像素的梯度强度与沿正负梯度方向上的两个像素

​				进行比较，如果当前像素的梯度强度与另外两个像素梯度强度相比最大，那么该像素点保留位边缘点，否则被抑制

第四步：应用双阈值法划分强边缘和弱边缘，将边缘处的梯度值与两个阈值进行比较，如果某像素的梯度幅值小于较小的阈值

​				那么会被去除；如果某像素的梯度幅值大于较小幅值、小于较大阈值，那么将该像素标记为弱边缘；如果某像素的梯度

​				幅值大于较大阈值，那么将该像素标记为强边缘

第五步：消除孤立的弱边缘。在弱边缘的8邻域范围寻找强边缘，如果8领域内存在强边缘，就保留该弱边缘， 否则将删除弱边缘

​				最终输出边缘检测结果

OpenCV4提供了Canny()函数实现Canny算法检测图像中的边缘

**Canny()函数原型：**

```cpp
void Canny(InputArray image,
           OuputArray edges,
           double threshold1,
           double threshold2,
           int apertureSize = 3,
           bool L2gradient = false
           )
```

* image：输入图像，必须是CV_8U的单通道或者三通道图像
* edges：输出图像，与输入图像具有相同的单通道图像，且数据类型为CV_8U
* threshold1：第一个滞后阈值
* threshold2：第二个滞后阈值
* apertureSize：Sobel算子的直径
* L2gradient：计算图像梯度幅方法的标志（两种）

该函数利用Canny算法提取图像中的边缘信息，第一个参数是需要提取边缘的输入图像，目前只支持数据类型为CV_8U的图像

输入图像可以是灰度图像或者彩色图像，第二个参数是边缘检测结果的输出图像，图像是数据类型为CV_8U的单通道灰度图像

第三、四个参数是Canny算法中用于区分强边缘和弱边缘的两个阈值，两个参数不区分较大阈值和较小阈值，函数自动区分大小

一般情况下较大阈值与较小阈值的比值在2：1到3：1之间

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    //读取图像，黑白图像边缘检测结果较为明显
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg",IMREAD_ANYDEPTH);
    if (img.empty())
    {
        cout << "error" << endl;
        return -1;
    }
    Mat resultHigh,resultLow,resultG;
    //高阈值检测图像边缘
    Canny(img,resultHigh,100,200,3);
    //低阈值检测图像边缘
    Canny(img,resultLow,20,40,3);
    //高斯模糊后检测图像边缘
    GaussianBlur(img,resultG,Size(3,3),5);
    Canny(resultG,resultG,100,200,3);
    //显示图像
    imshow("resultHigh",resultHigh);
    imshow("resultLow",resultLow);
    imshow("resultG",resultG);
    waitKey(0);
    return 0;
}
```

注：较高的阈值会降低噪声信息对图像提取边缘结果的影响，但是同时也会减少结果种的边缘结果





## 图像形态学操作

图像形态学操作主要包含图像腐蚀、膨胀、开运算与闭运算

### 1.图像像素距离变换

图像形态学在图像处理种具有广泛的应用，主要应用于从图像中提取对于表达和描述区域形状有意义的图像分量

以便后续的识别工作能够抓住对象最为本质的形状特性，例如边界、连通域等

由于图像形态学重点关注图像中物体的区域信息，忽略区域内部分纹理信息，为了方便表示图像的区域信息，加快图像形态学的处理速度

常将图像转化为二值图像后再进行图像形态学分析



在图像形态学运算中，常将不与其他区域连接的独立区域称为集合或者连通域，这个集合中的元素就是包含在连通域内的每一个像素

可以用该像素在图像中的坐标来描述，像素之间的距离可以用来表示两个连通域之间的关系

首先需要了解图像中两个像素之间的距离描述方式，以及如何从图像中分离出不同的连通域



#### 1.1图像像素距离变换distanceTransform()

图像中两个像素之间有多种定义方式，图像处理中常用的距离有欧式距离、街区距离和棋盘距离



欧式距离是指两个像素点之间的直线距离，与直角坐标系中两点之间都直线距离求取方式相同

分别计算两个像素在X方向和Y方向上的距离，之后利用勾股定理得到两个像素之间的距离，可以含有小数部分



街区距离是指两个像素点X方向和Y方向的距离之和，欧式距离表示的是两个像素点的最短距离

然而，有时我们并不能以两个点之间连线的方向前进，例如在一个城市内两点之间可能存在障碍物的阻碍，因此从一个点到另一个点需要

沿着街道行走，因此这种距离的度量方式被称为街区距离，根据定义一定为整数



棋盘距离是指两个像素点X方向距离和Y方向距离的最大值，与街区距离相似

但是，棋盘距离表示的是两个像素点移动到同一行或者同一列时需要移动的最大距离



OpenCV4中提供了用于计算图像中不同像素之间距离的distanceTransform()函数（两个原型）

**distanceTransform()函数原型1：**

```cpp
void distanceTransform(InputArray src,
                       OutputArray dst,
                       OutputArray labels,
                       int distanceType,
                       int maskSize,
                       int labelType = DIST_LABEL_CCOMP
                       )
```

* src：输入图像，数据类型为CV_8U的单通道图像
* dst：输出图像，与输入图像具有相同的尺寸，数据类型为CV_8U或CV_32F的单通道图像
* labels：二维的标签数组（离散Voronoi图），与输入图像具有相同的尺寸，数据类型为CV_32S的单通道数据
* distanceType：选择计算两个像素之间距离方法的标志，其常用的距离度量在表中
* maskkSize：距离变换掩码矩阵尺寸，参数可选**DIST_MASK_3(3x3) 和 DIST_MASK_5(5x5)**
* labelType：要构建的标签数组的类型，可以选择参数在表中

​                                                 **distanceTransform()函数中常用距离度量方法选择标志**

| 标志参数  | 简记 | 作业                                        |
| --------- | :--: | ------------------------------------------- |
| DIST_USER |  -1  | 自定义距离                                  |
| DIST_L1   |  1   | 街区距离，d = \|x1-x2\| + \|y1-y2\|         |
| DIST_L2   |  2   | 欧式距离，d = sqrt( [x1-x2]^2 + [y1-y2]^2 ) |
| DIST_C    |  3   | 棋盘距离，d = max(\|x1-x2\|,\|y1-y2\|)      |

​                                                  **distanceTransform()函数中标签数组类型标志**

| 标志参数         | 简记 | 作用                                                         |
| ---------------- | :--: | ------------------------------------------------------------ |
| DIST_LABEL_CCOMP |  0   | 输入图像中每个连接的0像素（以及最接近连接区域的所有非零像素）<br />都将被分配为相同的标签 |
| DIST_LABEL_PIXEL |  1   | 输入图像中每个0像素（以及接近它的所有非零像素）<br />都有自己的标签 |

该函数用于实现图像的距离变换，即统计图像中所有像素距离0像素的最小距离

第一个参数为待变换的输入图像，输入图像要求必须是CV_8U的单通道图像

第二个参数为输出图像，与输入图像具有相同的尺寸，图像中每个像素值表示该图像在原始图像中距离0像素的最小距离

由于图像的尺寸可能大于256，因此图像中某个像素距离0像素的最近距离可能大于255，为了正确地统计出每个像素距离0像素的

最小距离，输出图像的数据类型可以选择CV_8U或者CV_32F

第三个参数是原始图像的离散Voronoi图，输出图像是数据类型为CV_32S的单通道图像，图像尺寸与输入图像相同

第四个参数是距离变换过程中使用的距离种类，常用距离为欧式距离、街区距离和棋盘距离

第五个参数是求取路径时候的掩码矩阵尺寸，该尺寸与选择的距离种类有关，当使用街区距离时，掩码尺寸选择3x3还是5x5没有影响

因此通常默认选择3x3加快函数运算速度，当使用欧式距离时，3x3为粗略计算。5x5为精准计算，差异较大，推荐使用5x5

当使用棋盘距离时，掩码矩阵尺寸对计算结果没有影响

最后一个参数为构建标签数组的类型，当labelType == DIST_LABEL_CCOMP时，该函数会自动在输入图像中找到0像素的连通分量

并用相同的标签标记它们，当labelType == DIST_LABEL_PIXEL时，该函数扫描输入图像并用不同的标签标记所有0像素

该函数原型在对图像进行距离变换时会生成离散Voronoi图，但有时不需要使用离散图，占用了内存资源

因此distanceTransform()函数第二种原型取消了生成离散图，只输出距离变换后的图像

**distanceTransform()函数原型2：**

```cpp
void distanceTransform(InputArray src,
                       OutputArray dst,
                       int distanceType,
                       int maskSize,
                       int dstType = CV_32F)
```

* src：输入图像，数据类型为CV_8U的单通道图像
* dst：输出图像，与输入图像具有相同的尺寸，数据类型为CV_8U或CV_32F的单通道图像

* distanceType：选择计算两个像素之间距离方法的标志，其常用的距离度量在表中
* maskkSize：距离变换掩码矩阵尺寸，参数可选**DIST_MASK_3(3x3) 和 DIST_MASK_5(5x5)**
* dstType：输出图像的数据类型，可以是CV_8U或者CV_32F

该函数前4个参数相同，最后一个参数为输出图像的数据类型，虽然可以在CV_8U和CV_32F两个类型中选择，但是图像输出是实际的

数据类型与距离变换时选择的距离种类有着密切关系，CV_8U只能使用在计算街区距离时，当计算欧式距离和棋盘距离时

即使参数设置CV_8U，实际输出图像的数据类型也是CV_32F



由于distanceTransform()函数是计算图像中非零像素距离0像素的最小距离，而图像中0像素表示黑色，因此，为了保证能够清楚地

观察到距离变换的结果，不建议使用尺寸过小或者黑色区域较多的图像，否则处理后图像中几乎全为黑色，不利于观察

示例程序：首先将图像二值化，之后将二值化图像黑白像素反转，再利用distanceTransform()函数实现距离变换

​					查看图像时与原二值图像一致，但是内部数据不一致

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;
int main()
{
    //构建简易矩阵，用于求取像素之间的距离
    Mat a = (Mat_<uchar>(5,5) << 1,1,1,1,1,
                                             1,1,1,1,1,
                                             1,1,0,1,1,
                                             1,1,1,1,1,
                                             1,1,1,1,1);
    Mat dist_L1,dist_L2,dist_C,dist_L12;
    //计算街区距离
    distanceTransform(a,dist_L1,1,3,CV_8U);
    cout << "街区距离：" << endl << dist_L1 << endl;
    //计算欧式距离
    distanceTransform(a,dist_L2,2,5,CV_8U);
    cout << "欧式距离：" << endl << dist_L2 << endl;
    //计算棋盘距离
    distanceTransform(a,dist_C,3,5,CV_8U);
    cout << "棋盘距离：" << endl << dist_C << endl;
    //对图像进行距离变换
    Mat rice = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg",IMREAD_GRAYSCALE);
    if (rice.empty())
    {
        cout << "error" << endl;
        return -1;
    }
    Mat riceBW,riceBW_INV;
    //将图像转成二值图像，同时把黑白区域颜色互换
    threshold(rice,riceBW,50,255,THRESH_BINARY);
    threshold(rice,riceBW_INV,50,255,THRESH_BINARY_INV);
    //距离变换
    Mat dist,dist_INV;
    distanceTransform(riceBW,dist,1,3,CV_32F); //为了显示清晰，将数据类型变成CV_32F
    distanceTransform(riceBW_INV,dist_INV,1,3,CV_8U);
    //显示变换结果
    imshow("riceBW",riceBW);
    imshow("dist",dist);
    imshow("riceBW_INV",riceBW_INV);
    imshow("dist_INV",dist_INV);
    waitKey(0);
    return 0;
}
```

#### 1.2图像连通域分析

图像的连通域是指图像中具有相同像素值并且相邻的像素组成的区域，连通域分析是指在图像中寻找彼此独立的连通域并将其标记出来

提取图像中不同的连通域是图像处理中较为常用的方法，例如车牌识别、文字识别、目标检测等领域对感兴趣区域的分割和识别

一般情况下，一个连通域内只包含一个像素值，因此，为了防止像素值波动对提取不同连通域的影响，连通域分析常处理的是

二值化后的图像

在了解图像连通域分析方法之前，需要了解图像邻域的概念，图像中两个像素相邻有两中定义方式，分别是4-邻域和8-邻域

![](http://118.25.145.234/RM/OpenCV/an.png)

4-邻域定义下，两个像素相邻必须在水平和垂直方向上相邻

8-邻域定义下，两个像素相邻允许在对角线方向相邻

根据像素邻域定义方式不同，得到的连通域也不同，因此，在分析连通域时，一定要声明是哪种邻域下分析结果

常用的图像邻域分析法有两遍扫描法和种子填充法



两遍扫描法会遍历两次图像，第一次遍历图像时会给每一个非零像素赋予一个数字标签，当某个像素的上方和左侧邻域内的像素已经

有数字标签时，取两者中较小值作为当前像素的标签，否则赋予当前像素一个新的数字标签

在第一次遍历图像时候，同一个连通域可能会被赋予一个或者多个不同的标签

因此，第二次遍历需要将这些同一个连通域的不同标签合并，最后实现同一个邻域内的所有像素具有相同的标签



种子填充法源于计算机图像学，常用于对某些图像进行填充，该方法首先将所有非零像素放到一个集合中，之后在集合中随机选出一个

像素作为种子像素，根据邻域关系不断扩充种子像素所在的连通域，并在集合中删除扩充出的像素，直到种子所在的连通域无法扩充

之后再从集合中随机选取一个像素作为新的种子像素，重复上述过程直到集合中没有像素

##### 1.2.1connectComponents()

OpenCV4提供了用于提取图像中不同连通域的connectComponents()函数（两个原型）

**connectComponents()函数原型1：**

```cpp
int connectComponents(InputArray image,
                       OutputArray labels,
                       int connectivity,
                       int ltype,
                       int ccltype
                       )
```

* image：待标记不同连通域的单通道图像，数据类型必须为CV_8U
* labels：标记不同连通域后的输出图像，与输入图像具有相同的尺寸
* connectivity：标记连通域时使用的邻域种类，4表示4-邻域，8表示8-邻域
* ltype：输出图像的数据类型，目前支持CV_32S和CV_16U两种数据类型
* ccltype：标记连通域时使用的算法标志

​                                                     **connectComponents()函数中标记连通域算法类型可选择标志**

| 标志参数    | 简记 | 作用                                   |
| ----------- | :--: | -------------------------------------- |
| CCL_WU      |  0   | 8-邻域使用SAUF算法，4-邻域使用SAUF算法 |
| CCL_DEFAULT |  -1  | 8-邻域使用BBDT算法，4-邻域使用SAUF算法 |
| CCL_GRANA   |  1   | 8-邻域使用BBDT算法，4-邻域使用SAUF算法 |

该函数用于计算二值图像中连通域的个数，并在图像中将不同的连通域用不同的数字标签标志，其中标签0表示图像的背景区域

同时函数具有一个int类型的返回数据用于表示图像中连通域的数目

第一个参数是待标记连通域的输入图像，函数要求输入图像必须是数据类型为CV_8U的单通道灰度图像，最好是经过二值化的二值图像

第二个参数是标记连通域后的输出图像，输出图像尺寸与输入图像尺寸相同，数据类型与第四个参数有关

第三个参数是标记连通域是选择的邻域种类，该函数支持两种邻域，用4表示4-邻域，8表示8-邻域

第四个参数为输出图像的数据类型，可以选择CV_32S和CV_16U两种

最后一个参数是标记连通域是使用算法的标志，目前只支持Grana（BBDT）和Wu（SAUF）两种算法

这个函数原型所有参数都没有默认值，在调用时需要设置全部参数，增加了使用的复杂程度

因此OpenCV4提供了connectComponents()函数的简易原型，减少了参数数量，以及为部分参数增加了默认值

**connectComponents()函数原型2：**

```cpp
int connectComponents(InputArray image,
                      OutputAray labels,
                      int connectivity = 8,
                      int ltype = CV_32S
                      )
```

* image：待标记不同连通域的单通道图像，数据类型必须为CV_8U
* labels：标记不同连通域后的输出图像，与输入图像具有相同的尺寸
* connectivity：标记连通域时使用的邻域种类，4表示4-邻域，8表示8-邻域

* ltype：输出图像的数据类型，目前支持CV_32S和CV_16U两种数据类型

示例程序：首先将图像转换成灰度图像，然后将灰度图像二值化为二值图像，之后利用connectComponents()函数对图像进行连通域的统计，根据统计结果，将数字不同的标签设置成不同的颜色，以区分不同的连通域

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
#include <vector>
using namespace std;
using namespace cv;
int main()
{
    //对图像进行距离变换
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_0.jpg");
    if (img.empty())
    {
        cout << "error" << endl;
        return -1;
    }
    Mat riceBW,rice;
    //将图像转成二值图像，同时把黑白区域颜色互换
    cvtColor(img,rice,COLOR_BGR2GRAY);
    threshold(rice,riceBW,50,255,THRESH_BINARY);
    //生成随机颜色，用于区分不同的连通域
    RNG rng(10086);     //随机数种子
    Mat out;
    int number = connectedComponents(riceBW,out,8,CV_16U);  //统计图像中连通域的个数
    vector<Vec3b> colors;
    for (int i = 0; i < number; i++)
    {
        //使用均匀分布的随机数确定颜色
        Vec3b vec3 = Vec3b(rng.uniform(0,256),rng.uniform(0,256),rng.uniform(0,256));
        colors.push_back(vec3);
        //RNG类是opencv里C++的随机数产生器。它可产生一个64位的int随机数。目前可按均匀分布和高斯分布产生随机数
        //RNG::uniform( ) ：产生一个均匀分布的随机数
        //RNG::gaussian( ) ： 产生一个高斯分布的随机数
        //RNG::uniform(a, b ) 返回一个[a,b)范围的均匀分布的随机数，a,b的数据类型要一致，而且必须是int、float、double中的一种，默认是int
    }
    //用不同颜色标记不同的连通域
    Mat result = Mat::zeros(rice.size(),img.type());
    int w = result.cols;
    int h = result.rows;
    for (int row = 0; row < h; row++)
    {
        for (int col = 0; col < w; col++)
        {
            int label = out.at<uint16_t>(row,col);
            if(label == 0)  //背景为黑色不改变
            {
                continue;
            }
            result.at<Vec3b>(row,col) = colors[label];
        }
    }
    //显示结果
    imshow("原图",img);
    imshow("标记后的图像",result);
    waitKey(0);
    return 0;
}
```

注：

```cpp
//RNG类是opencv里C++的随机数产生器。它可产生一个64位的int随机数。目前可按均匀分布和高斯分布产生随机数
//RNG::uniform( ) ：产生一个均匀分布的随机数
//RNG::gaussian( ) ： 产生一个高斯分布的随机数
//RNG::uniform(a, b ) 返回一个[a,b)范围的均匀分布的随机数，a,b的数据类型要一致
//					  而且必须是int、float、double中的一种，默认是int
```

##### 1.2.2connectComponentsWithStats()

虽然connectComponents()函数可以实现图像中多个连通域的统计，但是只能通过标签将图像中不同连通域区分开

无法得到更多的统计信息，有时，我们希望得到每个连通域中心位置或者在图像中标记出连通域所在矩形区域

OpenCV4提供了connectComponentsWithStats()函数用于标记图像中不同连通域的同时统计连通域的位置、面积的信息

**connectComponentsWithStats()函数原型1：**

```cpp
int connectComponentsWithStats(InputArray image,
                               OutputArray labels,
                               OutputArray stats,
                               OutputArray centroids,
                               int connectivity,
                               int ltype,
                               int ccltype
                               )
```

* image：待标记不同连通域的单通道图像，数据类型必须为CV_8U
* labels：标记不同连通域后的输出图像，与输入图像具有相同的尺寸
* stats：含有不同连通域统计信息的矩阵，矩阵的数据类型为CV_32S，矩阵第i行是标签为i的连通域的统计特性（int读取）
* centroids：每个连通域质心的坐标，数据类型为CV_64F，（double读取）
* connectivity：统计连通域时使用的邻域种类，4表示4-邻域，8表示8-邻域
* ltype：输出图像的数据类型，目前支持CV_32S和CV_16U两种数据类型
* ccltpe：标记连通域使用的算法类型标志

该函数能够在图像中不同连通域标记标签的同时统计每个连通域的中心位置，矩形区域大小，区域面积信息

第三个参数为每个连通域统计信息矩阵，如果图像有N个连通域，那么该参数输出矩阵尺寸为Nx5，每一行分别保存每个连通域的

统计特性

第四个参数为每个连通域质心的坐标，如果有图像有N个连通域，那么输出矩阵尺寸为Nx2，矩阵每一行分别保存x、y坐标

第五个参数是统计连通域时选择的邻域种类，函数支持两种邻域，4表示4-邻域，8表示8-邻域

第六个参数为输出图像的数据类型，可以选择的参数为CV_32S和CV_16U两种

最后一个参数为标记连通域使用的算法，目前只支持Grana(BBDT)和Wu(SAUF)两种算法

​                                        **connectComponentsWithStats()函数中统计的连通域特性**

| 标记参数       | 简记 | 作用                                                         |
| -------------- | :--: | ------------------------------------------------------------ |
| CC_STAT_LEFT   |  0   | 连通域内最左侧像素的x坐标，水平方向上的包含连通域边界框的开始 |
| CC_STAT_TOP    |  1   | 连通域内最上方像素的坐标，垂直方向上的包含连通域边界框的开始 |
| CC_STAT_WIDTH  |  2   | 包含连通域边界框的水平长度                                   |
| CC_STAT_HEIGHT |  3   | 包含连通域边界框的垂直长度                                   |
| CC_STAT_AREA   |  4   | 连通域的面积（以像素为单位）                                 |
| CC_STAT_MAX    |  5   | 统计信息种类数目，无实际意义                                 |

OpenCV4提供了connectComponentsWithStats()函数的简易原型，减少了参数数目，以及增加了默认值

**connectComponentsWithStats()函数原型2：**

```cpp
int connectComponentsWithStats(InputArray image,
                               OutputArray labels,
                               OutputArray stats,
                               OutputArray centroids,
                               int connectivity = 8,
                               int ltype = CV_32F,
                               )
```

该函数与原型1参数意义相同

示例程序：将图像转换成灰度图像，然后将灰度图像化为二值图像，之后利用connectComponentsWithStats()函数对图像

​					进行连通域的统计，根据统计结果，用不同颜色的矩形将连通域围起来并标记出每个连通域的质心，标记出连通域的标签数字

​					以区分不同的连通域，最后输出每个连通域的面积

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
#include <vector>
using namespace std;
using namespace cv;
int main()
{
    //对图像进行距离变换
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_connect.png");
    if (img.empty())
    {
        cout << "error" << endl;
        return -1;
    }
    imshow("原图",img);
    Mat rice,riceBW;
    //将图像转成二值图像，同时把黑白区域颜色互换
    cvtColor(img,rice,COLOR_BGR2GRAY);
    threshold(rice,riceBW,50,255,THRESH_BINARY);
    //生成随机颜色，用于区分不同的连通域
    RNG rng(10086);     //随机数种子
    Mat out,stats,centroids;
    int number = connectedComponentsWithStats(riceBW,out,stats,centroids,8,CV_16U);  //统计图像中连通域的个数
    vector<Vec3b> colors;
    for (int i = 0; i < number; i++)
    {
        //使用均匀分布的随机数确定颜色
        Vec3b vec3 = Vec3b(rng.uniform(0,256),rng.uniform(0,256),rng.uniform(0,256));
        colors.push_back(vec3);
        //RNG类是opencv里C++的随机数产生器。它可产生一个64位的int随机数。目前可按均匀分布和高斯分布产生随机数
        //RNG::uniform( ) ：产生一个均匀分布的随机数
        //RNG::gaussian( ) ： 产生一个高斯分布的随机数
        //RNG::uniform(a, b ) 返回一个[a,b)范围的均匀分布的随机数，a,b的数据类型要一致，而且必须是int、float、double中的一种，默认是int
    }
    //用不同颜色标记不同的连通域
    Mat result = Mat::zeros(rice.size(),img.type());
    int w = result.cols;
    int h = result.rows;
    for (int i = 1; i < number; i++)
    {
        //中心位置
        int center_x = centroids.at<double>(i,0);
        int center_y = centroids.at<double>(i,1);
        //矩形边框
        int x = stats.at<int>(i,CC_STAT_LEFT);
        int y = stats.at<int>(i,CC_STAT_TOP);
        int w = stats.at<int>(i,CC_STAT_WIDTH);
        int h = stats.at<int>(i,CC_STAT_HEIGHT);
        int area = stats.at<int>(i,CC_STAT_AREA);
        //中心位置绘制
        circle(img,Point(center_x,center_y),2,Scalar(0,255,0),2,8,0);
        //外接矩形
        Rect rect(x,y,w,h);
        rectangle(img,rect,colors[i],1,8,0);
        putText(img, format("%d",i),Point(center_x,center_y),FONT_HERSHEY_SIMPLEX,0.5,Scalar(0,0,255),1);
        cout << "number:" << i << ",area：" << area << endl; //format(）函数用于格式化字符串。可以接受无限个参数，可以指定顺序。返回结果为字符串。
    }
    //显示结果
    imshow("标记后的图像",img);
    waitKey(0);
    return 0;
}
```

注：format(）函数用于格式化字符串。可以接受无限个参数，可以指定顺序。返回结果为字符串。



### 2.腐蚀和膨胀getStructuringElement()

腐蚀和膨胀是形态学的基本运算，通过这些基本运算可以去除图像中的噪声，分割出独立的区域或者将两个连通域连接在一起等

将图像二值化后通过计算图像的连通域的数目实现对图像中的计数，但是我们发现，图像两个不为0的像素也是独立的连通域

从而影响计数结果，这种面积较小的连通域可以通过腐蚀操作消除，从而减少因噪声导致的计数错误

因此腐蚀和膨胀在实际的图像处理项目中具有重要的作用

#### 2.1图像腐蚀erode()

图像的腐蚀过程与图像的卷积操作类似，都需要模板矩阵来控制运算的结果，在图像的腐蚀和膨胀中。这个模板矩阵称为结构元素

与图像卷积相同，结构元素可以任意指定图像的中心点，并且结构元素的尺寸和具体内容都可以根据需求自己定义

在定义结构元素之后，将结构元素绕着中心点旋转180°，之后将结构元素的中心点依次放到图像这种每一个非零元素处

如果此时结构元素内所有元素覆盖的图像像素值均不为0，那么保留结构元素中心点对应的图像像素，否则删除中心点对应像素



图像腐蚀过程中使用的结构元素可以根据需求自己生成

OpenCV4提供了getStructuringElement()函数用于生成常用的矩形结构元素、十字结构元素和椭圆结构元素

**getStructuringElement()函数原型：**

```cpp
Mat getStructuringElement(int shape,
                          Size ksize,
                          Point anchor = Point(-1,-1)
                          )
```

* shape：生成结构元素的种类
* ksize：结构元素的尺寸
* anchor：中心点的位置，默认为结构元素的几何中心

该函数生成常用的矩形结构元素、十字结构元素和椭圆结构元素

第一个参数为生成结构元素的种类

第二个参数是结构元素的尺寸，能够影响图像腐蚀的效果，一般情况下，当结构元素的种类相同时，结构元素的尺寸越大，腐蚀效果越

明显

最后一个参数是结构元素的中心点位置，只有十字结构元素的中心点位置会影响图像腐蚀后的轮廓形状

其他种类的结构元素只影响形态学操作结构的平移量

​                                              **getStructuringElement()函数结构元素的种类元素的可选择参数**

| 标记参数      | 简记 | 作用                                |
| ------------- | :--: | ----------------------------------- |
| MORPH_RECT    |  0   | 矩形结构元素，所有元素都为1         |
| MORPH_CROSS   |  1   | 十字结构元素，中间的列和行元素都为1 |
| MORPH_ELLIPSE |  2   | 椭圆结构元素，矩形的内接椭圆元素为1 |

OpenCV4提供了用于图像腐蚀erode()函数

**erode()图像腐蚀：**

```cpp
void erode(InputArray src,
           OutputArray dst,
           InputArray kernel,
           Point anchor = Point(-1,-1),
           int iterations = 1, 
           int borderType = BORDER_CONSTANT,
           const Scalar& borderValue = morphologyDefaultBorderValue()
           )
```

* src：输入的待腐蚀图像，图像通道数任意，但图像数据类型必须是CV_8U、CV_16U、CV_16S、CV_32F或CV_64F之一
* dst：腐蚀后的输出图像，与输入图像src具有相同的尺寸和数据类型
* kernel：用于腐蚀操作的结构元素，可以自己定义，也可以用getStructuringElement()函数生成
* anchor：中心点的位置，默认为结构元素的几何中心
* iterations：腐蚀的次数，默认值为1
* borderType：像素外推选择标志，默认为BORDER_DEFAULT，表示不包含边界值的倒序填充（2.3.4仿射变换）
* borderValue：使用边界不变外推法时的边界值

该函数根据结构元素对输入图像进行腐蚀，在腐蚀多通道图像时，每个通道独立进行腐蚀运算

第三个参数为结构元素

第四个参数为结构元素的中心位置，默认值Point（-1，-1），表示几何中心

第五个参数为腐蚀次数，默认值为1

最后两个参数对图像主要腐蚀操作没有影响，因此多数情况下使用默认参数

该函数的腐蚀过程只针对图像中的非零像素，因此，如果图像背景为0，那么腐蚀过后图像中内容变得更小、瘦

如果图像以255为背景，那么腐蚀操作后内容变得更粗、大

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
#include <vector>
using namespace std;
using namespace cv;
//绘制包含区域函数
void drawState(Mat &img,int number,Mat centroids,Mat stats,string str)
{
    RNG rng(10086);
    vector<Vec3b> colors;
    for (int i = 0; i < number; i++)
    {
        //使用均匀分布的随机数确定颜色
        Vec3b vec3 = Vec3b(rng.uniform(0, 256), rng.uniform(0, 256), rng.uniform(0, 256));
        colors.push_back(vec3);
    }
    for (int i = 1; i < number; i++)
    {
        //中心位置
        int center_x = centroids.at<double>(i, 0);
        int center_y = centroids.at<double>(i, 1);
        //矩形边框
        int x = stats.at<int>(i, CC_STAT_LEFT);
        int y = stats.at<int>(i, CC_STAT_TOP);
        int w = stats.at<int>(i, CC_STAT_WIDTH);
        int h = stats.at<int>(i, CC_STAT_HEIGHT);
        int area = stats.at<int>(i, CC_STAT_AREA);
        //中心位置绘制
        circle(img, Point(center_x, center_y), 2, Scalar(0, 255, 0), 2, 8, 0);
        //外接矩形
        Rect rect(x, y, w, h);
        rectangle(img, rect, colors[i], 1, 8, 0);
        putText(img, format("%d", i), Point(center_x, center_y), FONT_HERSHEY_SIMPLEX, 0.5, Scalar(0, 0, 255), 1);
        cout << "number:" << i << ",area：" << area << endl; //format(）函数用于格式化字符串。可以接受无限个参数，可以指定顺序。返回结果为字符串。
    }
    imshow(str,img);
}
int main()
{
    //生成用于腐蚀的原图
    Mat src = (Mat_<uchar>(6,6) << 0,0,0,0,255,0,
                                               0,255,255,255,255,255,
                                               0,255,255,255,255,0,
                                               0,255,255,255,255,0,
                                               0,255,255,255,255,0,
                                               0,0,0,0,0,0);
    Mat struct1,struct2;
    struct1 = getStructuringElement(0,Size(9,9)); //矩形结构元素
    struct2 = getStructuringElement(1,Size(3,3)); //十字结构元素
    Mat erodeSrc; //存放腐蚀后的图像
    erode(src,erodeSrc,struct2);
    namedWindow("src",WINDOW_GUI_NORMAL);
    namedWindow("erodeSrc",WINDOW_GUI_NORMAL);
    imshow("src",src);
    imshow("erodeSrc",erodeSrc);
    Mat LearnCV_black = imread("E:\\CLion\\opencv_xin\\SG\\SG_black.png",IMREAD_ANYCOLOR);
    Mat LearnCV_write = imread("E:\\CLion\\opencv_xin\\SG\\SG_write.png",IMREAD_ANYCOLOR);
    Mat erode_black1,erode_black2,erode_write1,erode_write2;
    //黑色背景图像腐蚀
    erode(LearnCV_black,erode_black1,struct1);
    erode(LearnCV_black,erode_black2,struct2);
    imshow("LearnCV_black",LearnCV_black);
    imshow("erode_black1",erode_black1);
    imshow("erode_black2",erode_black2);
    //白色背景图像腐蚀
    erode(LearnCV_write,erode_write1,struct1);
    erode(LearnCV_write,erode_write2,struct2);
    imshow("LearnCV_write",LearnCV_write);
    imshow("erode_write1",erode_write1);
    imshow("erode_write2",erode_write2);
    //验证腐蚀对小连通域的去除
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_connect.png");
    Mat img2;
    copyTo(img,img2,img); //复制一个单独的图像，用于后期图像绘制
    Mat rice,riceBW;
    //将图像转成二值图像，同时把黑白区域颜色互换
    cvtColor(img,rice,COLOR_BGR2GRAY);
    threshold(rice,riceBW,50,255,THRESH_BINARY);
    Mat out,stats,centroids;
    int number = connectedComponentsWithStats(riceBW,out,stats,centroids,8,CV_16U);  //统计图像中连通域的个数
    drawState(img,number,centroids,stats,"未腐蚀时统计连通域");  //绘制图像
    erode(riceBW,riceBW,struct1);   //对图像进行腐蚀
    number = connectedComponentsWithStats(riceBW,out,stats,centroids,8,CV_16U);  //统计图像中连通域的个数
    drawState(img2,number,centroids,stats,"腐蚀后统计连通域");  //绘制图像
    waitKey(0);
    return 0;
}
```

#### 2.2图像膨胀dilate()

相比于图像腐蚀，图像膨胀是其相反的过程，与图像腐蚀相似，图像膨胀同样需要结果元素用于控制图像膨胀的效果

结构元素可以任意指定图像的中心点，并且结构元素的尺寸和具体内容都可以根据需求自己定义

如果原图中某个元素被结构元素覆盖，但是该像素的像素值不与中心点对应像素点的像素值相同，那么将原图中该像素改为中心点像素值

OpenCV4提供了用于图像膨胀的dilate()函数

**dilate()函数原型：**

```cpp
void dilate(InputArray src,
            OutputArray dst,
            InputArray kernel,
            Point anchor = Point(-1,-1),
            int iterations = 1, 
            int borderType = BORDER_CONSTANT,
            const Scalar& borderValue = morphologyDefaultBorderValue()
            )
```

* src：输入的待膨胀图像，图像通道数任意，但图像数据类型必须是CV_8U、CV_16U、CV_16S、CV_32F或CV_64F之一
* dst：膨胀后的输出图像，与输入图像src具有相同的尺寸和数据类型
* kernel：用于膨胀操作的结构元素，可以自己定义，也可以用getStructuringElement()函数生成
* anchor：中心点的位置，默认为结构元素的几何中心
* iterations：膨胀的次数，默认值为1
* borderType：像素外推选择标志，默认为BORDER_DEFAULT，表示不包含边界值的倒序填充（2.3.4仿射变换）
* borderValue：使用边界不变外推法时的边界值

该函数参数与腐蚀意义相同

该函数的膨胀过程只针对图像中的非零像素，因此，如果图像背景为255，那么膨胀过后图像中内容变得更小、瘦

如果图像以0为背景，那么膨胀操作后内容变得更粗、大

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
#include <vector>
using namespace std;
using namespace cv;
int main()
{
    //生成用于腐蚀的原图
    Mat src = (Mat_<uchar>(6,6) << 0,0,0,0,255,0,
            0,255,255,255,255,255,
            0,255,255,255,255,0,
            0,255,255,255,255,0,
            0,255,255,255,255,0,
            0,0,0,0,0,0);
    Mat struct1,struct2;
    struct1 = getStructuringElement(0,Size(9,9)); //矩形结构元素
    struct2 = getStructuringElement(1,Size(3,3)); //十字结构元素
    Mat erodeSrc; //存放膨胀后的图像
    dilate(src,erodeSrc,struct2);
    namedWindow("src",WINDOW_GUI_NORMAL);
    namedWindow("dilateSrc",WINDOW_GUI_NORMAL);
    imshow("src",src);
    imshow("dilateSrc",erodeSrc);

    Mat LearnCV_black = imread("E:\\CLion\\opencv_xin\\SG\\SG_black.png",IMREAD_ANYCOLOR);
    Mat LearnCV_write = imread("E:\\CLion\\opencv_xin\\SG\\SG_write.png",IMREAD_ANYCOLOR);
    Mat dilate_black1,dilate_black2,dilate_write1,dilate_write2;
    //黑色背景图像膨胀
    dilate(LearnCV_black,dilate_black1,struct1);
    dilate(LearnCV_black,dilate_black2,struct2);
    imshow("LearnCV_black",LearnCV_black);
    imshow("dilate_black1",dilate_black1);
    imshow("dilate_black2",dilate_black2);
    //白色背景图像膨胀
    dilate(LearnCV_write,dilate_write1,struct1);
    dilate(LearnCV_write,dilate_write2,struct2);
    imshow("LearnCV_write",LearnCV_write);
    imshow("dilate_write1",dilate_write1);
    imshow("dilate_write2",dilate_write2);
    waitKey(0);
    return 0;
}
```

### 3.形态学应用morphologyEx()

图像形态学腐蚀可以将细小的噪声区域去除，但是会将图像主要区域的面积缩小，造成主要区域的形状发生改变；

图像形态学膨胀可以扩充每一个区域的面积，填充较小的空洞。但是会增加噪声的面积

结合两者的特性，将图像的腐蚀和膨胀适当结合，便可以即去除图像的噪声，又不缩小图像中主要区域的面积；

即填充较小的空洞，又不增加噪声所占的面积

#### 3.1开运算

图像开运算可以去除图像中的噪声，消除较小连通域，保留较大连通域，同时能够在两个物体纤细的连接处将物体分离

并且在不明显改变较大连通域面积的同时平滑连通域的边界

开运算是图像腐蚀和膨胀操作的结合，首先对图像腐蚀，消除图像中的噪声和较小的连通域，之后通过膨胀运算弥补

较大连通域因腐蚀而造成的面积减小

![](http://118.25.145.234/RM/OpenCV/xxxxx.png)

同时，可以去除较小连通域，保留较大连通域

开运算是对图像腐蚀和膨胀的组合

OpenCV4没有提供只用于图像开运算的函数，而是提供了图像腐蚀和膨胀运算不同组合形式的morphologyEx()函数

以实现图像的开运算、闭运算、形态学梯度、顶帽运算、黑帽运算，以及击中击不中变换

**morphologyEx()函数原型：**

```cpp
void morphologyEx(InputArray src,
            	  OutputArray dst,
                  int op,
            	  InputArray kernel,
            	  Point anchor = Point(-1,-1),
            	  int iterations = 1, 
            	  int borderType = BORDER_CONSTANT,
            	  const Scalar& borderValue = morphologyDefaultBorderValue()
                  )
```

* src：输入图像，图像通道数任意，但图像数据类型必须是CV_8U、CV_16U、CV_16S、CV_32F或CV_64F之一
* dst：形态学操作后的输出图像，与输入图像src具有相同的尺寸和数据类型
* op：形态学操作类型的标志
* kernel：结构元素，可以自己定义，也可以用getStructuringElement()函数生成
* anchor：中心点的位置，默认为结构元素的几何中心
* iterations：处理的次数，默认值为1
* borderType：像素外推选择标志，默认为BORDER_DEFAULT，表示不包含边界值的倒序填充（2.3.4仿射变换）
* borderValue：使用边界不变外推法时的边界值

该函数根据结构元素对输入图像进行多种形态学操作，在处理多通道图像时，每个通道独立进行处理

第三个参数是形态学操作类型的标志，可以选择的有开运算、闭运算、形态学梯度、顶帽运算、黑帽运算，以及击中击不中变换

第四、五个参数都是与结构元素相关的参数

第四个参数是结构元素，使用的结构元素尺寸越大，效果越明显

第五个参数是结构元素的中心位置，默认值为Point（-1，-1）、

第六个参数是处理次数，处理次数越多，效果越明显

最后两个参数对图像主要部分的形态学操作没有影响，多数情况下使用默认值

​                                              **morphologyEx()函数中形态学操作类型的标志及其含义**

| 标志参数       | 简记 | 作用           |
| -------------- | :--: | -------------- |
| MORPH_ERODE    |  0   | 图像腐蚀       |
| MORPH_DILATE   |  1   | 图像膨胀       |
| MORPH_OPEN     |  2   | 开运算         |
| MORPH_CLOSE    |  3   | 闭运算         |
| MORPH_GRADIENT |  4   | 形态学梯度     |
| MORPH_TOPHAT   |  5   | 顶帽运算       |
| MORPH_BLACKHAT |  6   | 黑帽运算       |
| MORPH_HITMISS  |  7   | 击中击不中运算 |

#### 3.2闭运算

图像闭运算可以去除连通域内的小型空洞，平滑物体轮廓，连接两个临近的连通域，闭运算是图像腐蚀和膨胀操作的结合

首先对图像进行膨胀,填充连通域内的小型空洞，扩大连通域的边界，将临近的两个连通域连接，之后通过腐蚀运算减少由膨胀运算

引起的连通域边界扩大以及面积的增加

闭运算是对图像膨胀和腐蚀的组合

OpenCV4提供的morphologyEx()函数可以选择闭运算参数**MORPH_CLOSE**实现图像的闭运算

#### 3.3形态学梯度

形态学梯度能够描述目标的边界，根据图像腐蚀和和膨胀与原图之间的关系计算得到

形态学梯度可以分为基本梯度、内部梯度和外部梯度

基本梯度是原图像膨胀后图像与腐蚀后图像间的差值图像

内部梯度是原图像与腐蚀后图像间的差值图像

外部梯度是膨胀后与原图像间的差值图像

OpenCV4提供的morphologyEx()函数可以选择闭运算参数**MORPH_GRADIENT**实现图像的基本梯度

#### 3.4顶帽运算

图像顶帽运算是原图像与开运算结果之间的差值，往往用来分离比邻边点亮一些的斑块

因为开运算带来的结果是放大了裂缝或者局部低亮度的区域，因此，从原图中减去开运算后的图，得到的效果图突出了

比原图轮廓周围的区域更明亮的区域

顶帽运算先对图像进行开运算，之后从原图像中减去开运算的结果

OpenCV4提供的morphologyEx()函数可以选择闭运算参数**MORPH_TOPHAT**实现图像的顶帽运算

#### 3.5黑帽运算

图像黑帽运算是与图像顶帽运算相对应的形态学操作，与顶帽运算不同，黑帽运算是原图像与顶帽运算结果之间的差值

往往用来分离比邻边点暗一些的斑块，黑帽运算首先对图像进行闭运算，之后从闭运算结果中减去原图

OpenCV4提供的morphologyEx()函数可以选择闭运算参数**MORPH_BLACKHAT**实现图像的黑帽运算

#### 3.6击中击不中变换

击中击不中变换是比图像腐蚀要求更加苛刻的一种形态学操作，图像腐蚀只需要图像能够将结构元素中所有非零元素包含

但是击中击不中变换要求原图像中需要存在于结构元素一模一样的结构，即结构元素中非零元素也需要同时被考虑

但是，在使用矩形结构元素时，击中击不中变换于图像腐蚀结构相同

OpenCV4提供的morphologyEx()函数可以选择闭运算参数**MORPH_HITMISS**实现图像的击中击不中变换



示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
#include <vector>
using namespace std;
using namespace cv;
int main()
{
    //用于验证形态学应用的二值化矩阵
    Mat src = (Mat_<uchar>(9,12) << 0,0,0,0,0,0,0,0,0,0,0,0,
                                    0,255,255,255,255,255,255,255,0,0,255,0,
                                    0,255,255,255,255,255,255,255,0,0,0,0,
                                    0,255,255,255,255,255,255,255,0,0,0,0,
                                    0,255,255,255,0,255,255,255,0,0,0,0,
                                    0,255,255,255,255,255,255,255,0,0,0,0,
                                    0,255,255,255,255,255,255,255,0,0,255,0,
                                    0,255,255,255,255,255,255,255,0,0,0,0,
                                    0,0,0,0,0,0,0,0,0,0,0,0);
    namedWindow("src",WINDOW_GUI_NORMAL);   //可以自由调节显示图像的尺寸
    imshow("src",src);
    //3x3矩形结构元素
    Mat kernel = getStructuringElement(0,Size(3,3));
    //对二值化矩形进行形态学操作
    Mat open,close,gradient,tophat,blackhat,hitmiss;
    //对二值化矩阵进行开运算
    morphologyEx(src,open,MORPH_OPEN,kernel);
    namedWindow("open",WINDOW_GUI_NORMAL);  //可以自由调节显示图像的尺寸
    imshow("open",open);
    //对二值化矩阵进行闭运算
    morphologyEx(src,close,MORPH_CLOSE,kernel);
    namedWindow("close",WINDOW_GUI_NORMAL);  //可以自由调节显示图像的尺寸
    imshow("close",close);
    //对二值化矩阵进行形态学梯度运算
    morphologyEx(src,gradient,MORPH_GRADIENT,kernel);
    namedWindow("gradient",WINDOW_GUI_NORMAL);  //可以自由调节显示图像的尺寸
    imshow("gradient",gradient);
    //对二值化矩阵进行顶帽运算
    morphologyEx(src,tophat,MORPH_TOPHAT,kernel);
    namedWindow("tophat",WINDOW_GUI_NORMAL);  //可以自由调节显示图像的尺寸
    imshow("tophat",tophat);
    //对二值化矩阵进行黑帽运算
    morphologyEx(src,blackhat,MORPH_BLACKHAT,kernel);
    namedWindow("blackhat",WINDOW_GUI_NORMAL);  //可以自由调节显示图像的尺寸
    imshow("blackhat",blackhat);
    //对二值化矩阵进行击中击不中运算
    morphologyEx(src,hitmiss,MORPH_HITMISS,kernel);
    namedWindow("hitmiss",WINDOW_GUI_NORMAL);  //可以自由调节显示图像的尺寸
    imshow("hitmiss",hitmiss);
    waitKey(0);
    return 0;
}
```

#### 3.7图像细化

图像细化是将图像的线条从多像素宽度减少到单位像素宽度的过程，有时又称为“骨架化”或者“中轴变化”

图像细化是模式识别领域重要的出来步骤之一，常用在文字识别中，因为其可以有效将文字细化，增加文字的可识别度

并且能够有效地减少数据量，降低图像的存储难度

图像细化一般要求细化后骨架的连通性、对原图像的细节特征要有较好保留、线条的端点保留完好的同时线条交叉点不能发生畸变

根据图像细化后的特性，并非所有形状的图像都适合进行细化，其主要应用在由线条形状组成的物体，例如圆环、文字等

但是实心圆不适合进行细化

根据算法处理步骤不同，细化算法又分为迭代细化算法和非迭代细化算法

根据检测像素方法的不同，迭代细化算法又分为串行细化算法和并行细化算法

OpenCV4提供了用于将二值图像细化的thinning()函数

**thinning()函数原型：**

```cpp
void cv::ximgprc::thinning(InputArray src,
                           OutputArray dst,
                           int thinningType = THINNING_ZHANGSUEN
                           )
```

* src：输入图像，必须是CV_8U的单通道图像
* dst：输出图像，与输入图像具有相同的尺寸和数据类型
* thinningType：细化算法标志，可以选择参数为**THINNING_ZHANGSUEN**（简记为0）和**THINNING_GUOHALL**（简记为1）

该函数可以对图像的连通域进行细化，得到单位像素宽度的连通域

第一个参数是输入图像，要求必须是CV_8U的单通道图像

第二个参数是输出图像，与输入图像具有相同的尺寸和数据类型

第三个参数为细化算法标志，一种是在并行细化算法中常用的Zhang算法可以用**THINNING_ZHANGSUEN**（简记为0）表示

另一种是Guo细化方法，可以用**THINNING_GUOHALL**（简记为1）表示

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
#include <opencv2/ximgproc.hpp>     //细化函数thinning()所在头文件
using namespace std;
using namespace cv;
int main()
{
    //对字符进行细化
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_black.png",IMREAD_ANYDEPTH);	//以8位读取
    if (img.empty())
    {
        cout << "error" << endl;
        return -1;
    }
    //对字符、实心圆和圆环进行细化
    Mat words = Mat::zeros(100,200,CV_8UC1);    //创建一个黑色的背景图片
    putText(words,"Learn",Point(30,30),2,1, Scalar(255),2); //添加字符
    putText(words,"OpenCV 4",Point(30,60),2,1, Scalar(255),2); //添加字符
    circle(words,Point(80,75),10,Scalar(255),-1);   //添加实心圆
    circle(words,Point(130,75),10,Scalar(255),3);   //添加圆环
    //进行细化
    Mat thin1,thin2;
    ximgproc::thinning(img,thin1,0); //注意类名
    ximgproc::thinning(words,thin2,0); //注意类名
    //显示处理结果
    imshow("thin1",thin1);
    imshow("img",img);
    namedWindow("thin2",WINDOW_GUI_NORMAL);
    imshow("thin2",thin2);
    namedWindow("words",WINDOW_GUI_NORMAL);
    imshow("words",words);
    waitKey(0);
    return 0;
}
```





# OpenCV C++ 应用

## 目标检测

图像中物体的形状信息是较为明显和重要的信息，可以通过对形状的识别实现对物体的检测

因此，检测图像中某些规则的形状是图像处理的重要方法，通过检测形状目标的位置

并通过对目标大小、位置等信息的处理进一步理解图像中的重要信息



### 2.轮廓检测

图像轮廓是指图像中对象的边界，是图像目标的外部特征

这个特征对于图像分析，目标识别和理解更深层次的含义具有重要作用

#### 2.1轮廓发现与绘制

图像的轮廓不但能够提供物体的边缘，而且能够提供物体边缘之间的层次关系及拓扑关系

我们可以将图像轮廓发现简单理解为带有结构关系的边缘检测，这种结构关系可以表明图像中连通域或者某些区域之间的关系

一个具有4个不连通边缘的二值化图像，由外到内依次为0号、1号、2号、3号条边缘

为了描述不同轮廓之间的结构关系，定义由外到内的轮廓级别越来越低，也就是高一层级的轮廓包围着较低层级的轮廓，被同一个轮廓包围的多个不互相包含的轮廓是同一层级轮廓

0号轮廓层级比1号和第2号轮廓的层及都要高，2号轮廓包围着3号轮廓，因此2号轮廓的层级要高于3号轮廓

![](http://118.25.145.234/RM/OpenCV/zxc.png)

为了更够更好的表明各个轮廓之间的层级关系，常用4个参数来描述不同层级之间的结构关系

这4个参数分别是：同层下一个轮廓索引、同层上一个轮廓索引、下一层第一个子轮廓索引和上层父轮廓索引

根据这种描述方式，0号轮廓没有同级轮廓和父轮廓需要用-1表示，其第一个子轮廓为1号轮廓

因此可以用[  -1 -1  1  -1 ]描述该轮廓的结构（0号父轮廓）

1号轮廓的下一个同级轮廓为2号轮廓但是没有上一个同级轮廓用-1表示，父轮廓为0号轮廓，第一个子轮廓为3号轮廓，因此可以用

[ 2  -1  3  0 ]（1号轮廓）描述该轮廓结构2号轮廓和3号轮廓同样可以用这样的方式构建结构关系



![](http://118.25.145.234/RM/OpenCV/zxcv.png)

OpenCV4提供了可以在二值图像中检测图像所有轮廓并生成不同轮廓结构关系的findContours()函数

##### 2.1.1 findContours()

**findContours()函数原型1：**

```cpp
void findContours(InputArray image,
                  OutputArrayOfArrays contours,
                  OutputArray hierarchy,
                  int mode,
                  int method,
                  Point offset = Point()
                  )
```

* image：输入图像，数据类型为CV_8U的单通道灰度图像或者二值化图像

* contours：检测到的轮廓，每个轮廓中存放这像素的坐标
* hierarchy：轮廓结构关系描述向量
* mode：轮廓检测模式标志
* method：轮廓逼近方法标志
* offset：每个轮廓点移动的可选偏移量，这个参数主要用在从ROI图像中找出轮廓并基于整个图像分析轮廓的场景中

该函数主要用于检测图像中的轮廓信息，并输出各个图像轮廓之间的结构信息

第一个参数是待检测轮廓的输入图像，理论是讲，需要是二值化图像，但是该函数会将非零像素视为1，0像素不变

因此可以接受非二值化的灰度图像，由于该函数默认二值化操作不能保持图像主要内容，因此常需要对图像进行预处理

利用threshold()函数或者adaptiveThreshold()函数根据需求进行二值化

第二个参数用于存放检测到的轮廓，数据类型为 vector<vector< Point >> ，每个轮廓中存放着属于该轮廓的像素坐标

第三个参数用于存放各个轮廓之间的结构信息，数据类型为 <vector< Vec4i >>，数据的尺寸与检测到的轮廓数目相同

每个轮廓结构信息中第一个数据是同层下一个轮廓索引，第二个数据是同层上一个轮廓索引，第三个数据是下一层第一个子轮廓索引

第四个数据是上层父轮廓索引

第四个参数是轮廓检测模式标志

第五个参数是轮廓逼近方法标志

最后一个参数是轮廓点移动的可选偏移量，主要用在从ROI图像中找出轮廓并基于整个图像分析轮廓的场景中

​                                                  **findContours()函数轮廓检测模式标志可选择参数**

| 标志参数      | 简记 | 含义                                                         |
| ------------- | :--: | ------------------------------------------------------------ |
| RETR_EXTERNAL |  0   | 只检测最外层轮廓，对所有轮廓设置hierarchy[i] [2]= -1         |
| RETR_LIST     |  1   | 提取所有轮廓，并且放置在list中，检测的轮廓不建立等级关系     |
| RETR_CCOMP    |  2   | 提取所有轮廓，并且将其组织为双层结构，顶层为连通域的外围边界<br />次层为孔的内层边界 |
| RETR_TREE     |  3   | 提取所有轮廓，并重新建立网状的轮廓结构                       |

​                                                **findContours()函数轮廓逼近方法标志可选择标志**

| 标志参数               | 简记 | 含义                                                         |
| ---------------------- | :--: | ------------------------------------------------------------ |
| CHAIN_APPROX_NONE      |  1   | 获取每个轮廓的每个像素，相邻两个点的像素位置相差1，<br />即max(ads(x1-x2),ads(y2-y1)) = 1 |
| CHAIN_APPROX_SIMPLE    |  2   | 压缩水平方向、垂直方向和对角线方向的元素，只保留该方向<br />的终点坐标，例如一个矩形只需要4个点来保持轮廓信息 |
| CHAIN_APPROX_TC89_L1   |  3   | 使用The-Chinl链逼近算法中的一个                              |
| CHAIN_APPROX_TC89_KCOS |  4   | 使用The-Chinl链逼近算法中的一个                              |

有时，我们只需要检测图像的轮廓，并不关心轮廓之间的结构关系信息，此时轮廓之间的结构信息关系变量会造成内存的浪费

因此OpenCV4提供了findContours()函数的另一种原型，可以不输出轮廓之间的结构关系信息

**findContours()函数原型2：**

```cpp
void findContours(InputArray image,
                  OutputArrayOfArrays contours,
                  int mode,
                  int method,
                  Point offset = Point()
                  )
```

* image：输入图像，数据类型为CV_8U的单通道灰度图像或者二值化图像
* contours：检测到的轮廓，每个轮廓中存放这像素的坐标
* mode：轮廓检测模式标志
* method：轮廓逼近方法标志
* offset：每个轮廓点移动的可选偏移量，这个参数主要用在从ROI图像中找出轮廓并基于整个图像分析轮廓的场景中

##### 2.1.2drawContours()

在提取图像轮廓后，为了能够直观地查看轮廓检测的结果，OpenCV4提供了显示轮廓的drawContours()函数

**drawContours()函数原型：**

```cpp
void drawContours(InputArray image,
                  OutputArrayOfArrays contours,
                  int contourIdx,
                  const Scalar& color,
                  int thickness = 1,
                  int lineType = LINE_8,
                  InputArray hierarchy = noArray(),
                  int maxLevel = INT_MAX,
                  Point offset = Point()
                  )
```

* image：绘制轮廓的目标图像
* contours：所有将要绘制的轮廓
* contourIdx：要绘制轮廓的数目，如果是负数，那么绘制所有的轮廓
* color：绘制轮廓的颜色
* thickness：绘制轮廓的线条粗细，如果参数为负数，那么绘制轮廓的内部，默认参数值为1
* lineType：边界线的连接类型，默认参数值为LINE_8
* hierarchy：可选的结构关系信息，默认值为noArray()
* maxLevel：表示绘制轮廓的最大等级，默认值为INF_MAX
* offset：可选的轮廓偏移参数，按指定的移动距离绘制所有轮廓

该函数用于绘制findContours()函数检测到的图像轮廓

第一个参数为绘制轮廓的图像，可以是单通道的灰度图像或者三通道的彩色图像

第二个参数是所有将要绘制的轮廓，数据类型为vector<vector< Point >>

第三个参数是要绘制的轮廓数目，与第二个参数相对应，应小于所有轮廓的数目，参数为负数，那么绘制所有轮廓

第四个参数是绘制轮廓的颜色，对于单通道的灰度图像，使用Scalar(x)赋值，对于三通道，使用Scalar(x,y,z)赋值

第六个参数是边界线的连接类型，默认参数为LINE_8

第七个参数是可选的结构关系信息，默认值为noArray()

第八个参数表示绘制轮廓的最大等级，如果参数值为0，那么仅绘制指定的轮廓，如果参数值为1，那么绘制轮廓和所有嵌套轮廓

如果参数值为2，那么绘制轮廓，以及所有嵌套轮廓和所有嵌套到嵌套轮廓的轮廓，以此类推，默认值为INT_MAX

最后一个参数是可选的轮廓偏移参数，按指定的移动距离绘制所有轮廓

​                                                              **drawContours()函数边界线的连接类型**

| 类型    | 简记 | 含义       |
| ------- | :--: | ---------- |
| LINE_4  |  1   | 4连通类型  |
| LINE_8  |  3   | 8连通类型  |
| LINE_AA |  4   | 抗锯齿线型 |

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
#include <vector>
using namespace std;
using namespace cv;
int main()
{
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_connect.png");
    if (img.empty())
    {
        cout << "error" << endl;
        return -1;
    }
    imshow("原图",img);
    Mat gray,binary;
    cvtColor(img,gray,COLOR_BGR2GRAY);  //转化成灰度图
    GaussianBlur(gray,gray,Size(13,13),4,4);    //平滑滤波
    threshold(gray,binary,170,255,THRESH_BINARY | THRESH_OTSU); //自适应二值化
    imshow("a",binary);
    //轮廓发现与绘制
    vector<vector<Point>> contours; //轮廓
    vector<Vec4i> hierarchy;    //存放轮廓结构变量
    findContours(binary,contours,hierarchy,RETR_TREE,CHAIN_APPROX_SIMPLE,Point());
    //绘制轮廓
    for (int i = 0; i < contours.size(); i++)
    {
        drawContours(img,contours,i,Scalar(0,0,255),2,8);       //每个i对于了一个轮廓
    }
    //输出轮廓结构描述
    for (int i = 0; i < hierarchy.size(); i++)
    {
        cout << hierarchy[i] << endl;
    }
    //显示结果
    imshow("轮廓检测结果",img);
    waitKey(0);
    return 0;
}
```

#### 2.2轮廓面积contourArea()

轮廓面积是轮廓的重要统计特性之一

通过轮廓面积可以进一步分析每个轮廓隐含的信息，例如通过轮廓面积区分物体大小、识别不同的物体等

轮廓面积是指每个轮廓中所有的像素点围成区域的面积，单位为像素

OpenCV提供了检测轮廓面积的contourArea()函数

**contourArea()函数原型：**

```cpp
double contourArea(InputArray contour,
                   bool oriented = false
                   )
```

* contour：轮廓的像素点

* oriented：区域面积是否具有方向的标志，true表示面积具有方向性，false表示面积不具有方向性

    			     默认值为面积不具有方向性的false

该函数用于统计轮廓像素点围成区域的面积，返回值是统计轮廓面积的结果，数据类型为double

第一个参数表示轮廓的像素点，数据类型为vector< Point >或者Mat，相邻的两个像素点之间逐一相连构成的多边形区域

即为轮廓面积的统计区域，连续的3个像素连线可能在同一条直线上，因此，为了减少输入轮廓像素点的数目，可以只输入轮廓的顶点

像素点，例如一个三角形的轮廓，轮廓中可能具有每一条边上的所有像素点，但是，在统计面积时，可以只输入三角形的3个顶点

第二个参数是区域面积是否具有方向的标志，轮廓顺时针给出和逆时针给出时统计的面积互为相反数

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
#include <vector>
using namespace std;
using namespace cv;
int main()
{
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_connect.png");
    if (img.empty())
    {
        cout << "error" << endl;
        return -1;
    }
    imshow("原图",img);
    Mat gray,binary;
    cvtColor(img,gray,COLOR_BGR2GRAY);  //转化成灰度图
    GaussianBlur(gray,gray,Size(13,13),4,4);    //平滑滤波
    threshold(gray,binary,170,255,THRESH_BINARY | THRESH_OTSU); //自适应二值化
    //轮廓检测
    vector<vector<Point>> contours; //轮廓
    vector<Vec4i> hierarchy;    //存放轮廓结构变量
    findContours(binary,contours,hierarchy,RETR_TREE,CHAIN_APPROX_SIMPLE,Point());
    //输出轮廓面积
    for (int i = 0; i < contours.size(); i++)
    {
        double area1 = contourArea(contours[i]);
        cout << "第" << i << "轮廓面积" << area1 << endl;
    }
    drawContours(img,contours,6,Scalar(0,0,255),2,8);
    imshow("draw",img);
    waitKey(0);
    return 0;
}
```

#### 2.3轮廓周长arcLength()

轮廓的周长也是轮廓的重要统计特性之一，虽然轮廓的周长无法直接反映轮廓区域的大小和形状

但是可以与轮廓面积结合得到关于轮廓区域的更多信息

例如，当某个区域的面积与周长的平方数的比值为1：16时，该区域为正方形

OpenCV4提供了用于轮廓周长或者曲线长度的arcLength()函数

**arcLength()函数原型：**

```cpp
double arcLength(InputArray curve,
                 bool closed
                 )
```

* curve：轮廓或者曲线的二维像素点
* closed：轮廓或者曲线是否闭合的标志，true表示闭合

该函数能够统计轮廓周长或者曲线的长度，返回值为统计长度，单位为像素，数据类型为double

第一个参数是轮廓或者曲线的二维像素点，数据类型为vector< Point >或者Mat

第二个参数是轮廓或者曲线是否闭合的标志，true表示闭合

该函数统计的长度是轮廓或者曲线相邻两个像素点之间连线的距离，例如，在计算三角形3个顶点A、B和C构成的轮廓长度时

若该函数的第二个参数为true，那么统计的长度是三角形3边之和，即AB+BC+CA

若参数为false，那么统计的长度是由A到C这3个点之间依次连线的距离之和，即AB和BC的长度之和

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
#include <vector>
using namespace std;
using namespace cv;
int main()
{
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_connect.png");
    if (img.empty())
    {
        cout << "error" << endl;
        return -1;
    }
    imshow("原图",img);
    Mat gray,binary;
    cvtColor(img,gray,COLOR_BGR2GRAY);  //转化成灰度图
    GaussianBlur(gray,gray,Size(9,9),2,2);    //平滑滤波
    threshold(gray,binary,170,255,THRESH_BINARY | THRESH_OTSU); //自适应二值化
    //轮廓检测
    vector<vector<Point>> contours; //轮廓
    vector<Vec4i> hierarchy;    //存放轮廓结构变量
    findContours(binary,contours,hierarchy,RETR_TREE,CHAIN_APPROX_SIMPLE,Point());
    //输出轮廓面积
    for (int i = 0; i < contours.size(); i++)
    {
        double length = arcLength(contours[i], true);
        cout << "第" << i << "轮廓面积" << length << endl;
    }
    drawContours(img,contours,6,Scalar(0,0,255),2,8);
    imshow("draw",img);
    waitKey(0);
    return 0;
}
```

#### 2.4轮廓外接多边形

由于噪声和光照的影响，物体的轮廓会出现不规则的形状，不规则的轮廓形状不利于对图像内容进行分析

此时需要将物体的轮廓拟合成规则的几何形状，根据需求可以将图像轮廓拟合成矩形、多边形等

矩形是常见的几何形状，矩形的处理和分析方法也比较简单，OpenCV4提供了两个函数用于求取外接矩形

分别是求取轮廓最大外接矩形的boundingRect()函数和求取轮廓最小外接矩形的minAreaRect()函数

寻找轮廓外接最大矩形就是寻找轮廓X方向和Y方向两端的像素，该矩形的长与宽分别与图像的两条轴平行

##### 2.4.1 boundingRect()

**boundingRect()函数原型：**

```cpp
Rect boundingRect(InputArray array)
```

* array：表示输入的灰度图像或者二维点集，数据类型为vector< Point >或者Mat

该函数可以求取包含输入图像中物体轮廓或者二维点集的最大外接矩形，只有一个参数

该函数的返回值是一个Rect类型的变量，该变量可以直接用rectangle()函数绘制矩形

返回值共有4个参数，前两个参数是最大外接矩形左上角第一个像素的坐标，后两个参数表示最大外接矩形的宽与高

最小外接矩形的4条边都与轮廓相交，该矩形的旋转角度与轮廓的形状有关，多数情况下矩形的4条边不与图像的两条轴平行

minAreaRect()函数可以求取轮廓的最小外接矩形

##### 2.4.2 minAreaRect()

**minAreaRect()函数原型：**

```cpp
RotatedRect minAreaRect(InputArray points)
```

* points：表示输入的二维点集合

该函数可以根据输入的二维点集合计算最小外接矩形

返回值是RotatedRect类型的变量，含有矩形的中心位置，矩形的宽和高，以及矩形旋转的角度

RotatedRect类具有两个重要的方法和属性，可以输出矩形的4个顶点和中心坐标

输出4个顶点坐标的方法是points()

假设RotatedRect类的变量为rrect，可以通过rrect.points(points)命令进行读取，坐标存放的变量是Point2f类型的数组

输出矩形中心坐标的属性是center

角度angle，rrect.angle获得角度

假设RotatedRect类的变量为rrect，可以通过opt = rrect.center命令，其中坐标存放的变量是Point2f类型

示例程序：首先利用Canny算法提取图像边缘，之后通过膨胀算法将邻近的边缘连接成一个连通域

​				   并提取每一个轮廓的最大外接矩形和最小外接矩形，最后在图像中绘制出矩形轮廓

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
#include <vector>
using namespace std;
using namespace cv;
int main()
{
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_Rect.png");
    if (img.empty())
    {
        cout << "error" << endl;
        return -1;
    }
    Mat img1,img2;
    img.copyTo(img1);   //深拷贝用来绘制最大外接矩形
    img.copyTo(img2);   //深拷贝用来绘制最小外接矩形
    imshow("原图",img);
    //去噪声和二值化
    Mat canny;
    Canny(img,canny,80,160,3,false);
    imshow("",canny);
    //膨胀运算，将细小缝隙填补
    Mat kernel = getStructuringElement(0,Size(3,3));
    dilate(canny,canny,kernel);
    imshow("膨胀之后",canny);
    //轮廓发现与绘制
    vector<vector<Point>> contours; //轮廓
    vector<Vec4i> hierarchy;    //存放轮廓结构变量
    findContours(canny,contours,hierarchy,0,2,Point());
    //寻找轮廓的外接矩形
    for (int i = 0; i < contours.size(); i++)
    {
        //最大外接矩形
        Rect rect = boundingRect(contours[i]);
        rectangle(img1,rect,Scalar(0,0,255),2,8,0);
        //最小外接矩形
        RotatedRect rrect = minAreaRect(contours[i]);
        Point2f points[4];
        rrect.points(points);   //读取最小外接矩形的4个顶点
        Point2f cpt = rrect.center; //最小外接矩形的中心
        //绘制旋转矩形与中心位置
        for (int j = 0; j < 4; j++)
        {
            if(j == 3)
            {
                line(img2,points[j],points[0], Scalar(0,255,0),2,8,0);
                break;
            }
            line(img2,points[j],points[j + 1], Scalar(0,255,0),2,8,0);
            cout << points[j] << endl;
        }
        //绘制矩形的中心
        circle(img,cpt,2,Scalar(255,0,0),2,8,0);
    }
    //输出绘制的外接矩形的结果
    imshow("max",img1);
    imshow("min",img2);
    waitKey(0);
    return 0;
}
```



##### 2.4.3 approxPolyDDP()

有时用矩形逼近轮廓会造成较大的误差

如果寻找逼近轮廓的多边形，那么多边形围成的面积会更加接近真实的圆形轮廓面积

OpenCV4提供了approxPolyDDP()函数用于寻找逼近轮廓的多边形

**approxPolyDDP()函数原型：**

```cpp
void approxPolyDDP(InputArray curve,
                   OutputArray approxCurve,
                   double epsilon,
                   bool closed
                   )
```

* curve：输入轮廓像素点
* approxCurve：多边形逼近结果，以多边形顶点坐标的形式给出
* epsilon：逼近的精度，即原始曲线和逼近曲线的最大距离
* closed：逼近曲线是否为封闭曲线的标志，true表示曲线封闭，即最后一个顶点与第一个顶点相连

该函数根据输入的轮廓得到最佳的逼近多边形

第一个参数是输入的轮廓二维像素点，数据类型是vector< Point >或者Mat

第二个参数是多边形的逼近结果，以多边形顶点坐标的形式输出，是CV_32SC2类型的Nx1的Mat矩阵

可以通过输出结果的顶点数目初步判断轮廓的集合形状

第三个参数是多边形逼近时的精度，即原始曲线和逼近曲线之间的最大距离

第四个参数是逼近曲线是否为封闭曲线的标志，其中true表示曲线封闭，即最后一个顶点与第一个顶点相连

示例程序：首先提取了图像的边缘，然后对边缘进行膨胀运算，将靠近的边缘变成一个连通域，之后对边缘结果进行轮廓检测

​				   并对每个轮廓进行多边形逼近，将逼近结果绘制在原图像中，并通过判断多边形顶点数目识别轮廓形状

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
#include <vector>
using namespace std;
using namespace cv;
//绘制轮廓函数
void drawapp(Mat result,Mat img2)
{
    for (int i = 0; i < result.rows; i++) \
    {
        //最后一个坐标点与第一个坐标点相连
        if (i == result.rows -1)
        {
            Vec2i point1 = result.at<Vec2i>(i);
            Vec2i point2 = result.at<Vec2i>(0);
            line(img2,point1,point2,Scalar(0,0,255),2,8,0);
            break;
        }
        Vec2i point1 = result.at<Vec2i>(i);
        Vec2i point2 = result.at<Vec2i>(i + 1);
        line(img2,point1,point2,Scalar(0,0,255),2,8,0);
    }
}
int main()
{
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_Rect.png");
    if (img.empty())
    {
        cout << "error" << endl;
        return -1;
    }
    Mat canny;
    imshow("img",img);
    Canny(img,canny,80,160,3,false);
    //膨胀运算
    Mat kernel = getStructuringElement(0,Size(3,3));
    dilate(canny,canny,kernel);
    //轮廓发现与绘制
    vector<vector<Point>> contours; //轮廓
    vector<Vec4i> hierarchy;    //存放轮廓结构变量
    findContours(canny,contours,hierarchy,0,2,Point());
    //绘制多边形
    for (int i = 0; i < contours.size(); ++i)
    {
        //用最小外接矩形求取轮廓中心
        RotatedRect rrect = minAreaRect(contours[i]);
        Point2f center = rrect.center; //最小外接矩形的中心
        circle(img,center,2,Scalar(255,0,0),2,8,0);
        Mat result;
        approxPolyDP(contours[i],result,4, true);   //多边形逼近
        drawapp(result,img);
        cout << "corners：" << result.rows << endl;
    }
    imshow("result",img);
    waitKey(0);
    return 0;
}
```



#### 2.5轮廓距离pointPolygonTest()

点到轮廓的距离，对于计算轮廓的图像的位置、两个轮廓之间的距离以及确定图像某一点是否在轮廓内部具有重要的作用

OpenCV4提供了计算像素点距离轮廓最小距离的pointPolygonTest()函数

**pointPolygonTest()函数原型：**

```cpp
double pointPolygonTest(InputArray contour,
                        Point2f pt,
                        bool measureDist
                        )
```

* contour：输入的轮廓
* pt：需要计算与轮廓距离的像素点
* measureDist：计算的距离是否具有方向性的标志，当参数为true时，点在轮廓内部时，距离为正，点在轮廓外部时，距离为负

​																							    当参数为false时，只检测是否在轮廓内

该函数能够计算指定像素点距离轮廓的最小距离并以double类型的数据返回

第一个参数是表示轮廓，数据类型是vector< Point >或者Mat

第二个参数是需要计算与轮廓距离的像素点坐标

第三个参数是计算的距离是否具有方向性的标志，当参数为true时，点在轮廓内部时，距离为正，点在轮廓外部时，距离为负

 当参数为false时，只检测是否在轮廓内

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
#include <vector>
using namespace std;
using namespace cv;
int main()
{
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_Rect.png");
    if (img.empty())
    {
        cout << "error" << endl;
        return -1;
    }
    Mat canny;
    Canny(img,canny,80,160,3,false);
    //膨胀运算
    Mat kernel = getStructuringElement(0,Size(3,3));
    dilate(canny,canny,kernel);
    //轮廓发现
    vector<vector<Point>> contours; //轮廓
    vector<Vec4i> hierarchy;    //存放轮廓结构变量
    findContours(canny,contours,hierarchy,0,2,Point());
    //创建图像中的一个像素点并绘制圆形
    Point point = Point(250,200);
    circle(img,point,2,Scalar(0,255,0),2,8,0);
    //多边形
    for (int i = 0; i < contours.size(); ++i)
    {
        //用最小外接矩形求取轮廓中心
        RotatedRect rrect = minAreaRect(contours[i]);
        Point2f center = rrect.center;          //最小外接矩形的中心
        circle(img,center,2,Scalar(255,0,0),2,8,0);     //创建圆心点
        //轮廓外部点距离轮廓的距离
        double dis = pointPolygonTest(contours[i],point, true);
        //轮廓内部点距离轮廓的距离
        double dis2 = pointPolygonTest(contours[i],center, true);
        //输出结果
        cout << "外部点距离轮廓距离" << dis << endl;
        cout << "内部点距离轮廓距离" << dis2 << endl;
    }
    return 0;
}
```

#### 2.6凸包检测convexHull()

有时物体的形状太过复杂，用多边形逼近处理起来依然较为复杂，例如人手、海星等

对于形状较为复杂的物体，可以利用凸包近似表示，凸包是图形学中常见的概念

将二维平面上的点集最外层的点连接起来构成的凸包多边形称为凸包

OpenCV4提供了用于物体检测的convexHull()函数

convexHull()函数原型：

```cpp
void convexHull(InputArray points,
                OutputArray hull,
                bool clockwise = false,
                bool returnPoints = true
                )
```

* points：输入的二维点集或轮廓坐标
* hull：输出凸包的顶点
* clockwise：方向标志，当参数为true时，凸包顺序为顺时针方向；当参数为false时，凸包顺序为逆时针方向
* returnPoints：输出数据的类型标志，当参数为true时，第二个参数输出结果是凸包顶点的坐标

​																		当参数为false时，第二个参数输出结果是凸包顶点的索引

该函数用于寻找二维点集或者轮廓的凸包

第一个参数是输入的二维点集或者轮廓坐标，数据类型是vector< Point >或者Mat

第二个参数是凸包顶点的坐标或者索引，数据类型是vector< Point >或者vector< int >

第三个参数是凸包方向的标志，当参数为true时，凸包顺序为顺时针方向；当参数为false时，凸包顺序为逆时针方向

最后一个参数是输出数据的类型标志，当参数为true时，第二个参数输出结果是凸包顶点的坐标，数据类型是vector< Point >或者Mat

当参数为false时，第二个参数输出结果是凸包顶点的索引，数据类型是vector< int >

示例程序：首先对图像二值化，并利用开运算消除二值化过程中产生的较小区域，之后寻找图像的轮廓，最后对图像中每个轮廓进行

凸包检测，并绘制凸包的顶点和每一条边

示例：

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
#include <vector>
using namespace std;
using namespace cv;
int main()
{
    Mat img = imread("E:\\CLion\\opencv_xin\\SG\\SG_Approx.png");
    if (img.empty())
    {
        cout << "error" << endl;
        return -1;
    }
    imshow("img",img);
    Mat gray,binary;
    cvtColor(img,gray,COLOR_BGR2GRAY);  //转化成灰度图
    GaussianBlur(gray,gray,Size(9,9),2,2);    //平滑滤波
    threshold(gray,binary,50,255,THRESH_BINARY);
    //开运算消除细小区域
    Mat k = getStructuringElement(MORPH_RECT,Size(3,3),Point(-1,-1));
    morphologyEx(binary,binary,MORPH_OPEN,k);
    imshow("binary",binary);
    //轮廓发现
    vector<vector<Point>> contours; //轮廓
    vector<Vec4i> hierarchy;    //存放轮廓结构变量
    findContours(binary,contours,hierarchy,0,2,Point());
    for (int i = 0; i < contours.size(); ++i)
    {
        //计算凸包
        vector<Point> hull;
        convexHull(contours[i],hull);
        //绘制凸包
        for (int j = 0; j < hull.size(); ++j)
        {
            //绘制凸包顶点
            circle(img,hull[j],4,Scalar(255,0,0),2,8,0);
            //连接凸包
            if (j == hull.size() - 1)
            {
                line(img,hull[j],hull[0],Scalar(0,0,255),2,8,0);
                break;
            }
            line(img,hull[j],hull[j+1],Scalar(0,0,255),2,8,0);
        }
    }
    imshow("hull",img);
    waitKey(0);
    return 0;
}
```



## 立体视觉

对图像的处理及从图像中提取信息最终目的都是为了得到环境信息，而图像采集是从环境信息到图像信息的映射

因此图像采集是视觉系统中首要环节，图像采集原理更是根据图像中信息推测环境信息的重要依据

### 1.单目视觉











































































# Eigen库

将Eigen文件放在工程中

include_directories(Eigen) //设置头文件

头文件：#include <Eigen/Dense>

```c++
一、矩阵的定义
Matrix<double, 3, 3> A;                // A.定义3x3double 矩阵
Matrix<double, 3, Dynamic> A;          //定义3xn double 矩阵,列为动态变化
Matrix<double, Dynamic, Dynamic>  A;   // 定义 double 矩阵,行、列为动态变化，由需要决定
MatrixXd A;                            // 定义 double 矩阵,行、列为动态变化，由需要决定
Matrix<double, 3, 3, RowMajor>  A;     // 定义3x3 double 矩阵,按行储存，默认按列储存效率较高。
Matrix3f  A;                           // 定义3x3 float 矩阵A.
Vector3f  A;                           // 定义3x1 float 列向量A.
VectorXd  A;                           // 定义动态double列向量A
RowVector3f  A;                        // 定义1x3 float 行向量A.
RowVectorXd  A;                        // 定义动态double行向量A.

二、基础用法
// Eigen        // Matlab           // comments
A.size()        // length(x)        // 元素个数
C.rows()        // size(C,1)        // 行个数
C.cols()        // size(C,2)        // 列个数

A(i)            // x(i+1)           // 默认情况下列优先，访问(0,i)的元素
C(i, j)         // C(i+1,j+1)       //访问（i, j）的元素。
A.resize(4, 4);       // 运行时，如果之前已经定义过形状则会报错。
B.resize(4, 9);       // 运行时，如果之前已经定义过形状则会报错。
A.resize(3, 3);       // Ok; size didn't change.
B.resize(3, 9);       // Ok; only dynamic cols changed.
 
A << 1, 2, 3,     // Initialize A. The elements can also be
     4, 5, 6,     // matrices, which are stacked along cols
     7, 8, 9;     // and then the rows are stacked.
B << A, A, A;     // B is three horizontally stacked A's.
A.fill(10);       // Fill A with all 10's.

三、特殊矩阵定义
// Eigen                                                
//单位矩阵定义
MatrixXd::Identity(rows,cols)       
C.setIdentity(rows,cols)            
//零矩阵定义
MatrixXd::Zero(rows,cols)          
C.setZero(rows,cols)              
//全1矩阵定义
MatrixXd::Ones(rows,cols)         
C.setOnes(rows,cols)               
//随即矩阵定义
MatrixXd::Random(rows,cols)            
C.setRandom(rows,cols)          
//线性阵定义
VectorXd::LinSpaced(size,low,high)  
v.setLinSpaced(size,low,high)     

四、矩阵分块
// 下面x为列或行向量，P为矩阵
**************************只能对向量操作************************************
x.head(n)                          // 列向量的前n个元素
x.head<n>()                        // 行向量的前n个元素
x.tail(n)                          // 列向量的倒数n个元素
x.tail<n>()                        // 行向量的倒数n个元素
x.segment(i, n)                    // 行向量从i开始的n个元素
x.segment<n>(i)                    // 列向量从i开始的n个元素
**************************只能对矩阵操作******************************************
P.block(i, j, rows, cols)          // 从i行j列开始的rows行cols列块。
P.block<rows, cols>(i, j)          //  从i行j列开始的rows行cols列块
P.row(i)                           // 矩阵P的第i行元素
P.col(j)                           // 矩阵P的第j列元素
P.leftCols<cols>()                 // P矩阵左边cols列元素
P.leftCols(cols)                   //P矩阵左边cols列元素
P.middleCols<cols>(j)              // P矩阵第j列开始的cols列元素
P.middleCols(j, cols)              // P矩阵第j列开始的cols列元素
P.rightCols<cols>()                // P矩阵右边cols列元素
P.rightCols(cols)                  // P矩阵右边cols列元素
P.topRows<rows>()                  // P矩阵前rows行元素
P.topRows(rows)                    // P矩阵前rows行元素
P.middleRows<rows>(i)              // P矩阵第i行开始的row行元素
P.middleRows(i, rows)              // P矩阵第i行开始的row行元素
P.bottomRows<rows>()               // P矩阵倒数row行
P.bottomRows(rows)                 // P矩阵倒数row行
P.topLeftCorner(rows, cols)        // P矩阵左上角rows行，cols列元素
P.topRightCorner(rows, cols)       // P矩阵右上角rows行，cols列元素
P.bottomLeftCorner(rows, cols)     
P.bottomRightCorner(rows, cols)    
P.topLeftCorner<rows,cols>()       
P.topRightCorner<rows,cols>()      
P.bottomLeftCorner<rows,cols>()    
P.bottomRightCorner<rows,cols>()   

五、矩阵元素交换                      
R.row(i) = P.col(j);    //可以将P的列元素去替换R的行元素
R.col(j1).swap(P.col(j2));  //将P的列元素和R的列元素进行互换


六、矩阵转置                       
R.adjoint()                        // R矩阵的伴随矩阵
R.transpose()                      // R矩阵的转置
R.diagonal()                       // R矩阵的迹，用列表示
x.asDiagonal()                     // 对角矩阵
R.reverse()                        // R矩阵逆时针旋转180度(反转)
R.colwise().reverse(); 			// R矩阵的列反转
R.rowwise().reverse(); 			// R矩阵的行反转
R.transpose().colwise().reverse(); // R矩阵逆时针旋转90度
R.transpose().rowwise().reverse(); // R矩阵顺时针旋转90度
R.conjugate()                      // conj(R)共轭矩阵


七、矩阵乘积
// Matrix-vector.  Matrix-matrix.   Matrix-scalar.
y  = M*x;          R  = P*Q;        R  = P*s;
a  = b*M;          R  = P - Q;      R  = s*P;
a *= M;            R  = P + Q;      R  = P/s;
                   R *= Q;          R  = s*P;
                   R += Q;          R *= s;
                   R -= Q;          R /= s;

八、矩阵内部元素操作
// Vectorized operations on each element independently
// Eigen                  // Matlab
R = P.cwiseProduct(Q);    // R = P .* Q对应点乘
R = P.array() * s.array();// R = P .* s对应点乘
R = P.cwiseQuotient(Q);   // R = P ./ Q对应点除
R = P.array() / Q.array();// R = P ./ Q对应点除
R = P.array() + s.array();// R = P + s  对应点加
R = P.array() - s.array();// R = P – s  对应点减
R.array() += s;           // R = R + s
R.array() -= s;           // R = R - s
R.array() < Q.array();    // R < Q  //Q矩阵元素比较，会在相应位置置0或1
R.array() <= Q.array();   // R <= Q //Q矩阵元素比较，会在相应位置置0或1
R.cwiseInverse();         // 1 ./ P   //1点除以P
R.array().inverse();      // 1 ./ P   //1点除以P
R.array().sin()           // sin(P)
R.array().cos()           // cos(P)
R.array().pow(s)          // P .^ s
R.array().square()        // P .^ 2
R.array().cube()          // P .^ 3
R.cwiseSqrt()             // sqrt(P)
R.array().sqrt()          // sqrt(P)
R.array().exp()           // exp(P)
R.array().log()           // log(P)
R.cwiseMax(P)             // max(R, P) //max(R, P) 对应取大
R.array().max(P.array())  // max(R, P) //min(R, P) 对应取小
R.cwiseMin(P)             // min(R, P)
R.array().min(P.array())  // min(R, P)
R.cwiseAbs()              // abs(P)
R.array().abs()           // abs(P)
R.cwiseAbs2()             // abs(P.^2)
R.array().abs2()          // abs(P.^2)
(R.array() < s).select(P,Q);  // (R < s ? P : Q)
附：
要取得[a,b)的随机整数，使用(rand() % (b-a))+ a;

要取得[a,b]的随机整数，使用(rand() % (b-a+1))+ a;

要取得(a,b]的随机整数，使用(rand() % (b-a))+ a + 1;

通用公式:a + rand() % n；其中的a是起始值，n是整数的范围。

要取得a到b之间的随机整数，另一种表示：a + (int)b * rand() / (RAND_MAX + 1)。

要取得0～1之间的浮点数，可以使用rand() / double(RAND_MAX)。
```

















# 卡尔曼滤波



OpenCV中有两个版本的卡尔曼滤波方法KalmanFilter(C++)和CvKalman(C)，用法差不太多



​    CV_WRAP const Mat& predict(const Mat& control=Mat());           //计算预测的状态值    

​    CV_WRAP const Mat& correct(const Mat& measurement);             //根据测量值更新状态值



step1：定义KalmanFilter类并初始化

  //构造KF对象

  KalmanFilter KF(DP, MP, 0);

  //初始化相关参数

  KF.transitionMatrix             转移矩阵 A

  KF.measurementMatrix          测量矩阵  H

  KF.processNoiseCov           过程噪声 Q

  KF.measurementNoiseCov       测量噪声    R

  KF.errorCovPost               最小均方误差 P

  KF.statePost                 系统初始状态 x(0) 

  Mat measurement              定义初始测量值 z(0) 

step2：预测

  KF.predict( )                         //返回的是下一时刻的状态值KF.statePost (k+1) 

step3：更新

  更新measurement;                   //注意measurement不能通过观测方程进行计算得到，要自己定义！

  更新KF  KF.correct(measurement)

最终的结果应该是更新后的statePost.



X(k)：k时刻系统状态
A：状态转移矩阵，对应opencv里kalman滤波器的transitionMatrix矩阵
B：控制输入矩阵，对应opencv里kalman滤波器的controlMatrix矩阵
U(k)：k时刻对系统的控制量
Z(k)：k时刻的测量值
H：系统测量矩阵，对应opencv里kalman滤波器的measurementMatrix矩阵
Q：过程噪声协方差矩阵，对应opencv里的kalman滤波器的processNoiseCov矩阵
R：观测噪声协方差矩阵，对应opencv里的kalman滤波器的measurementNoiseCov矩阵
P: 状态估计协方差矩阵

示例：

```python
    def __init__(self, id, frame, track_window):
        # 构建感兴趣区域ROI
        self.id = int(id)
        x, y, w, h = track_window
        self.track_window = track_window

        # 转换成HSV颜色空间，可以更好地专注于颜色
        self.roi = cv2.cvtColor(frame[y:y + h, x:x + w], cv2.COLOR_BGR2HSV)

        # 图像彩色直方图 16列直方图，每列直方图以0为左边界，18为右边界
        roi_hist = cv2.calcHist([self.roi], [0], None, [16], [0, 180])  
        # 直方图归一化到0~255范围内
        self.roi_hist = cv2.normalize(roi_hist, roi_hist, 0, 255, cv2.NORM_MINMAX) 

        # 构建卡尔曼滤波器
        self.kalman = cv2.KalmanFilter(4, 2)
        self.kalman.measurementMatrix = np.array([[1, 0, 0, 0], [0, 1, 0, 0]], np.float32)
        self.kalman.transitionMatrix = np.array([[1, 0, 1, 0], [0, 1, 0, 1], [0, 0, 1, 0], [0, 0, 0, 1]], np.float32)
        self.kalman.processNoiseCov = np.array([[1, 0, 0, 0], [0, 1, 0, 0], [0, 0, 1, 0], [0, 0, 0, 1]],
                                               np.float32) * 0.03
        self.measurement = np.array((2, 1), np.float32)
        self.prediction = np.zeros((2, 1), np.float32)
        self.term_crit = (cv2.TERM_CRITERIA_EPS | cv2.TERM_CRITERIA_COUNT, 10, 1)
        self.center = None
        self.update(frame)

    # 更新 行人行踪 的方法
    def update(self, frame):
        hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)
        back_project = cv2.calcBackProject([hsv], [0], self.roi_hist, [0, 180], 1)

        # 使用CamShift跟踪行人的行踪
        ret, self.track_window = cv2.CamShift(back_project, self.track_window, self.term_crit)
        pts = cv2.boxPoints(ret)
        pts = np.int0(pts)
        self.center = center(pts)
        cv2.polylines(frame, [pts], True, 255, 1)

        # 根据行人的的实际位置 矫正卡尔曼滤波器
        self.kalman.correct(self.center)
        prediction = self.kalman.predict()
        cv2.circle(frame, (int(prediction[0]), int(prediction[1])), 4, (255, 0, 0), -1)
```

















































































