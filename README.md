# ES2016_14353012
## Description
### DOL框架描述：
>分布式操作层（DOL）是一个让程序可并行应用的软件开发框架。它允许指定的应用程序基于Kahn过程网络模型的计算和以基于SystemC的模拟引擎为特征。此外，DOL提供了一个基于xml规范的格式用以描述并行应用程序在一个多处理器系统上的包含绑定和映射的实现。
DOL由三个基本部分组成：
* DOL应用程序编程接口
* DOL功能仿真
* DOL映射优化


## How to install
## DOL 安装笔记：
1.安装一下必要的环境：  
$ sudo apt-get update   
$ sudo apt-get install ant   
$ sudo apt-get install openjdk-7-jdk   
$ sudo apt-get install unzip  

2.下载文件：  
sudo wget http://www.accellera.org/images/downloads/standards/systemc/systemc-2.3.1.tgz   
sudo wget http://www.tik.ee.ethz.ch/~shapes/downloads/dol_ethz.zip   
 
3.解压文件：
* 新建dol文件夹  
$	mkdir dol  
* 将dolethz.zip解压到dol文件夹中  
$	unzip dol_ethz.zip -d dol  
* 解压systemc  
$	tar -zxvf systemc-2.3.1.tgz  

4.编译systemc：  
* 解压后进入systemc-2.3.1的目录下  
$	cd systemc-2.3.1  
* 新建一个临时文件夹objdir  
$	mkdir objdir  
* 进入该文件夹objdir  
$	cd objdir  
* 运行configure  
$	../configure CXX=g++ --disable-async-updates  
* 下图为运行configure后的截图：     
![1.png](https://ooo.0o0.ooo/2016/10/08/57f8c74744c15.png)

5.编译  
$	sudo make install  
编译完后文件目录如下  
![2.png](https://ooo.0o0.ooo/2016/10/08/57f8c7fa1cd46.png)

* 记录当前的工作路径  
$	pwd  

6.编译dol：  
进入刚刚的dol文件夹，修改build.xml文件   
找到下面这段话，即上面编译的systemc位置在哪里  
<property name="systemc.inc" value="YYY/include"/>  
<property name="systemc.lib" value="YYY/lib-linux/libsystemc.a"/>  
把YYY改成上页pwd的结果  
Ps：对于 64位 系统的机器，lib-linux要改成lib-linux64  

7.编译  
$	ant -f build_zip.xml all  
若成功会显示build successful  

>我们来运行第一个例子  
$	cd dol/build/bin/main  
$	sudo ant -f runexample.xml -Dnumber=1  
![3.png](https://ooo.0o0.ooo/2016/10/08/57f8c85575c6e.png)


## Experimental experience：
>实验感受……师兄配好的虚拟机真好用……只要改改密码名字就好……所以过程中的截图都不是我的机子截的……   
据说按照这个过程是可以一路畅通，但是这种事情一般看脸，脸黑，一般不配个几次是不会成功的= =。
