# 开发环境配置
- Download Visual Studio 2013 Community Edition (make sure to install MFC)
- Download Qt 5.6.0 (x32) for MSVC2013, install in C:\Qt\qt-5.6.0-x86-msvc2013.
- Download Qt 5.6.0 (x64) for MSVC2013, install in C:\Qt\qt-5.6.0-x64-msvc2013.
- Download Qt Creator 4.0.0, install in C:\Qt\qtcreator-4.0.0.
- [Get the Dependencies](Getting the Dependencies)



### 以上是github上的环境配置文档，具体配置步骤如下：  
1. 下载[Visual Studio 2013](https://www.visualstudio.com/en-us/news/releasenotes/vs2013-community-vs)（装有MFC版本的）
2. 下载Qt 5.6.0 for MSVC2013  
    - [Qt x86 MSVC2013 下载地址](http://download.qt.io/official_releases/qt/5.6/5.6.0/qt-opensource-windows-x86-msvc2013-5.6.0.exe)  安装路径为 C:\Qt\qt-5.6.0-x86-msvc2013  
    - [Qt x64 MSVC2013 下载地址](http://download.qt.io/official_releases/qt/5.6/5.6.0/qt-opensource-windows-x86-msvc2013_64-5.6.0.exe) 安装路径为 C:\Qt\qt-5.6.0-x64-msvc2013    
      （注：1. 必须遵守路径进行安装 2. Qt与VS的版本须相互对应）  
3. 下载[Qt Creator 4.0.0](http://download.qt.io/official_releases/qtcreator/4.0/4.0.0/qt-creator-opensource-windows-x86-4.0.0.exe), 安装路径为 C:\Qt\qtcreator-4.0.0
4. Get the Dependencies  
    - 这一步没有看懂，应该是编者利用markdown语法添加链接失败了。在操作过程省去这步对整个编译过程并没有产生影响。

# 源代码下载
- Clone the repository to your local drive. Make sure to include the submodules in your clone command!  
    - git clone --recurse-submodules https://github.com/x64dbg/x64dbg.git

### 以上是GitHub上的源码介绍，注意事项如下：
- 若在网页上点击Clone or download按钮下载源码，则会出现上述说的submodules不全的情况，部分目录下缺少文件导致编译失败。**解决方法为：** 在git bash中使用上述命令下载代码。

# 编译准备步骤
- 在源码根目录下，按顺序运行：
    - install.bat 
        - this ensures your code is auto-formatted to the x64dbg standards and initializes the repository
    - setenv.bat
    - build.bat
        - If you install Qt and/or Visual Studio in different paths, you can set (global) environment variables to make setenv.bat create a custom environment for compiling with build.bat. 
- 注意：本文中提到运行.bat文件，建议均使用cmd/PowerShell ，cd到.bat文件目录运行，这样可看到文件的输出结果；若直接双击运行文件，文件将在运行完毕后自动退出，从而无法看到输出日志。


# 编译过程
- [github文档教程](https://github.com/x64dbg/x64dbg/wiki/Compiling-the-whole-project)


- Open x64dbg.sln in Visual Studio 2013
- Compile the solution (F7)
- Open src\gui\x64dbg.pro in Qt Creator
- Compile the GUI.


- 首次运行QTCreator ，可能需要进行一些配置：
    - 打开QTCreator菜单，工具-选项，在对话框左边选择“构建和运行”（或在打开工程之后的初始窗口，点击++option++）
    - 编译Qt程序需要以下几个部件，构建套件Kit、Qt Versions、编译器、QDebuggers。下面依顺序讲解配置步骤：
        - QDebuggers
            - 点击Debugger标签页，点击右边的Add按钮，在下方的Path处点击浏览，找到gdb.exe的位置并添加
        - 编译器
            - 点击右边“添加”，弹出菜单有MinGW、GCC、Clang、Custom和QCC，选择MinGW，名称设为MinGW，对于编译器路径设置，就浏览找到g++.exe，设置好这两条就够了，然后点击右下角“Apply”（此处我未进行配置，直接使用的Auto-detected）
        - Qt Versions
            - 点击右边“添加”，弹出的文件查找框，选择qmake.exe。根据之前安装的路径，选中qmake.exe 
        - 配置Kit
            - 在窗口下方进行选择。编译器一行使用默认选好的值即可，Qt版本选择刚才添加的版本，Qt mkspec不用管。
    - 配置完成后，点击configure project即可。
- 进入Qt主界面后，在左下角选择release编译方式，点击下方的绿色箭头（或Ctrl+R)即可编译gui

# 发布
- 运行根目录下的release.bat文件，即可在根目录下生成/release文件夹，在\release\release路径下，找到x96dbg.exe并运行，按照提示即可完成安装。在桌面找到对应的程序快捷方式，即可运行程序。


# release版本直接下载
- [下载链接](https://github.com/x64dbg/x64dbg/releases/download/snapshot/snapshot_2017-04-12_15-29.zip)