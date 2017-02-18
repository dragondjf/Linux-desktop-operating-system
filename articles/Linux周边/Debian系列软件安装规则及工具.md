Debian系列软件安装规则及工具
==========================

#####1.概述
要想使用Debian系统在线安装软件，首先需要配置source.list文件。source.lst文件位于"/etc/apt"目录下，可以使用编辑器去编辑此文件。

![sources.list](../images/sourceslist.png)


#####2.关于 Debian 源的一些简单说明

+ 以**`水平的观点`**来看 Debian 的源：stable、lenny、unstable  stable, lenny, unstable, experimental 的源算是对 Debian 软件包的一种水平划分。其实也可看成为稳定性不同的发行版本。 通常我们还会以开发代号来称呼它们，目前的 stable 的开发代号是 sagre， lenny 的开发代号是 lenny，而 unstable 的开发代号一直是 sid。  
    +  **`stable源`**  就如同字面意思一样，是最稳定的源，但相对的各个软件则通常不是最新版， 一般情况下没有出现什么安全问题是不会更新的，所安装软件较少也较为固定。  如果是搭建服务器的话，一般都采用 stable 的源。  
    
            deb http://debian.cn99.com/debian/ stable main contrib non-free 
            deb-src http://debian.cn99.com/debian/ stable main contrib non-free  
            deb http://debian.cn99.com/debian-non-US stable/non-US main contrib non-free 
            deb-src http://debian.cn99.com/debian-non-US stable/non-US main contrib non-free

    +   **`lenny 源`**  虽名为测试版，实则已经相当接近于 stable 版本的程度，这个版本的软件多半是在 unstable 中经由维护、开发人员不断的测试之后流入，所以在某种程度来说，其实已经做过初步的检测，这里头的软件大多也是相当稳定的，而且软件也都会比 stable 里头的新，而且软件总量来说则比上 stable 要多很多。 大多数人一般都使用的都是 lenny 的源。

            deb http://debian.cn99.com/debian/ lenny main contrib non-free
            deb-src http://debian.cn99.com/debian/ lenny main contrib non-free
            deb http://debian.cn99.com/debian-non-US lenny/non-US main contrib non-free
            deb-src http://debian.cn99.com/debian-non-US lenny/non-US main contrib non-free   

    +   **`unstable 源`**  个人看法这才算是 测试版 ，这里头最大的特色就是软件更新速度快，几乎都与该软件同步，因为太新相对的使用 unstable 的人也必须承担更高的风险，有时候您可能会遭遇到一早更新完所有软件后，发现有些软件不能正常运作的状况，不过庆幸的是这种情形大概只会持续一两天左右， 因为 unstable 的特色就是更新速度快，一旦有人回报问题，维护的人很快就会作修正。 如果喜欢玩软件，也不在乎有时候系统有出现一些小毛病，那就用它吧! 

            deb http://debian.cn99.com/debian/ unstable main contrib non-free 
            deb-src http://debian.cn99.com/debian/ unstable main contrib non-free  
            deb http://debian.cn99.com/debian-non-US unstable /non-US main contrib non-free 
            deb-src http://debian.cn99.com/debian-non-US unstable /non-US main contrib non-free   

    +  **`experimental源`**  按照官方的说法，里面的软件大多都是很不稳定和充满bug的，并可能导致数据的丢失…. 如果想用到最新的软件并充满小白鼠的献身精神或者是立志成为Bug Reporter..那就用它吧… 
    
            deb http://debian.cn99.com/debian/ experimental main contrib non-free

   + **` backports源`**  为 Debian 提供不需要非 Stable 链接库就可在 Stable 版运行的新软件包，有效地弥补了Debian Stable版软件较旧的缺点。属于稳定性和功能之间的一个新的平衡点吧。  而且，这是 Lonecat 大大目前使用的源，还想什么，就选它吧。  

            deb http://debian.cn99.com/mirror/debian-backports sarge-backports main non-free contrib 
            deb http://debian.cn99.com/mirror/debian-bit stable main non-free contrib 
            deb http://debian.cn99.com/mirror/debian-marillat stable main  

    + **`debian-uo`**，uo 是 Unofficial 的简写，也就是非官方的软件库。  
            
            deb http://debian.cn99.com/debian-uo sid marillat rareware misc ustc java firefly jrfonseca xorg 
            deb-src http://debian.cn99.com/debian-uo sid marillat rareware misc ustc java firefly jrfonseca xorg 


+ 以**`垂直`**的观点来看 Debian 软件的分布：`main`、`contrib`、`non-free`、`non-us` 而其中出现的 `main contrib non-free` 之类的，理解为一种垂直的划分吧。  因为 Debian 是非营利组织，但是组织架构严谨，有一套完善的软件管理方式。基于其对软件 free 度的一种坚持，对不同版权软件包的录入有一些限定。 下面是对它们的一些简要介绍：  


    | 版本               |           简介       |
    | :-------------------|------------------------:| 
    | **main** |    Debian 里最基本及主要且符合自由软件规范的软件 ( packages )。|  contrib     这里头软件虽然可以在 Debian ( non-free ) 软件。 
    | **non-free**   |不属于自由软件范畴的软件。 | 
    | **non-us**    | 这个分类里头的软件都来自非美国地区，当中可能有牵扯到专利、加密..等等问题。|
    |**marillat** |   对应 Christian Marillat 的软件仓库，包括mplayer, transcode等。  |
    |**rareware** |  对应 rarewares.org 的软件仓库, 包括很多音效程序，如lame, musepack, beep media player等。 | 
    | **ustc**   |   对应 debian@ustc 维护的一些软件包，如 mule-gbk, gaim-openq, scim, stardict dicts, patched xpdf, irssi, xmms。| 
    |  **java**  |     对应 Blackdown java。包括 j2re, j2sdk ,mozilla java plugin。| 
    |   **firefly** |     对应打过firefly补丁的包，包括 fontconfig mozilla mozilla-firefox pango1.0 qt-x11-free xft |
    |  **misc**  |    对应其它无分类的包，包括 nvidia-kernel, winex3, rox, chmsee等  debian-bit Lonecat 大大自己编译的一些软件包都在这里。|


#####3.选择最佳镜像发布站点加入source.list文件：netselect，netselect-apt
    sudo apt-get install netselect
    sudo apt-get install netselect-apt
    sudo netselect-apt

#####4.apt系列命令用法

|         功能         | 具体语句            | 
| :-------------------|------------------------:| 
| 软件源设置           | **`/etc/apt/sources.list`** |  
| 更新软件源数据       |   **`sudo apt-get update`** | 
| 更新已安装软件       |    **`sudo apt-get upgrade`** |
| 更换系统版本       |    **`sudo apt-get dist-upgrade`** |
|通过安装包或卸载包来修复依赖错误       |    **`sudo apt-get -f install`** |
| 搜索软件源数据       |    **`sudo apt-cache search foo`** |
| 解压安装软件包       |    **`sudo apt-get install foo`** |
| 重新安装软件包       |    **`sudo apt-get --reinstall install foo`** |
| 删除软件包释放的内容       |    **`sudo apt-get remove foo`** |
| 卸载软件，同时清除该软件配置文件| **`sudo apt-get --purge remove foo`** |
| 删除不需要的包       |    **`sudo apt-get autoclean`** |
| 删除所有已下载的包       |    **`sudo apt-get clean`** |
| 自动安装编译一软件所需要的包       |    **`sudo apt-get build-dep foo`** |


#####5.dpkg系列命名用法
|         功能         | 具体语句            | 
| :-------------------|------------------------:| 
| 显示DEB包信息          | **`dpkg -I xx.deb`** |  
| 显示DEB包文件列表      |   **`dpkg -c xx.deb`** | 
| 安装DEB包             |    **`sudo dpkg -i xx.deb`** |
| 安装DEB包（指定根目录） |  **`sudo dpkg --root=<directory> -i xx.deb`** |
|显示所有已安装软件       |    **`dpkg -l`** |
| 显示已安装包信息       |    **`dpkg -s foo`** |
| 显示已安装包文件列表    |    **`dpkg -L foo`** |
| 卸载包                |    **`dpkg -r foo`** |
| 卸载软件包并删除其配置文件|    **`dpkg -P foo`** |
| 重新配置已安装程序      |    **`dpkg-reconfigure foo`** |


#####6.使用查找软件包名称

+ 根据某个“.h”头文件，查找提供该文件的软件包

        【执行命令】dpkg -S stdio.h
        【输出：】
                libstdc++-4.9-dev:amd64: /usr/include/c++/4.9/tr1/stdio.h
                libc6-dev:amd64: /usr/include/x86_64-linux-gnu/bits/stdio.h
                libglib2.0-dev: /usr/include/glib-2.0/glib/gstdio.h
                libstdc++-5-dev:amd64: /usr/include/c++/5/tr1/stdio.h
                libc6-dev:amd64: /usr/include/stdio.h
        
        【执行命令】dpkg -S /usr/include/stdio.h
        【输出：】
                libc6-dev:amd64: /usr/include/stdio.h

+  使用dpkg 通过关键字查询已安装的

        【执行命令】dpkg -l | grep google
        【输出：】
                ii  google-chrome-stable   47.0.2526.106-1  amd64   The web browser from Google

+ 使用apt-cache search 查询软件包
    
        【执行命令】apt-cache search dde-desktop
        【输出：】
                dde-desktop - deepin desktop-environment - desktop module

####7.apt相关的关键路径说明

|路径|说明|
| :-------------------|------------------------:| 
|`/etc/apt/sources.list` | debian源地址存放目录|
|`/var/lib/apt/lists/` |  `sudo apt-get update`后所有软件更新记录存放目录|
|`/var/cache/apt/archives`|`sudo apt-get upgrade`后所有软件包下载缓存的目录|

#####参考文章
+ http://wenku.baidu.com/link?url=WJ8TymOgvQjEFM_jdBKKB4ma7q-E6yTxF4NASxpt0QEPdqU-LoKue1KFSeOUHBt9Qm42OZLio6VblIm-6ppweVImaxDXWXDS0a-O8fB302y
+ http://jingyan.baidu.com/article/851fbc37c22b233e1f15ab12.html
+ https://www.debian.org/doc/manuals/apt-howto/index.zh-cn.html#contents