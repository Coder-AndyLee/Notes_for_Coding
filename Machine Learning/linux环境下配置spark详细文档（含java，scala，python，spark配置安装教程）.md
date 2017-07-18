# 安装Java
1. 下载Java JDK
- [Java SE 8官方网址](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
- 根据自己的系统版本来选择是要使用32位版还是64位版。Linux提供了两种安装方式一个是.rpm，另一个是.tar.gz，我所使用的是.tar.gz。在下载前不要忘了选择Accept License Agreement。
2. 查看linux系统版本，32位 or 64位
3. 对下载的安装包进行解压（压缩包名称为 jdk-8u131-linux-x64.tar.gz）  
  tar -zxvf jdk-8u131-linux-x64.tar.gz
4. 创建系统路径并将解压缩出的文件移动过去  
  mkdir /usr/java  
  mv  ./jdk1.8.0_131 /usr/java
5. 配置环境变量
- 修改/etc/profile文件  
  sudo vim /etc/profile
- 在文件最后添加下面的内容  
#set java  
export JAVA_HOME=/usr/java/jdk1.8.0_131  
export JRE_HOME=/usr/java/jdk1.8.0_131/jre  
export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH  
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
- 其中，java文件的具体文件名，需按自己下的安装包名字做更改
6. 重启计算机并输出java检测是否配置成功
- reboot
- java

# 安装scala
1. 下载scala安装包 [官方下载链接](http://www.scala-lang.org/download/)
2. 对下载的安装包进行解压（压缩包名称为scala-2.11.4.tgz）  
  tar -zxvf scala-2.11.4.tgz
3. 创建系统路径并将解压缩出的文件移动过去  
  mkdir /usr/scala  
  mv  ./scala-2.11.4 /usr/scala
4. 配置环境变量
- 修改/etc/profile文件  
  sudo vim /etc/profile
- 在文件最后添加下面的内容  
#set scala  
export SCALA_HOME=/usr/scala/scala-2.11.4  
export PATH=$PATH:$SCALA_HOME/bin  
- 其中，scala文件的具体文件名，需按自己下的安装包名字做更改
5. 重启计算机并输出java检测是否配置成功
- reboot
- scala

# 安装Python
http://www.cnblogs.com/ich1990/p/3779608.html
1. 准备编译环境gcc（我的本机Ubuntu是包含gcc的）
2. 去官网下载要安装的对应版本的python的源代码
- 推荐下载Python2.7，Python3可能会出问题
- 下载地址：https://www.python.org/downloads/source/  
- 下载的文件为Python-2.7.13.tgz
3. 解压下载的代码包  
- tar -zxvf Python-2.7.13.tgz  
- cd Python-2.7.13  
4. 配置
- 查找configure文件  
  find . -name configure
- 运行配置文件  
  ./configure
5. 编译
- make
- make install
#### 此时在当前目录下即可输入python来进入python开发环境
6. 替换以前的python默认版本（创建新的软连接）
- cd /usr/bin/
- rm -rf python
- ln -s /usr/local/Python-x.x.x/bin/python ./python
- 这样，你直接输入python就是你最新安装的python新版本啦，要想用以前的版本，可以输入pythonx.x来启动

# 安装spark
1. 下载spark-1.3.1-bin-hadoop2.6.tgz，解压到/usr/spark
- 下载地址：http://spark.apache.org/downloads.html
- 具体操作方法同安装java和scala
2. 配置环境变量
- 修改/etc/profile文件  
  sudo vim /etc/profile
- 在文件最后添加下面的内容
- #set spark  
  export SPARK_HOME=/usr/spark/spark-2.1.0-bin-hadoop2.7

- #set pySpark 将Spark中的pySpark模块增加的Python环境中  
  export PYTHONPATH=/usr/spark/spark-2.1.0-bin-hadoop2.7/python
- 其中，spark 文件的具体文件名，需按自己下的安装包名字做更改
- 重启电脑，使/etc/profile永久生效；若想临时生效，打开命令窗口，执行 source /etc/profile  在当前窗口生效
3. 测试安装结果
- 打开命令窗口，切换到Spark根目录（/usr/spark）
- 执行 ./bin/spark-shell,打开Scala到Spark的连接窗口  
  启动过程中无错误信息，出现scala>，则启动成功
- 执行./bin/pyspark ,打开Python到Spark的连接窗口  启动过程中无错误，在出现python运行环境时，启动成功。

http://www.open-open.com/lib/view/open1432192407317.html

# IDE安装
- IDEA安装  
  http://www.he11oworld.com/program/3013.html  
  linux下激活方式：blog.csdn.net/to_baidu/article/details/52979840
- Pycharm安装  
  http://blog.csdn.net/rebelqsp/article/details/21548969  
  激活码：http://blog.csdn.net/fx677588/article/details/58164902

# 可能遇到的问题
#### 1. 启动pyspark时，无法正常启动，并无法读取sc的值：   zipimport.ZipImportError: can't decompress data; zlib not available
- 类似问题解决方法：http://zhoujianghai.iteye.com/blog/1521993
- 1. 先安装系统相应的依赖库文件  
      sudo apt-get install zlibc zlib1g-dev
    2. 到python安装目录下执行  
      sudo ./configure  
    3. 编辑Modules/Setup文件  
      vim Modules/Setup  
      找到下面这句，去掉注释  
      \#zlib zlibmodule.c -I$(prefix)/include -L$(exec_prefix)/lib -lz
    4. 重新编译安装：  
      sudo make  
      sudo make -i install  
    5. 此时可正常启动pyspark