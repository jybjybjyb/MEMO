
- [基础操作](#基础操作)
  - [**嵌入式 Python 包**](#嵌入式-python-包)
  - [**Pystand Python 加壳**](#pystand-python-加壳)
    - [**安装依赖**](#安装依赖)
    - [**裁剪依赖**](#裁剪依赖)
    - [**二进制压缩**](#二进制压缩)
    - [**代码组织**](#代码组织)
- [加密](#加密)
  - [**基础加密**](#基础加密)
  - [**高级加密**](#高级加密)
- [简单方法](#简单方法)


# 基础操作

## **嵌入式 Python 包**

当然是手工打包，现在 Python 3.5 以后，官方都会发布一个嵌入式 Python 包：

**华为镜像下载**


3.8 是最后一个支持 Win7 的版本，3.9 以后就不支持了。那么为什么选择 32 位？因为打包出来 32 位是最紧凑的，64 位会大很多，除非你要一次性在内存里 load 2GB 以上的数据，否则基本就选择 32 位的。

在项目路径里建立一个新的 `runtime` 文件夹，把这些文件放进去，外层写个批处理，调用一下里面的 python.exe 基本就可以跑程序了。当然这样看起来很原始，所以精细一点的话，为这个 embedded python 做一个壳，直接加载里面 python3.dll 或者 python38.dll 来运行程序。

## **Pystand Python 加壳**

上面说的加壳我写了个例子了，叫做 PyStand：

[https://github.com/skywind3000/PyStand](https://link.zhihu.com/?target=https%3A//github.com/skywind3000/PyStand)

到 [Release](https://link.zhihu.com/?target=https%3A//github.com/skywind3000/PyStand/releases) 下载下来是这样：

![](https://pic1.zhimg.com/50/v2-ec06e1b3809554e0d2b4590649d35b11_720w.jpg?source=1940ef5c)

选择第一个 [PyStand-py38-pyqt5-lite](https://www.zhihu.com/search?q=PyStand-py38-pyqt5-lite&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2336654649%7D) 这个包，下载下来 14MB，解压后：

![](https://picx.zhimg.com/50/v2-463f295b3f0c210cddd8b40f7d6958e4_720w.jpg?source=1940ef5c)

目录非常清爽，比 PyInstaller 非单文件那种上百个 dll 的目录干净多了，就几个文件：

-   runtime：之前官方包 embedded python 解压后的内容。
-   site-packages：第三方依赖
-   PyStand.exe：主程序入口。
-   [http://PyStand.int](https://link.zhihu.com/?target=http%3A//PyStand.int)：脚本入口。

这个 PyStand.exe 就是可以直接运行的程序，双击：

![](https://pic1.zhimg.com/50/v2-f2dd800f8d89e66d04317093306eb1bc_720w.jpg?source=1940ef5c)

运行成功，打包文件只有 14MB，就能跑一个完整 PyQt5 的项目了，比 PyInstaller 的 30MB 小不少，你就是解压开也才 40MB，比 70MB 的 PyInstaller 解压后大小精简很多。

主要代码就是写在 [PyStand.int](https://link.zhihu.com/?target=http%3A//pystand.int/) 里，这个 PyStand.exe 启动后会自动加载同名的 .int 文件：

```
import sys, os
from PyQt5.QtWidgets import *
from PyQt5.QtCore import *
app = QApplication([])
win = QWidget()
win.setWindowTitle('PyStand')
layout = QVBoxLayout()
label = QLabel('Hello, World !!')
label.setAlignment(Qt.AlignCenter)
layout.addWidget(label)
btn = QPushButton(text = 'PUSH ME')
layout.addWidget(btn)
win.setLayout(layout)  
win.resize(400, 300)
btn.clicked.connect(lambda : [
        print('exit'),
        sys.exit(0),
    ])
win.show()
app.exec_()
```

也就是说可执行叫做 PyStand.exe ，它会加载 [http://PyStand.int](https://link.zhihu.com/?target=http%3A//PyStand.int)；如果改名叫做 MyDemo.exe 它就会加载 [MyDemo.int](https://link.zhihu.com/?target=http%3A//mydemo.int/) 里的代码。

那么其实大家可以直接使用了，把程序名字改成你想要的编写对应的 .int 即可，换图标的话，可以自己重新编译 PyStand 项目，或者直接 Resource Hacker 更换图标：

![](https://pica.zhimg.com/50/v2-fe09b0558013896fe92f4b8e58e9b307_720w.jpg?source=1940ef5c)

根本不需要配置 C/C++ 编译环境。

### **安装依赖**

我们需要一个对应版本号的 32 位的完整 python 3.8，然后新建个干净的虚拟环境：

```
\path\to\py38\python.exe  venv test
```

生成 test 目录里，大概目录是这样：

![](https://pic1.zhimg.com/50/v2-91ffcb1e2521e2bb1613a4f0be2cd4f0_720w.jpg?source=1940ef5c)

用 cmd.exe 进入 Scripts 目录，运行 `activate` 后，用 pip 安装你需要的包，然后到上面虚拟环境的 `Lib/site-packages` 里，把你需要的包找出来：

![](https://pica.zhimg.com/50/v2-743e3f784552e4f6b239d8e65c1540c8_720w.jpg?source=1940ef5c)

拷贝到 PyStand.exe 所在目录的 site-packages 里面即可使用，注意多余的，没有依赖的东西无需拷贝，比如上图的 pip 包。

### **裁剪依赖**

现在你已经把依赖的包拷贝到 PyStand 的 site-packages 里了，比如 PyQt5 这个包：

![](https://picx.zhimg.com/50/v2-20a9a6b463cfb1bd97f27dbb0b7bc81a_720w.jpg?source=1940ef5c)

进去看一眼，把你不要的模块全部删除，什么 OpenGL，AxContainer，Multimedia，Position, Location, RemoteObject, DBus, QtQuick, QtWebEngine 之类的，不确定的可以删除了运行下你的程序测试下行不行，不行的又拷贝回来。

然后继续进入上图 Qt5 的目录里的 bin 目录：

![](https://pic1.zhimg.com/50/v2-8f44fcabd9c3d15799b4e66ebd609ca5_720w.jpg?source=1940ef5c)

这里有很多尺寸很大的模块，继续删除多余的，什么 QML，Test，Help，OpenGL ，GLES，Sensor, Bluetooth 之类的全部干掉，再到上层：

![](https://picx.zhimg.com/50/v2-5b86216119a4a5559b7c9035f872e17c_720w.jpg?source=1940ef5c)

接着精简 plugins 目录里不要的东西，删除 qml/qsci, 然后到 translations 里把不要的语言删除掉，这么一圈下来，整个目录从原来的 134MB：

![](https://picx.zhimg.com/50/v2-ce8b2cd79535359bf21dee51b09ece13_720w.jpg?source=1940ef5c)

精简到 46.8 MB：

![](https://pica.zhimg.com/50/v2-409fca46c4b62f5de7d7a48a5d691a13_720w.jpg?source=1940ef5c)

比你 PyInstaller 解压出来的 70MB 小了一大半，打包出来就是 14MB：

![](https://picx.zhimg.com/50/v2-8cb1436b9fb81cbf0adfd6233a86548e_720w.jpg?source=1940ef5c)

我已经帮你裁剪好了，你可以直接使用，还有好多可以根据你程序的需要进行裁剪，比如 [openssl](https://www.zhihu.com/search?q=openssl&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2336654649%7D)，sqlite 这些都非常大，总之还有很多压缩空间，按照你的程序需要还可以二次裁剪。

手工裁剪比无脑 PyInstaller 可靠的多，不但可以精细裁剪，每一步你都清晰的知道是怎么来的，出了问题你也知道该怎么回退。

### **二进制压缩**

还可以用 upx 压缩一些比较大的文件，但 runtime 下面的 python3.dll, python38.dll, [vcruntime140.dll](https://www.zhihu.com/search?q=vcruntime140.dll&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2336654649%7D) 不能压缩，而 PyQt5 里的 QtCore, QtWidgets, QtGUI 不能压缩，一边测试一边压缩，还可以进一步精简。

### **代码组织**

你有很多 py 代码，可以在 PyStand 下面新建一个 script 目录：

![](https://pic1.zhimg.com/50/v2-05ab8c60db2402d984da63ad9534efbf_720w.jpg?source=1940ef5c)

在里面放一个 main.py，实现一个 main 方法，然后改写 [http://PyStand.int](https://link.zhihu.com/?target=http%3A//PyStand.int)：

```
import sys, os
os.chdir(os.path.dirname(__file__))
sys.path.append(os.path.abspath('script'))
sys.path.append(os.path.abspath('script.egg'))
import main
main.main()
```

这个代码就是做了三件事情：矫正当前运行目录，设置 `sys.path`，然后导入 main 模块并执行 main 方法。注意后面 sys.path 里追加了一个 `script.egg`，意思是你调试好了，发布时把 script 目录里面的代码或者 [pyc](https://www.zhihu.com/search?q=pyc&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2336654649%7D) 压缩成要给 zip 文件，叫做 script.egg 放在 PyStand 那里删除 script 目录即可：

![](https://picx.zhimg.com/50/v2-0769bdb45512f31cc753e4ef2aabe583_720w.jpg?source=1940ef5c)

发布出来大概是这样，运行 PyStand.exe 成功的 import 到了 main.main() [函数](https://www.zhihu.com/search?q=%E5%87%BD%E6%95%B0&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2336654649%7D)：

![](https://picx.zhimg.com/50/v2-bfd073e9ff067b85501b714829b890b3_720w.jpg?source=1940ef5c)

主目录下面就三个文件，打包放到其他机器上解压就运行，不喜欢 PyStand.exe 这个名字可以随便改，同时修改 [PyStand.int](https://link.zhihu.com/?target=http%3A//pystand.int/) 的名字即可：

![](https://pic1.zhimg.com/50/v2-f78d5d9dd0145b5fe2bd345ec1483904_720w.jpg?source=1940ef5c)

比如这样，运行 PyQt-Demo.exe 它会根据自身的名字，正确的找到 [PyQt-Demo.int](https://link.zhihu.com/?target=http%3A//pyqt-demo.int/) 文件并执行。


# 加密

## **基础加密**

要求不高的话，上面你将 script 目录内的 .py 文件打包成 script.egg，直接就可以发布了，至少不会满目录的 .py 文件。要求高一点的话，把 .py 先转换成 .pyc 再压缩成 script.egg，然后把关键几个模块用 cython 之类的工具转换成 .pyd 即可。

上面基础加密基本够用了，个人开发者可以就此止步，如果你是一个团队，要发布面向百万以上用户产品级的东西，追求比 PyInstaller 更安全的加密方式可以继续往下。

## **高级加密**

接下来技巧我在 Python 2 时代都做过，你可以视精力酌情添加：

第一层：pyc 加密，自己写一个 importer，放到 [PyStand.int](https://link.zhihu.com/?target=http%3A//pystand.int/) 里初始化，作用是加载自定义的 `.pz` 文件，而 `.pz` 文件是根据 `.pyo` [文件加密](https://www.zhihu.com/search?q=%E6%96%87%E4%BB%B6%E5%8A%A0%E5%AF%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2336654649%7D)得到，你的 importer 负责解密并加载这些[字节码](https://www.zhihu.com/search?q=%E5%AD%97%E8%8A%82%E7%A0%81&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2336654649%7D)，把这个 importer 添加到 sys.path\_hooks 里面，这样 python 就能 import `.pz` 文件了，再写个[批处理](https://www.zhihu.com/search?q=%E6%89%B9%E5%A4%84%E7%90%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2336654649%7D)，把项目文件全部编译转换成 `.pz` 压缩成 script.egg。

第二层：zip 文件加密码，参考 python 自带的 zipimporter 实现一个 zipimporter2，支持 zip 文件加密码，只要在 sys.path.append('script.egg@12345') 类似这样的路径，就可以按给定密码 import zip 内的东西，当然密码可以写的不那么明显，还可以支持 7zip 导入。

第三层：将 .dll/.pyd 封装近 [python38.zip](https://link.zhihu.com/?target=http%3A//python38.zip/) 或者 script.egg 内，这里你会用到 py2exe 的两个子模块：MemoryModule：

![](https://picx.zhimg.com/50/v2-37e8915d0d9a5fcabde2bb9c14f74f5d_720w.jpg?source=1940ef5c)

地址：[https://github.com/py2exe/py2exe/tree/master/source](https://link.zhihu.com/?target=https%3A//github.com/py2exe/py2exe/tree/master/source)

可以用来从内存加载 dll/pyd，然后还有一个 zipdllimporter 的脚本，可以从内存/zip 文件直接加载 pyd/dll，这样你的所有的 pyd/dll 都可以塞到 .zip/.egg 文件里了，根本不用暴露。

第四层：[源代码](https://www.zhihu.com/search?q=%E6%BA%90%E4%BB%A3%E7%A0%81&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2336654649%7D)重新编译 Python，将很多东西直接编译进去，比如上面说的各种 importer 实现，[memory importer](https://www.zhihu.com/search?q=memory%20importer&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2336654649%7D)，加密 zip 文件之类的，并且支持加密的 [http://PyStand.int](https://link.zhihu.com/?target=http%3A//PyStand.int)。

第五层：修改字节码，找到 python 源代码的 [include/opcode.h](https://link.zhihu.com/?target=https%3A//github.com/python/cpython/blob/main/Include/opcode.h)：

![](https://pica.zhimg.com/50/v2-ad2d699d0f455a666cd86f6b06e1d997_720w.jpg?source=1940ef5c)

自己魔改一遍，基本[反编译](https://www.zhihu.com/search?q=%E5%8F%8D%E7%BC%96%E8%AF%91&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2336654649%7D)的程序都蒙圈了，再到 Include/internal/pycore\_ast.h 下面修改一些结构体的内部顺序，这样只要对方没有你的头文件，想从进程内存级别 intercept 进来获取字节码或者 ast 的都会非常麻烦。

第六层：[静态编译](https://www.zhihu.com/search?q=%E9%9D%99%E6%80%81%E7%BC%96%E8%AF%91&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2336654649%7D)，把所有第三方库和 python 自己静态编译成一个 exe 或者 dll，没有任何依赖，不暴露任何 dll 接口，集成上面说过的所有功能。由于你全部依赖都静态编译了，所以可以给 PyObject 里加两个无关的成员，调整一下已有成员顺序，别人就是进程截取 PyObject 的指针，由于没有[头文件](https://www.zhihu.com/search?q=%E5%A4%B4%E6%96%87%E4%BB%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2336654649%7D)，内部结构不知道，所以它也没有任何办法。

第七层：你的可执行每次启动会检测[可执行文件](https://www.zhihu.com/search?q=%E5%8F%AF%E6%89%A7%E8%A1%8C%E6%96%87%E4%BB%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2336654649%7D)末尾，是否有添加的内容，如果有，把他视为一个加密的[压缩包](https://www.zhihu.com/search?q=%E5%8E%8B%E7%BC%A9%E5%8C%85&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2336654649%7D)，在内存里解密并 import 对应模块，这样你上面的单个程序就可以和具体逻辑相分离，有了新的逻辑代码，[压缩加密](https://www.zhihu.com/search?q=%E5%8E%8B%E7%BC%A9%E5%8A%A0%E5%AF%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2336654649%7D)后添加在唯一可执行尾部即可。

。。。。。

还有很多类似的方法，这里仅仅抛砖引玉，没有绝对的安全，就是看你愿意投入多少人力，可以做到哪一层，个人的话，上面基础加密足够了，公司团队的话，安排人搞个一两个月，基本也就搞定了。

**错误调试**

这个 PyStand.exe 是窗口程序，那么出错了怎么看 [exception](https://www.zhihu.com/search?q=exception&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2336654649%7D) 呢？可以打开一个 cmd.exe，用 cmd.exe 启动 PyStand，就能看到错误了，你自己也可以记录下日志，catch 一下内部的 exception。

# 简单方法

一、关于对比PyInstaller的几个类似问题：

请直接参见[韦易笑](https://www.zhihu.com/search?q=%E9%9F%A6%E6%98%93%E7%AC%91&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3112029377%7D)大佬的回答：  
[怎么样打包 pyqt 应用才是最佳方案？或者说 pyqt 怎样的发布方式最优？](https://www.zhihu.com/question/48776632/answer/2336654649)

另外，找到韦大神的答案之前，答主肯定是试用过PyInstaller的，结论就是不太理想，尤其是项目里有很多本地模块DLL的那种，很折腾。我这个方法其实就相当于设定好模块加载路径，然后自带一个python直接调用，非常直观透明，因为不用先解包，启动速度也飞快。

二、我这个方式有些人觉得麻烦，其实很简单，[runtime]和pystand.exe都是不用变的，拷贝过来直接用，python3x.\_pth一般也不用改，pywin32的两个dll一共800K，也可以不管用不用到直接拷贝过来。

三、关于打包大小问题，python3.8\_x64的嵌入式包一共8M，也就是说如果没有其它依赖，你的包体可以控制在10M以内。如果为了极限压缩交付包，我一般用7-zip压缩，然后自带一个7z.exe(700K)，写个install.bat来解压缩，这样也就7-8M。如果用32位Python交付，还能再小一点。另外，也可以打包成7zip的自解压包。

另外，无论是用PyInstaller还是我的方案，都要用虚拟环境来控制要打包的第三方模块的，那位打包出200M的同学，估计就是把本地所有第三方模块都打进去了。

四、PyStand的详细用法可以直接去韦大神的Github仓库：[https://github.com/skywind3000/PyStand](https://link.zhihu.com/?target=https%3A//github.com/skywind3000/PyStand/releases)

还有这篇文章可以参考：[https://gist.github.com/myd7349/9f7c6334e67d1aee68a722a15df4a62a](https://link.zhihu.com/?target=https%3A//gist.github.com/myd7349/9f7c6334e67d1aee68a722a15df4a62a)

五、关于Cython加密构建，其实够开篇新帖子了，这里不展开了，可以参考下我的一个简单[小程序]：

[GitHub - rockswang/py\_optimize: Python Optimization Demo](https://link.zhihu.com/?target=https%3A//github.com/rockswang/py_optimize)

这里的setup.py可以接受通过[环境变量]传入的路径[通配符]，对指定的文件进行加密构建，还能自动清理.c文件和.py[源文件]（默认不打开，请配合构建[批处理]
TT\_CYEXC: 不构建成pyd的源文件黑名单  
TT\_DELETE\_SOURCE: 是否删除.c文件和python/cython源文件  
注意：路径添加逻辑是先根据白名单批量添加，再根据黑名单批量去除，[路径]支持空格分隔的多个GLOB通配符。  
举个例子，打开命令行窗口，运行下面命令就可以Cython构建了：

```bat
set TT_CYINC=*.py **/*.py
set TT_CYEXC=main.py
python setup.py build_ext --inplace
```

\-------------------- 原回答内容 ------------------------

都3202年了，Python的交付我感觉现在是非常简单方便的，不要再用[pyinstaller](https://www.zhihu.com/search?q=pyinstaller&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3112029377%7D)了，没有必要，我说说的我的方法吧，我现在写个小脚本都用这个法子交付了。

1\. 使用嵌入式Python的运行时，可以从Python官网下载，比如\`python-3.11.1-embed-amd64.zip\`，请下载和你的[开发环境]一致的版本。

2\. 解压了放在自己程序的runtime目录。

3\. 修改此目录中的 python3x.\_pth 文件，把最后一句\`#import site\`前的注释去掉，然后在前面以[相对路径]添加你的脚本所在的目录，比如\`..\`就是runtime目录同层，我一般脚本就直接放在这层，然后还要加上第三方库的相对路径，比如\`..\\site-packages\`。

4\. 关于依赖的第三方库，一般是创建一个[虚拟环境]，运行\`pip install -r requirements.txt\`，安装完以后，再手动把虚拟环境下的\`site-packages\`目录复制到打包目录。我一般用更简单的办法，不需要虚拟环境，直接运行\`pip install -r requirements.txt --target site-packages\`，这样直接就安装到指定的目录了。

5\. 注意一些特殊处理：

\* 如果你的库直接或间接引用了pywin32库，则需要在上面的python3x.\_pth里再添加几行，你也可以不管三七二十一都加上，没有影响：

```bat
../site-packages/win32  
../site-packages/win32/lib
../site-packages/Pythonwin
```

\* 如果你的site-packages目录下有pywin32\_system32这个[子目录]，把里面的两个DLL：pythoncom3x.dll, pywintypes3x.dll 复制到runtime目录里

6\. 写一个批处理用来启动你的python程序，内容如下：

```text
@echo off
set APPDIR=%~dp0
"%APPDIR%runtime\python.exe" "%APPDIR%main.py"
```

把main.py替换成你自己的程序入口，上面的批处理相当于用[绝对路径]启动了你的python脚本，这样你的脚本里可以用\_\_file\_\_正确获取到入口文件的路径。

7\. 如果希望有个exe作为入口，显得专业一些，可以把韦易笑大神的pystand.exe复制过来，并改成和你的入口脚本同名，比如main.exe。可用ResourceHacker直接修改掉pystand.exe的图标。

另外，我一般发布前都要用Cython把除了入口和第三方模块外所有的py文件编译成pyd，并把.py源文件删除的，这样可以完美保护源码。

