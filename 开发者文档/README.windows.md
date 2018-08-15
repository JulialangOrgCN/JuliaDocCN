# 问都死上的猪吏

本文叙述如何在Windows上安装或构建以及使用Julia。

## Windows的常规信息

### Unicode字体支持

Windows内建的字体对Unicode字符空间的覆盖相当可怜。

【[DejaVu Sans Mono](https://dejavu-fonts.github.io/)】，这个自由字体可作为Windows控制台字体的替代物。

自Windows 2K以来，简单地下载和安装字体已经不够，因为Windows在注册表中维持一份经过核准地字体清单。

给Windows 2K及更新版本的终端添加字体的指令说明在[这里]((https://superuser.com/a/5079)。

    TODO: 翻译Windows添加字体方法。

此外，不甘依靠默认命令行提示，就需要采用别的终端模拟程序，例如[Conemu](https://code.google.com/p/conemu-maximus5/)/[Mintty](https://github.com/mintty/mintty)（注意：在Mintty运行Julia则stty.exe必须在PATH环境变量给出的路径下方可正常工作）。

还有[Juno](http://junolab.org)/[Sublime-IJulia](https://github.com/quinnj/Sublime-IJulia)/[IJulia](https://github.com/JuliaLang/IJulia.jl)等富功能IDE供选择。

### 行末

Julia使用单一的二进制模式文件。

与众多别的Windows程序不同，如果用户书写“\n”到文件，用户则只从文件读出“\n”而没有别的比特模式。这符合别的操作系统所展现的行为。如果用户安装了[Git for Windows](https://gitforwindows.org/)，这是推荐的、但非必需，就请将Git配置成相同的习惯。

    git config --global core.eol lf
    git config --global core.autocrlf input

或编辑“%USERPROFILE\.gitconfig%”如下：

    [core]
        eol = lf
        autocrlf = input

## 二进制分发

详查【[下载 - 朱华社区](http://julialang.org.cn/download.html)】。


## 源代码分发

### Windows支持的构建平台

- Windows 10: 32/64位均可。
- Windows 8: 32/64位均可。
- Windows 7: 32/64位均可。

### Cygwin/MinGW交叉编译

Windows上编译Julia推荐[Cygwin](http://www.cygwin.com)交叉编译，所用的MinGW-w64版本编译器在Cygwin的包管理器中可用。

- 一、下载

    获取Cygwin，32位或64位都行。
    注意，用32位或64位的Cygwin都能编译32位或64位的Julia，64位的Cygwin有略小但通常更新的包选项。

    高级——通过命令行运行可以跳过二到四步骤：

        setup-x86_64.exe -s &lt;url&gt; -q -P cmake,gcc-g++,git,make,patch,curl,m4,python,p7zip,mingw64-i686-gcc-g++,mingw64-i686-gcc-fortran,mingw64-x86_64-gcc-g++,mingw64-x86_64-gcc-fortran

        把url替换为Cygwin的[镜像](https://cygwin.com/mirrors.html)，或先手动执行setup选择一个镜像。

- 二、选择安装位置及下载镜像

- 三、选择需要的包

    在*Devel*分类选择：cmake/gcc-g++/git/make/patch

    在*Net*分类选择：curl

    在*Interpreters*或*Python*分类选择：m4/python

    在*Archive*分类选择：p7zip

    若要编译32位Julia，还需要从*Devel*分类中选择：mingw64-i686-gcc-g++/mingw64-i686-gcc-fortran

    若要编译64位Julia，还需要从*Devel*分类中选择：mingw64-x86_64-gcc-g++/mingw64-x86_64-gcc-fortran

- 四、确保“依赖解决”的“选择必选包（推荐）”启用

- 五、所有安装结束后，启动安装位置的快捷方式“Cygwin Terminal”或“Cygwin64 Terminal”

- 六、从源代码构建Julia及其依赖

    获取Julia源代码：

        git clone https://github.com/JuliaLang/julia.git
        cd julia

        提示：若碰到“error: cannot fork() for fetch-pack: Resource temporarily unavailable”，添加“alias git="evn PATH=/usr/bin git"”到“~/.bashrc”并重启Cygwin/Cygwin64。

    设置Make.user中的XC_HOST变量指明采用MinGW-w64交叉编译：

        echo 'XC_HOST = i686-w64-mingw32' > Make.user # 编译32位Julia
        echo 'XC_HOST = x86_64-w64-mingw32' > Make.user # 编译64位Julia

    启动构建：

        make -j 4 # 根据用户构建环境调整线程数

    > ```sh
    > make O=julia-win32 configure
    > make O=julia-win64 configure
    > echo 'XC_HOST = i686-w64-mingw32' > julia-win32/Make.user
    > echo 'XC_HOST = x86_64-w64-mingw32' > julia-win64/Make.user
    > echo 'ifeq ($(BUILDROOT),$(JULIAHOME))
    >         $(error "in-tree build disabled")
    >       endif' >> Make.user
    > make -C julia-win32  # build for Windows x86 in julia-win32 folder
    > make -C julia-win64  # build for Windows x86-64 in julia-win64 folder
    > ```

- 七、直接运行Julia可执行程序

    ```
    usr/bin/julia.exe
    usr/bin/julia-debug.exe
    ```

### MinGW/MSYS2编译

在[MSYS2]上编译Julia过去是有效的，但目前未被支持。
欢迎提交PR修复，查看[之前在MYSYS2上编译Julia的指令](https://github.com/JuliaLang/julia/blob/v0.6.0/README.windows.md)。

### 在Unix上交叉编译

可以在Linux/Mac/WSL（Windows Subsystem for Linux）上通过MinGW-w64交叉编译来构建一个Windows版本的Julia。

注意：在WSL中编译，需要用Linux文件系统环境，而不能是“/mnt/”模拟Windows路径，因为“/mnt/”下[时间戳不能正常工作](https://github.com/Microsoft/BashOnWindows/issues/1939)，却是配置脚本和.mk必须的。

为了做大成都兼容包，使用[WinRPM.jl](https://github.com/JuliaLang/WinRPM.jl)解决Windows上的二进制依赖，非常推荐用OpenSUSE 42.2来交叉编译一个Windows版本的Julia。

如果是别的Linux发行版或Max OS X，安装[Vagrant](http://www.vagrantup.com/downloads)并采用下述Vagrantfile。

    ```
    # Vagrantfile for MinGW-w64 cross-compilation of Julia

    $script = <<SCRIPT
    # Change the following to i686-w64-mingw32 for 32 bit Julia:
    export XC_HOST=x86_64-w64-mingw32
    # Change the following to 32 for 32 bit Julia:
    export BITS=64
    zypper addrepo http://download.opensuse.org/repositories/windows:mingw:win$BITS/openSUSE_Leap_42.2/windows:mingw:win$BITS.repo
    zypper --gpg-auto-import-keys refresh
    zypper -n install --no-recommends git make cmake tar wine which curl \
        python python-xml patch gcc-c++ m4 p7zip.i586 libxml2-tools winbind
    zypper -n install mingw$BITS-cross-gcc-c++ mingw$BITS-cross-gcc-fortran \
        mingw$BITS-libstdc++6 mingw$BITS-libgfortran3 mingw$BITS-libssp0
    # opensuse packages the mingw runtime dlls under sys-root/mingw/bin, not /usr/lib64/gcc
    cp /usr/$XC_HOST/sys-root/mingw/bin/*.dll /usr/lib*/gcc/$XC_HOST/*/
    git clone git://github.com/JuliaLang/julia.git julia
    cd julia
    make -j4 win-extras julia-ui-release
    export WINEDEBUG=-all # suppress wine fixme's
    # this last step may need to be run interactively
    make -j4 binary-dist
    SCRIPT

    Vagrant.configure("2") do |config|
    config.vm.box = "bento/opensuse-leap-42.2"
    config.vm.provider :virtualbox do |vb|
        # Use VBoxManage to customize the VM. For example to change memory:
        vb.memory = 2048
    end
    config.vm.provision :shell, :inline => $script
    end
    ```

# 不用Vagrant来交叉编译Julia

如果不关心WinRPM生态系统（偶发于OpenSUSE）潜在的不兼容，按如下步骤交叉编译Julia：

    首先，需要确保系统已安装必要的依赖：wine 1.7.5+，系统编译器，下载工具。

    在Ubuntu上（别的Linux系统，依赖名称很相似）：
        apt-get install wine subversion cvs gcc wget p7zip-full winbind mingw-w64
    在Mac上：安装XCode、XCode命令行工具，X11（现在是[XQuartz](http://xquartz.macosforge.org/)），[MacPorts](http://www.macports.org/install.php)或[HomeBrew](https://brew.sh/)，然后执行：
        port install wine wget mingw-w64
        或
        brew install wine wget mingw-w64

    然后，执行构建。
        ```
        git clone https://github.com/JuliaLang/julia.git julia-win32
        echo override XC_HOST = i686-w64-mingw32 >> Make.user
        make
        make win-extras (Necessary before running "make binary-dist")
        make binary-dist
        move the julia-*.exe installer to the target machine
        ```
        如果构建64位的Windows版本Julia，主要步骤一样，只需要把XC_HOST改为x86_64的。
        注意：Mac上wine只能运行在32位模式。

## 在wine上调试交叉编译构建（的Windows版本Julia）

在交叉编译主机上调试交叉编译版本的Juia的最有效方式是安装Windows版本的GDB并像在wine上一样运行GDB。

预构建包在[这儿](https://sourceforge.net/projects/msys2/files/REPOS/MINGW/)，是MSYS2项目的一部分，已知有效。

除了GDB包，还需要python和termcap包。

最后，命令行启动GDB可能不会工作，可以通过将wineconsole伪装成GDB调用来规避。

## 用Windows虚拟机

[Vagrant](http://www.vagrantup.com/downloads)也可以在Windows VM上通过【contrib/windows/Vagrantfile】来使用，只需要在该目录下执行“vagrant up”。

## 编译之后

通过以上某种方式构建基本的Julia，而某些扩展组件并未包含。

若需要这些组件，最简单的方式是在执行完“make binary-dist”之后接着执行“make win-extras”，然后运行产出的安装程序。

## Windows构建调试

### GDB配Cygwin Mintty

在Windows控制台（cmd）运行gdb，gdb可能在mintty上不能和非Cygwin应用
和谐工作。可以在mintty中用“cmd /c start”来启动Windows控制台（如需）。

### GDB没有附着在正确的进程

通过Windows任务管理器或ps命令的WINPID（在类Unix命令行如pgrep等命令应该是PID）找到正确的进程。

在Windows任务管理器上看不到PID列则需要添加显示该列。

### GDB没有显示正确的栈回溯

当附着在Julia进程后，GDB可能没有附着在正确的线程。

用“info thread”查看全部线程、用“thread thread-number”来切换线程。

确保32位的GDB调试32位的Julia、64位的GDB调试64位的Julia。

### 构建进程慢或吃内存搞挂机器

停用Windows [Superfetch](https://en.wikipedia.org/wiki/Windows_Vista_I/O_technologies#SuperFetch)和[Program Compatibility Assistant](http://blogs.msdn.com/b/cjacks/archive/2011/11/22/managing-the-windows-7-program-compatibility-assistant-pca.aspx)这两个服务，一致在MinGW/Cygwin存在[spurious interactions](https://cygwin.com/ml/cygwin/2011-12/msg00058.html)。

如上面链接提到的，svchost占用过多的内存可以通过任务管理器、选择占用内存高的svchost进程、转到服务来调查。

逐个禁止子服务，直到那个“罪人”被揪出来。

警惕[BLODA](https://cygwin.com/faq/faq.html#faq.using.bloda)！

分辨这种软件冲突，工具[vmmap]()不可或缺。

用vmmap检查bash/mintty、别的用来驱动构建的持久进程已经加载的DLLs。

实际上，任何Windows系统之外的DLL都有潜在的BLODA。

---
# 译后感

- 就是说还没有MS套件的编译方式。
