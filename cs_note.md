- [Zotero](#zotero)
  - [一键抓取doi](#一键抓取doi)
  - [设置Sci-Hub作为PDF解析器](#设置sci-hub作为pdf解析器)
  - [脚本](#脚本)
    - [批量将语言设置为en（英语）](#批量将语言设置为en英语)
  - [将文献的题目大小写修改为句首字母大写（Sentence case）](#将文献的题目大小写修改为句首字母大写sentence-case)
    - [将条目的题目大小写转为词首字母大写（Title Case）](#将条目的题目大小写转为词首字母大写title-case)
    - [将条目的题目大小写转为部分全部小写](#将条目的题目大小写转为部分全部小写)
    - [将Extra字段清空](#将extra字段清空)
    - [将作者大小写修改词首字母大写](#将作者大小写修改词首字母大写)
    - [批量删除（合并）重复文献](#批量删除合并重复文献)
    - [批量删除tags](#批量删除tags)
- [Ubuntu 服务器](#ubuntu-服务器)
  - [Complier](#complier)
  - [VNC](#vnc)
    - [vnc ssh隧道](#vnc-ssh隧道)
  - [中文显示支持](#中文显示支持)
  - [nividia](#nividia)
  - [查看发行版](#查看发行版)
  - [证书错误](#证书错误)
  - [后台执行](#后台执行)
  - [自动登录](#自动登录)
  - [VNC for Xfce](#vnc-for-xfce)
- [Miner](#miner)
  - [网络](#网络)
    - [linux](#linux)
    - [windows](#windows)
  - [nvidia linux 超频](#nvidia-linux-超频)
    - [ubuntu](#ubuntu)
    - [安装gwe](#安装gwe)
  - [windwos bat自启动文件](#windwos-bat自启动文件)
- [Aliyun 服务器](#aliyun-服务器)
  - [密码帐号](#密码帐号)
  - [frp](#frp)
    - [frps.ini](#frpsini)
    - [frpc.ini](#frpcini)
  - [frp开机自启动](#frp开机自启动)
- [Opencv](#opencv)
  - [编译选项](#编译选项)
  - [3rd包](#3rd包)
  - [安装opencv-contrib](#安装opencv-contrib)
- [wsl Arch](#wsl-arch)
- [manjaro](#manjaro)
  - [初始配置](#初始配置)
  - [rc-local](#rc-local)
- [Raspi](#raspi)
  - [初始配置](#初始配置-1)
  - [ubuntu sever](#ubuntu-sever)
  - [升级固件 U盘启动](#升级固件-u盘启动)
  - [添加用户到sudoer列表中](#添加用户到sudoer列表中)
  - [VNC XRDP](#vnc-xrdp)
  - [配置smb](#配置smb)
  - [硬盘挂载](#硬盘挂载)
    - [权限管理](#权限管理)
    - [nofail](#nofail)
    - [示例](#示例)
  - [配置aria2](#配置aria2)
  - [秘钥](#秘钥)
  - [Dlna](#dlna)
- [Python](#python)
  - [embeded python](#embeded-python)
  - [虚拟环境打包  pyinstaller](#虚拟环境打包--pyinstaller)
  - [打包pyd](#打包pyd)
- [Docker](#docker)
  - [](#)
  - [Docker Hub](#docker-hub)
  - [数据卷容器（Data volume containers）](#数据卷容器data-volume-containers)
  - [数据卷（Data volumes）](#数据卷data-volumes)
  - [Docker 命令](#docker-命令)
  - [Docker Aira](#docker-aira)
- [CZU服务器centos](#czu服务器centos)
  - [服务器密码](#服务器密码)
    - [todesk](#todesk)
  - [vmware](#vmware)
    - [VMware Fusion Player 13 – Personal Use](#vmware-fusion-player-13--personal-use)


# Zotero

## 一键抓取doi

从GitHub上下载最新版本

## 设置Sci-Hub作为PDF解析器

打开Zotero的首选项，进入Advanced-->Config Editor

```
extensions.zotero.findPDFs.resolvers
```

删除[]，并将以下代码粘贴进去

```
{
    "name":"Sci-Hub",
    "method":"GET",
    "url":"https://sci-hub.se/{doi}",
    "mode":"html",
    "selector":"#pdf",
    "attribute":"src",
    "automatic":true
}
```

## 脚本

### 批量将语言设置为en（英语）

```
zoteroPane = Zotero.getActiveZoteroPane();
items = zoteroPane.getSelectedItems();
var rn=0; 
var lan="en"; 
for (item of items) {
var la = item.getField("language");
if (la=="")  
 {item.setField("language", lan);
rn+=1;
 await item.saveTx();
}

}
return 
```

## 将文献的题目大小写修改为句首字母大写（Sentence case）

```

var items = ZoteroPane.getSelectedItems();
var n = 0;
for (item of items) {
            var title = item.getField('title');
            new_title = title.toLowerCase()
            
            new_title = new_title.replace(new_title[0],new_title[0].toUpperCase())
                            
            item.setField('title', new_title);
            await item.saveTx();
            n ++
};

return n + '个条目完成';

```

### 将条目的题目大小写转为词首字母大写（Title Case）

```
var items = ZoteroPane.getSelectedItems();
var n = 0;
for (item of items) {
            var title = item.getField('title');
            new_title = titleCase(title). // 转换为单词首字母大写
                            replace('And', 'and'). // 替换And
                            replace('For', 'for'). // 替换For
                            replace('In', 'in'). // 替换In
                            replace('Of', 'of'). // 替换Of
                            replace('With', 'with').
                            replace('Usa', 'USA').
                            replace('Dna', 'DNA').
                            replace('Pcr', 'PCR').
                            replace('Ros', 'ROS').
                            replace('To', 'to')
                            
                            
                            
            item.setField('title', new_title);
            await item.saveTx();
            n ++
};

return n + '个条目的题目大小写转为词首字母大写，有些特殊缩写单词可能转换错误，请查实。';

// 将单词转为首字母大写
function titleCase(str) {   
     var newStr = str.split(" ");    
     for(var i = 0; i < newStr.length; i++) {
        newStr[i] = newStr[i].slice(0,1).toUpperCase() + newStr[i].slice(1).toLowerCase();
        }      
     return newStr.join(" ");
};
```

### 将条目的题目大小写转为部分全部小写

```
var items = ZoteroPane.getSelectedItems();
var n = 0;
for (item of items) {
            var title = item.getField('title');
            new_title = title.toLowerCase()
                            
                            
                            
            item.setField('title', new_title);
            await item.saveTx();
            n ++
};

return n + '个条目的题目大小写转为全部大写，请查实。';

```

### 将Extra字段清空

```
zoteroPane = Zotero.getActiveZoteroPane();
items = zoteroPane.getSelectedItems();
for (item of items) {
item.setField("extra", "");
await item.saveTx();
}
//alert("Extra已清除完成")
return "Extra已清除完成";
```

### 将作者大小写修改词首字母大写

```

var newFieldMode = 0; // 0: two-field, 1: one-field (with empty first name)
var s = new Zotero.Search();
s.libraryID = ZoteroPane.getSelectedLibraryID();
var tagName = 'try'; //需要提前将需要修改的项目添加标签
s.addCondition('tag', 'is', tagName);
//s.addCondition('creator', 'is', oldName);
 
var ids = await s.search();
if (!ids.length) {
    return "No items found";
}

await Zotero.DB.executeTransaction(async function () {
        for (let id of ids) {
            var item = await Zotero.Items.getAsync(id);
            var creators = item.getCreators();

            let newCreators = [];
            for (let creator of creators) {
                creator.firstName = titleCase(creator.firstName.trim());
                creator.lastName = titleCase(creator.lastName.trim());
                creator.fieldMode = newFieldMode;
                newCreators.push(creator);
                } 
            item.setCreators(newCreators);
            await item.save();
            }  

}); 
return ids.length + "个条目作者变为作者首字母大写。";


function titleCase(str) {    var newStr = str.split(" ");    for(var i = 0; i<newStr.length; i++){
        newStr[i] = newStr[i].slice(0,1).toUpperCase() + newStr[i].slice(1).toLowerCase();
    }    return newStr.join(" ");
}
```

### 批量删除（合并）重复文献

```

var DupPane = Zotero.getZoteroPanes();
for(var i = 0; i < 1000; i++) {
await new Promise(r => setTimeout(r, 1000));
DupPane[0].mergeSelectedItems();
Zotero_Duplicates_Pane.merge();
}
```

### 批量删除tags

```

var items = ZoteroPane.getSelectedItems();
var n = 0;
for (item of items) {
            item.setField('tags', '';
            await item.saveTx();
            n ++
};

return n + '个tag清空';


```

# Ubuntu 服务器

## Complier

`sudo apt install build-essential crossbuild-essential-arm64 crossbuild-essential-armel crossbuild-essential-armhf crossbuild-essential-riscv64 crossbuild-essential-amd64`

## VNC

### vnc ssh隧道

```
ssh -L 5901:127.0.0.1:5901 -N -f -l jiang 192.168.3.20
```

## 中文显示支持

```
sudo apt-get install ttf-wqy-zenhei
```

## nividia

```
nividia-smi

# 出现建议，不要选 util-xxx
sudo apt install nvidia-xxx
sudo apt-get install dkms

cd /usr/src

sudo reboot
```

## 查看发行版

```
lsb_release -a
```

## 证书错误

```
sudo apt-get install -y ca-certificates
```

## 后台执行

```
nohup command &
```

## 自动登录

在终端模拟器使用命令

```
sudo nano /etc/lightdm/lightdm.conf
```

编辑文件/etc/lightdm/lightdm.conf，修改为：

```
[SeatDefaults]
user-session=xubuntu
greeter-session=lightdm-gtk-greeter
autologin-user=yuanfei #请替换为自己的用户名
autologin-user-timeout=0
```

## VNC for Xfce

 如果没有图形界面

```
sudo apt install xfce4 xfce4-goodies xorg dbus-x11 x11-xserver-utils
```

```
sudo apt install tigervnc*
```

```
vncserver # 输入密码 view only 必须

```

```
vncserver -kill :1
```

配置 VNC 服务器

```
nano ~/.vnc/xstartup
```

```
#!/bin/sh
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
exec startxfce4 
```

```
chmod u+x ~/.vnc/xstartup
```

```
nano ~/.vnc/config
```

```
geometry=1920x1084
dpi=96
```

创建 Systemd 单元文件

```
sudo nano /etc/systemd/system/vncserver@.service
```

务必更改第 7 行中的用户名以匹配您的用户名。

```
[Unit]
Description=Remote desktop service (VNC)
After=syslog.target network.target

[Service]
Type=simple
User=username ##### 匹配用户
PAMName=login
PIDFile=/home/%u/.vnc/%H%i.pid
ExecStartPre=/bin/sh -c '/usr/bin/vncserver -kill :%i > /dev/null 2>&1 || :'
ExecStart=/usr/bin/vncserver :%i -geometry 1440x900 -alwaysshared -fg
ExecStop=/usr/bin/vncserver -kill :%i

[Install]
WantedBy=multi-user.target
```

```
sudo systemctl daemon-reload
sudo systemctl start vncserver@1.service
sudo systemctl enable vncserver@1.service
sudo systemctl status vncserver@1.service
```

连接到 VNC 服务器

```
ssh -L 5901:127.0.0.1:5901 -N -f -l username server_ip_address
```

该-L开关指定的端口绑定。在这种情况下，我们将5901远程连接的端口5901绑定到本地计算机上的端口。该-C开关启用压缩，而-N开关告诉ssh我们不希望执行远程命令。该-l开关指定远程登录名。

记得替换 username，并 server_ip_address 与您的服务器的须藤非 root 用户名和 IP 地址。

如果您使用的是图形化 SSH 客户端（如 PuTTY），请将 server_ip_address 用作连接 IP，并在程序的 SSH 隧道设置中设置localhost:5901为新的转发端口。

隧道运行后，使用 VNC 客户端进行连接localhost:5901。系统将提示您使用在步骤 1 中设置的密码进行身份验证。

```
ssh -L 5901:127.0.0.1:5901 -N -f -l jiang 192.168.3.102
```

# Miner

## 网络

### linux

### windows

1 先在网络里面改变wifi和有线跃点，使得wifi和有线都生效
2 关闭自动配置，手动写ip，不写路由（防止每次自动生成0.0.0.0路由）

```
# 删除默认
route delete 0.0.0.0

# 添加无线， -p 重启也生效
route add 16.162.79.108 mask 255.255.255.255 10.168.1.1 -p

# 添加有线，metric控制跃点，配置跃点使得wifi和有线同时使用（使得wifi跃点小于有线）
route add 0.0.0.0 mask 0.0.0.0 192.168.3.1 metric 5  -p 
```

## nvidia linux 超频

生成xorg

```
sudo nvidia-xconfig
```

### ubuntu

```
sudo nvidia-xconfig -a --allow-empty-initial-configuration --cool-bits=28
```

### 安装gwe

```
sudo apt install git meson python3-pip libcairo2-dev libgirepository1.0-dev libglib2.0-dev libdazzle-1.0-dev gir1.2-gtksource-3.0 gir1.2-appindicator3-0.1 python3-gi-cairo appstream-util
cd && cd Downloads
git clone --recurse-submodules -j4 https://gitlab.com/leinardi/gwe.git
cd gwe
git checkout release
sudo pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
sudo pip3 install -r requirements.txt
meson . build --prefix /usr
ninja -v -C build
sudo ninja -v -C build install
```

manjaro

```
sudo nvidia-xconfig --cool-bits=28
sudo reboot
```

```
#安装 greenwithenvy
yay -S gwe
```

直接在图形界面下超频，记得回车

## windwos bat自启动文件

```
@echo off
start cmd /k "cd C:\Users\jiang_bi\Desktop\frp && .\frpc.exe -c .\frpc.ini"
start cmd /k "cd C:\Users\jiang_bi\Desktop\gminer && .\mine_eth.bat"
start cmd /k "cd C:\Users\jiang_bi\Desktop&& python .\pyConnect.py"
```

放入

```
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp

or：

Win+R，在打开的运行程序中输入 shell:startup，回车
```

# Aliyun 服务器

## 密码帐号

```
114.55.174.213
root
zaqxsw123!
```

## frp

### frps.ini

```
[common]
# 7000/7001 for bhhs 2070s
bind_port = 7000
dashboard_port = 7500
dashboard_user = jybjybjyb
dashboard_pwd = zaqxsw123!
authentication_method=token
token=zaqxsw123!
```

```
./frps -c ./frps.ini
```

### frpc.ini

```
### 2080 ti

[common]
server_addr = 114.55.174.213
server_port = 7000
authentication_method=token
token=zaqxsw123!


[jssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 20022

[jxrdp]
type=tcp
local_ip=127.0.0.1
local_port=3389
remote_port=23389

```

```
### 2070s
[common]
server_addr = 114.55.174.213
server_port = 7000
authentication_method=token
token=zaqxsw123!


[ssh2070s]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 30022

[xrdp2070s]
type=tcp
local_ip=127.0.0.1
local_port=3389
remote_port=33389

```

```
### 3080ti-2
[common]
server_addr = 114.55.174.213
server_port = 7000
authentication_method=token
token=zaqxsw123!


[ssh3080ti-2]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 40022

[xrdp3080ti-2]
type=tcp
local_ip=127.0.0.1
local_port=3389
remote_port=43389

```

## frp开机自启动

新建一个文件内容如下：

```
/etc/systemd/system/frps.service
或者
/etc/systemd/system/frpc.service
```

注意事项 frps 还是 frpc

```
[Unit]
Description=frps
After=network.target

[Service]
Type=simple
ExecStart=/root/frp/frps -c /root/frp/frps.ini
# 修改为你的frp实际安装目录
ExecStop=/bin/kill $MAINPID
#启动失败1分钟后再次启动
RestartSec=1min
KillMode=control-group
#重启控制：总是重启
Restart=always

[Install]
WantedBy=multi-user.target

```

```
sudo systemctl enable frpc.service
sudo systemctl start frpc.service
sudo systemctl status frpc.service
```

# Opencv

## 编译选项

world

nonfree

python：注意先安装numpy，检查config信息必须有如下信息

```
Python 3:
    Interpreter:                 /usr/bin/python3 (ver 3.8.10)
    Libraries:                   /usr/lib/x86_64-linux-gnu/libpython3.8.so (ver 3.8.10)
    numpy:                       /usr/lib/python3/dist-packages/numpy/core/include (ver 1.17.4)
    install path:                lib/python3.8/dist-packages/cv2/python-3.8
    
```

## 3rd包

```
ping raw.githubusercontent.com

sudo nano /etc/hosts

加入
ping值 raw.githubusercontent.com
```

## 安装opencv-contrib

树莓派

```
sudo apt-get install ffmpeg libhdf5-dev libhdf5-serial-dev libqtgui4 libqtwebkit4 libqt4-test python3-pyqt5 libatlas-base-dev libjasper-dev  libjpeg-dev libtiff-dev libjasper-dev libpng-dev libavcodec-dev libavformat-dev libswscale-dev libv4l-dev libxvidcore-dev libx264-dev libgtk-3-dev libcanberra-gtk* libatlas-base-dev gfortran python3-dev cmake -y
```

```
cmake -D CMAKE_BUILD_TYPE=RELEASE \
   -D CMAKE_INSTALL_PREFIX=/usr/local \
   -D OPENCV_EXTRA_MODULES_PATH=~/app/opencv451/opencv_contrib/modules \
   -D ENABLE_NEON=ON \
   -D ENABLE_VFPV3=ON \
   -D BUILD_TESTS=OFF \
   -D OPENCV_ENABLE_NONFREE=ON \
   -D INSTALL_PYTHON_EXAMPLES=OFF \
   -D INSTALL_C_EXAMPLES=OFF \
   -D WITH_FFMPEG=ON \
   -D BUILD_EXAMPLES=OFF ..
   
make
```

ubuntu20.04

```
sudo apt-get install build-essential cmake git pkg-config libgtk-3-dev \
    libavcodec-dev libavformat-dev libswscale-dev libv4l-dev \
    libxvidcore-dev libx264-dev libjpeg-dev libpng-dev libtiff-dev \
    gfortran openexr libatlas-base-dev python3-dev python3-numpy \
    libtbb2 libtbb-dev libdc1394-22-dev libopenexr-dev \
    libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev
```

```
cmake-gui

ON：
OPENCV_GENERATE_PKGCONFIG
OPENCV_EXTRA_MODULES_PATH
OPENCV_ENABLE_NONFREE
WORLD

OFF：
TEST
EXAMPLE
```

```
cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D INSTALL_C_EXAMPLES=OFF \
    -D INSTALL_PYTHON_EXAMPLES=OFF \
    -D OPENCV_GENERATE_PKGCONFIG=ON \
    -D OPENCV_EXTRA_MODULES_PATH=~/app/opencv451/opencv_contrib/modules \
    -D BUILD_EXAMPLES=OFF ..

make -j
```

```

sudo make install
sudo ldconfig
```

验证

```
pkg-config --modversion opencv4
python3 -c "import cv2; print(cv2.__version__)"
```

```
sudo apt-get install libopencv-contrib3.2 python-opencv
sudo apt-get remove libopencv-contrib3.2 python-opencv
手动安装wheel

sudo apt update
sudo apt install libopencv-dev python3-opencv

```

# wsl Arch

打开 PowerShell 并运行,将 WSL 2 设置为默认版本

```
wsl --set-default-version 2
```

选择一个空目录（例如 D:/Arch）并解压 Arch.zip 到此处

```
.\Arch.exe
```

初始化包管理器 pacman

```
# nano /etc/pacman.conf
// 推荐取消注释 Misc options 中的 Color、CheckSpace 及 VerbosePkgLists 项
// 并添加 DisableDownloadTimeout 及 ParallelDownloads = 2
# nano /etc/pacman.d/mirrorlist
// 请使用 https
```

初始化并安装

```
pacman-key --init
pacman-key --populate
pacman -Syy 
pacman -S archlinux-keyring
pacman -S base-devel git zsh cmake net-tools
```

退出并重新打开 Arch Linux

# manjaro

## 初始配置

```
sudo pacman-mirrors -i -c China -m rank

sudo nano /etc/pacman.conf

[archlinuxcn]
SigLevel = Optional TrustedOnly
Server = http://mirrors.tuna.tsinghua.edu.cn/archlinuxcn//$arch
```

```
# 更新数据源
sudo pacman -Syy
# 安装导入 GPG key
sudo pacman -S archlinuxcn-keyring

```

```
sudo pacman -S yay
yay --aururl "https://aur.tuna.tsinghua.edu.cn" --save
```

```
# 更新系统
sudo pacman -Syu
```

```
# 基础库
sudo pacman -Sy base-devel net-tools
```

```
# 中文
sudo pacman -S fcitx-im fcitx-configtool fcitx-googlepinyin

nano ~/.xprofile

export LC_ALL=zh_CN.UTF-8
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=“@im=fcitx”

```

## rc-local

虽然 Arch Linux 没有默认支持 rc-local.service 服务，但是我们可以手动安装这个服务并设置开机启动：

```
sudo pacman -Sy systemd-rc-local
sudo systemctl enable rc-local.service
```

要想使用 rc-local.service 服务在系统启动时运行用户自定义的脚本命令，首先需要在 /etc 目录下创建 rc.local 文件并修改可执行权限：

```
sudo touch /etc/rc.local
sudo chmod 755 /etc/rc.local
```

打开 /etc/rc.local 文件，往其中添加运行自定义脚本的命令即可。

【注】/etc/rc.local 以及自定义脚本中都不能使用系统变量（比如 $HOME，原因在于其执行自定义脚本时并没有继承系统变量）。

查看脚本执行结果

```
systemctl status rc-local.service
```

# Raspi

## 初始配置

1. 在写入新系统镜像后，不要弹出SD卡；在其中新建ssh的空文件。（不要后缀名）
1. 2022-4之前 系统用户名：pi 密码：raspberry
1. sudo raspi-config // 配置ssh, VNC, etc..

//系统用户名：pi 密码：q123456

在boot分区创建文件userconf

文件内容

用户名:加密后的密码

加密密码的方法：

echo '明文密码' | openssl passwd -6 -stdin

得到 q123456 的密文如下

```
$6$O/bGVyH9YxcfCXBL$RTnaolLfeS9KDVOPqaox8/ZdByWlBmx.KwAUg39JnRr8cXMIa09o2rSDFaGlpcaUS9XPiRvSpW1jXwsvKK4eS/
```

## ubuntu sever

```
# system-boot/network-config
wifis:
  wlan0:
  dhcp4: true
  optional: true
  access-points:
    <wifi network name>:
      password: "<wifi password>"
      
      

      
<!--启动比较麻烦，得有耐心，开机后需要等10分钟左右，系统会创建用户ubuntu，
然后你需要在路由器中查看树莓派的IP地址，如果没找到，不要慌，在确保wifi信息配置无误的情况下，
重启树莓派，再等10分钟左右去路由器看一下，应该就有了。-->


# username: ubuntu
# password: ubuntu

<!--不要使用清华源-->
<!--可以使用华为源-->
sudo sed -i "s@http://.*archive.ubuntu.com@http://repo.huaweicloud.com@g" /etc/apt/sources.list
sudo sed -i "s@http://.*security.ubuntu.com@http://repo.huaweicloud.com@g" /etc/apt/sources.list

sudo apt-get install net-tools

# GUI
sudo apt install xubuntu-desktop
sudo apt install lubuntu-desktop
sudo apt install kubuntu-desktop

```

## 升级固件 U盘启动

```
sudo apt-get update && sudo apt upgrade
sudo apt-get install rpi-eeprom
sudo rpi-eeprom-update
// 编辑/etc/default/rpi-eeprom-update文件，把内容改为FIRMWARE_RELEASE_STATUS="stable"
sudo rpi-eeprom-update -a
```

## 添加用户到sudoer列表中

```
nano /etc/sudoers
    root    ALL=(ALL)       ALL
    your_user_name ALL=(ALL) NOPASSWD: ALL
    %admin ALL=(ALL) NOPASSWD: ALL
```

一定加在最后

## VNC XRDP

XRDP

```
sudo apt-get -y install xrdp
sudo adduser xrdp ssl-cert
sudo systemctl enable xrdp
sudo systemctl start xrdp
mstsc // windows端
```

VNC

```
sudo apt-get install tightvncserver
vncpasswd  
// 一定要！！！！mlgb千万要设置view-only密码
```

```
# 启动 vnc
vncserver // 5900端口 :1 指的是5901
vncserver -kill :1
vncserver -geometry 1440x900 -dpi 96 :1
```

## 配置smb

```
sudo apt-get install samba samba-common-bin
sudo nano /etc/samba/smb.conf
```

```
sudo nano /etc/samba/smb.conf
[home]
readonly = no

[pi]
  path = /home/pi
  available = yes
  browseable = yes
  #public = yes
  writable = yes
  create mask = 0777
  directory mask = 0777

```

```
//samba只能创建linux已有的用户账户
//比如系统中没有abc这个用户
//samba就无法创建abc账户
sudo smbpasswd -a pi
sudo systemctl restart smbd.service 
sudo systemctl enable smbd.service
```

```
//修改读写权限
sudo chown -R ubuntu:ubuntu /home/pi/share
sudo chmod -R 777 /home/pi/share
service smbd restart //重启samba服务
//即可进行连接测试（在运行窗口中输入\\ip即可访问）
```

## 硬盘挂载

```
sudo blkid
/dev/mmcblk0p1: SEC_TYPE="msdos" UUID="9FF3-2ADB" TYPE="vfat" PARTUUID="c2909e2d-01"
/dev/mmcblk0p2: UUID="a407e17a-e170-45d8-8fb7-dd34fe646ab9" TYPE="ext4" PARTUUID="c2909e2d-02"
/dev/mmcblk0: PTUUID="c2909e2d" PTTYPE="dos"
/dev/sda1: LABEL="New Volume" UUID="848CC9268CC91398" TYPE="ntfs" PARTUUID="000d2ae0-01"
```

"38E28323E282E50A"

"4094FB7294FB68B4"

“E62E141E2E13E5F9”

```
sudo nano /etc/fstab // 编辑设备管理,在最后一行添加你要挂载的设备
```

### 权限管理

```
umask, fmask, dmask, uid, gid
umask用来指定挂载windows分区后文件的默认权限
（事实上，是默认没有的权限，即umask参数指出的值挂载后的文件将不具有），
因为Windows分区里面的文件是没有权限这个概念的，所以要手动指定默认权限，
于是指定umask为0000,就是不排除任何，即具有所有权限

id #查看uid和gid
# 在default后面加上uid和gid
/dev/sda3      /media/program    ntfs defaults,uid=1000,gid=1000     0       0
```

### nofail

即使boot阶段没有接入移动硬盘，也能正常启动。

### 示例

```
UUID=38E28323E282E50A /media/pi ntfs defaults,nofail,x-systemd.device-timeout=1,noatime 0 0

UUID=38E28323E282E50A /media/pi ntfs defaults,nofail,noatime,umask=0000,uid=1000,gid=1000 0 0
```

```
options常用参数类型：

auto – 在启动时或键入了 mount -a 命令时自动挂载。
noauto – 只在你的命令下被挂载。
exec – 允许执行此分区的二进制文件。
noexec – 不允许执行此文件系统上的二进制文件。
ro – 以只读模式挂载文件系统。
rw – 以读写模式挂载文件系统。
user – 允许任意用户挂载此文件系统，若无显示定义，隐含启用 noexec, nosuid, nodev 参数。
users – 允许所有 users 组中的用户挂载文件系统.
nouser – 只能被 root 挂载。
owner – 允许设备所有者挂载.
sync – I/O 同步进行。
async – I/O 异步进行。
dev – 解析文件系统上的块特殊设备。
nodev – 不解析文件系统上的块特殊设备。
suid – 允许 suid 操作和设定 sgid 位。这一参数通常用于一些特殊任务，使一般用户运行程序时临时提升权限。
nosuid – 禁止 suid 操作和设定 sgid 位。
noatime – 不更新文件系统上 inode 访问记录，可以提升性能(参见 atime 参数)。
nodiratime – 不更新文件系统上的目录 inode 访问记录，可以提升性能(参见 atime 参数)。
relatime – 实时更新 inode access 记录。只有在记录中的访问时间早于当前访问才会被更新。（与 noatime 相似，但不会打断如 mutt 或其它程序探测文件在上次访问后是否被修改的进程。），可以提升性能(参见 atime 参数)。
flush – vfat 的选项，更频繁的刷新数据，复制对话框或进度条在全部数据都写入后才消失。
defaults – 使用文件系统的默认挂载参数，例如 ext4 的默认参数为:rw, suid, dev, exec, auto, nouser, async.

```

## 配置aria2

```
sudo apt install -y aria2 
mkdir -p ~/.config/aria2/
nano ~/.config/aria2/aria2.config
```

```
#后台运行
daemon=true
#用户名
#rpc-user=user
#密码
#rpc-passwd=passwd
#设置加密的密钥
rpc-secret=secret
#允许rpc
enable-rpc=true
#允许所有来源, web界面跨域权限需要
rpc-allow-origin-all=true
#是否启用https加密，启用之后要设置公钥,私钥的文件路径
#rpc-secure=true
#启用加密设置公钥
#rpc-certificate=/home/pi/.config/aria2/example.crt
#启用加密设置私钥
#rpc-private-key=/home/pi/.config/aria2/example.key
#允许外部访问，false的话只监听本地端口
rpc-listen-all=true
#RPC端口, 仅当默认端口被占用时修改
#rpc-listen-port=6800
#最大同时下载数(任务数), 路由建议值: 3
max-concurrent-downloads=5
#断点续传
continue=true
#同服务器连接数
max-connection-per-server=5
#最小文件分片大小, 下载线程数上限取决于能分出多少片, 对于小文件重要
min-split-size=10M
#单文件最大线程数, 路由建议值: 5
split=10
#下载速度限制
max-overall-download-limit=0
#单文件速度限制
max-download-limit=0
#上传速度限制
max-overall-upload-limit=0
#单文件速度限制
max-upload-limit=0
#断开速度过慢的连接
#lowest-speed-limit=0
#验证用，需要1.16.1之后的release版本
#referer=*
#文件保存路径, 默认为当前启动位置(我的是外置设备，请自行坐相应修改)
dir=/media/piusb/TDDOWNLOAD
#文件缓存, 使用内置的文件缓存, 如果你不相信Linux内核文件缓存和磁盘内置缓存时使用, 需要1.16及以上版本
#disk-cache=0
#另一种Linux文件缓存方式, 使用前确保您使用的内核支持此选项, 需要1.15及以上版本(?)
#enable-mmap=true
#文件预分配, 能有效降低文件碎片, 提高磁盘性能. 缺点是预分配时间较长
#所需时间 none < falloc ? trunc << prealloc, falloc和trunc需要文件系统和内核支持
file-allocation=prealloc
#不进行证书校验
check-certificate=false
#保存下载会话
save-session=/home/pi/.config/aria2/aria2.session
input-file=/home/pi/.config/aria2/aria2.session
#断电续传
save-session-interval=60

bt-tracker=udp://62.138.0.158:6969/announce,udp://87.233.192.220:6969/announce,udp://111.6.78.96:6969/announce,udp://90.179.64.91:1337/announce,udp://51.15.4.13:1337/announce,udp://151.80.120.113:2710/announce,udp://191.96.249.23:6969/announce,udp://35.187.36.248:1337/announce,udp://123.249.16.65:2710/announce,udp://210.244.71.25:6969/announce,udp://78.142.19.42:1337/announce,udp://173.254.219.72:6969/announce,udp://51.15.76.199:6969/announce,udp://51.15.40.114:80/announce,udp://91.212.150.191:3418/announce,udp://103.224.212.222:6969/announce,udp://5.79.83.194:6969/announce,udp://92.241.171.245:6969/announce,udp://5.79.209.57:6969/announce,udp://82.118.242.198:1337/announce,udp://tracker.coppersurfer.tk:6969/announce,udp://tracker.open-internet.nl:6969/announce,udp://tracker.skyts.net:6969/announce,udp://tracker.piratepublic.com:1337/announce,udp://tracker.opentrackr.org:1337/announce,udp://9.rarbg.to:2710/announce,udp://retracker.coltel.ru:2710/announce,udp://pubt.in:2710/announce,udp://public.popcorn-tracker.org:6969/announce,udp://z.crazyhd.com:2710/announce,udp://wambo.club:1337/announce,udp://tracker4.itzmx.com:2710/announce,udp://tracker1.wasabii.com.tw:6969/announce,udp://tracker.zer0day.to:1337/announce,udp://tracker.xku.tv:6969/announce,udp://tracker.vanitycore.co:6969/announce,udp://ipv4.tracker.harry.lu:80/announce,udp://inferno.demonoid.pw:3418/announce,udp://open.facedatabg.net:6969/announce,udp://mgtracker.org:6969/announce


```

```
touch /home/pi/.config/aria2/aria2.session
## 测试aria2是否启动成功
aria2c --conf-path=/home/pi/.config/aria2/aria2.config
# 用 ps aux|grep aria2 看是否有进程启动，若有说明启动成功了。 
# 附：强制结束进程kill -9 3140（相应pid）
## 没有任何提示则表示成功。
```

```
//添加开机自启：
sudo nano /lib/systemd/system/aria.service
```

```
[Unit]
Description=Aria2 Service
After=network.target
[Service]
User=pi
Type=forking
ExecStart=/usr/bin/aria2c --conf-path=/home/pi/.config/aria2/aria2.config
[Install]
WantedBy=multi-user.target
```

```
## 重新载入服务，并设置开机启动
sudo systemctl daemon-reload
sudo systemctl enable aria
## 查看aria服务状态
sudo systemctl status aria
## 启动，停止，重启aria服务
sudo systemctl（start、stop、restart） aria
配置 nginx 管理
# 安装git和nginx
sudo apt-get install -y git nginx
# 下载aira-ng
wget https://github.com/mayswind/AriaNg/releases/download/0.4.0/aria-ng-0.4.0.zip -O aira-ng.zip
# 解压
unzip aria-ng.zip -d aria-ng
# 将aria-ng放到nginx的/var/www/html/目录下，然后设置开机启动nginx
sudo mv aria-ng /var/www/html/
sudo systemctl enable nginx
```

用浏览器访问树莓派IP下的aira-ng，即：<http://192.168.1.xxx/aira-ng> 然后在系统设置点击AriaNg设置 –> 全局 –> 设置语言为中文 –> 点击RPC–>修改为 rpc 密钥：secret

## 秘钥

```
ssh-keygen
cd ~/.ssh
cat id_rsa.pub >> authorized_keys
sudo service sshd restart
```

## Dlna

```
//ntfs
sudo apt-get install -y ntfs-3g minidlna
sudo nano /etc/minidlna.conf
```

```
# /etc/minidlna.conf
db_dir=/home/pi/db   
log_dir=/home/pi/log    
inotify=yes    
```

```
sudo /etc/init.d/minidlna restart
sudo /etc/init.d/minidlna status
```

```
sudo update-rc.d minidlna defaults
sudo service minidlna start
sudo service minidlna force-reload //强制刷新
sudo update-rc.d -f minidlna remove
sudo service minidlna stop
sudo killall minidlna
sudo apt-get remove --purge minidlna
```




# Python

## embeded python

1. 下载 embed 版 Python 并解压： <https://www.python.org/downloads/windows/>
2. 下载 get-pip 并放入 embed 版 Python 文件夹中： <https://pip.pypa.io/en/latest/installing/>
3. 打开 embed 版 Python 中的 python**._pth，其中**是版本号，掉 import site 前的注释。
3. 命令行运行 .\python.exe .\get-pip.py
4. 安装需要的 python 模块 .\python.exe -m pip install 模块名 -i <https://pypi.doubanio.com/simple> --no-warn-script-location
5. 建立一个 bat 的启动脚本，内容：
@.\python.exe .\程序的入口文件.py
@pause

## 虚拟环境打包  pyinstaller

```
# 第一次
pipenv --python 3.7.4
```

```
pipenv install  #建立虚拟环境 
pipenv shell  # 进入虚拟环境（上一步可省略,因为没有虚拟环境的话会自动建立一个）  
pip install xxx  #安装模块 
pip install pyinstaller  #打包的模块也要安装 
pyinstaller -Fw E:\test\url_crawler.py #开始打包 
# 选项格式说明-D, --onedir –创建一个包含可执行文件的单个文件夹包
# 默认-F, --onefile–创建一个文件捆绑的单一的可执行文件
# -w 表示去掉控制台窗口
```

一闪而过
控制台运行exe或者添加

```
input("please input any key to exit!")
```

打包失败 no module named pkg_resources.py2_warn pyinstaller

```
我的方法是：找到 \Python37\Lib\site-packages\pkg_resources\__init__.py，把86行 注释掉。重新打包即可。

__import__('pkg_resources.extern.packaging.version')
__import__('pkg_resources.extern.packaging.specifiers')
__import__('pkg_resources.extern.packaging.requirements')
__import__('pkg_resources.extern.packaging.markers')
#__import__('pkg_resources.py2_warn')


__metaclass__ = type
```

## 打包pyd

```
# build_pyd.py
# -*- coding: utf-8 -*-
"""
@author:
@ Email:
"""

from distutils.core import setup
from Cython.Build import cythonize
import cv2

setup(
    name='any words.....',
    ext_modules=cythonize(["tst_package_dll.py",
                           ]
                          ),
)
```

```
python build_pyd.py build_ext --inplace
```

# Docker

##

nvidia

```
docker run --gpus all -it tensorflow/tensorflow:latest-gpu bash
```

## Docker Hub

```
jybjybjyb
fe1d42c8-262f-4653-a930-9e0d0299c10a
```

## 数据卷容器（Data volume containers）

```
# 创建一个命名的数据卷容器 dbdata
docker run -it -v /dbdata --name dbdata ubuntu

# 在其他容器中使用--volumes-from来挂载 dbdata 容器中的数据卷
docker run -it --volumes-from dbdata --name db1 ubuntu
```

容器db1和db2都挂载同一个数据卷到相同的/dbdata目录.三个容器任何一方在该目录下的写入,任何容器都能看到。
使用--volumes-from参数所挂载数据卷的容器自身并不需要保持在运行状态
如果删除了挂载的容器（包括 dbdata、db1 和 db2），数据卷并不会被自动删除。如果要删除一个数据卷，必须在删除最后一个还挂载着它的容器时使用 docker rm -v命令来指定同时删除关联的容器。这可以让用户在容器之间升级和移动数据卷。

## 数据卷（Data volumes）

在用 docker run 命令的时候，使用 -v 标记来创建一个数据卷并挂载到容器里。在一次 run 中多次使用可以挂载多个数据卷。

```
docker run -d -v /test:/
```

## Docker 命令

```
// root 启动（默认是UID 1000，啥权限没有）
// connect via VNC viewer localhost:5901
docker run -d -p 5901:5901 -p 6901:6901 --user 0 xxxxxx

// image命令
docker images # 查看image
docker rmi  # 删除image
docker tag IMAGEID(镜像id) REPOSITORY:TAG（仓库：标签）
docker import 
docker export 

// container命令
docker ps -a  # 查看container
docker save 
docker export

// 使用 -it 时，则和我们平常操作 console 界面类似，而且也不会像attach方式因为退出，导致整个容器退出 使用 -d 参数，
// 在后台执行一个进程。如果一个命令需要长时间进程，会很快返回
docker run --name test daocloud.io/library/ubuntu   # 创建并启动一个容器 名为 test 使用镜像daocloud.io/library/ubuntu
docker exec -it {{containerName or containerID}} bash
docker exec -d {{containerName or containerID}} bash
```

## Docker Aira
<https://p3terx.com/archives/docker-aria2-pro.html>

```
docker pull p3terx/aria2-pro
```

```
docker run -d \
    --name aria2-pro \
    --restart unless-stopped \
    --log-opt max-size=1m \
    --network host \
    -e PUID=$UID \
    -e PGID=$GID \
    -e RPC_SECRET=<TOKEN> \
    -e RPC_PORT=6800 \
    -e LISTEN_PORT=6888 \
    -v ~/aria2-config:/config \
    -v ~/aria2-downloads:/downloads \
    p3terx/aria2-pro
```


# CZU服务器centos

user: root

passwd: jyb86131

user: eda

passwd: 4444

## 服务器密码

bihui

bihui123456

caichengjie

ccj123456

### todesk

!Jyb86131


## vmware

### VMware Fusion Player 13 – Personal Use

COMPONENT EXPIRATION DATE LICENSE KEYS
VMware Fusion Player 13 – Personal Use  5128K-4ALD1-081Q2-0HC8K-C0UK4
