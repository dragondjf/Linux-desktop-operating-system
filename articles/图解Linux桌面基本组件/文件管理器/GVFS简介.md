GVFS简介
===============
####1. 什么是GVFS

`GVFS`是GNOME桌面系统的虚拟文件系统，通过GVFS，用户可以很容易的通过SFTP、FTP、WebDAV、SMB等访问远程数据，通过HAL integration、OBEX等访问本地数据。(GVFS 是 GNOME-VFS 的替代品, 可以理解为GNOME-VFS已被废弃)

GVFS 包含两个部分：

* GIO，作为 GLib的一部分的新共享库，提供了 针对 GVFS 的 API；
* GVFS 本身，是一个包含多种文件系统和协议（如SFTP, FTP, DAV, SMB 和 ObexFTP）支持的后台软件包。

GVFS致力于提供一个现代的，易用的 VFS 系统。它的目标是提供一些列 API 给开发者，以是他们不再使用原始的 POSIX IO 访问。它提供了一个更高级的以文件为中心的接口，而不仅仅是 POSIX IO 的复制品。除了文件的读写支持外，GIO 还提供了文件监视工具，异步 IO，和文件名完成功能。

GVFS 通过运行一个单独的主守护进程 (gvfsd) 来工作，它保证了对当前的 GVFS 挂载的跟踪。每一个挂在都有独立的守护进程。(一些挂载也会同时共享一个进程，但多数情况下不会这样。) 客户端通过一个联合 D-Bus 会话来与这些挂载通信(在会话总线上，但是使用点对点 D-Bus)，同时用一个用户协议来进操作文件内容。通过进程进行后台传递大大简化了程序的依赖关系，使整个系统更加健壮。

GVFS 也提供了在 `/run/user/1000/gvfs`提供了一个 FUSE 挂在点，这样可以使得 GVFS 挂载可以被传统的使用标准 POSIX IO 的应用程序使用。

不同于 GNOME-VFS，GVFS 中的连接是有状态的。这意味着用户仅仅需要输入一次密码，而不是每次成功的连接都需要一次次地重复输入。

####2. GVFS架构设计图
![GVFS架构图](https://developer.gnome.org/gio/stable/gvfs-overview.png)

####3. 目前支持的后端(GVfs 1.30.0)

| Backend      |    Description | Libraries  |
| :-------- | --------:| :--: |
| admin   | Admin access for local filesystem |  libpolkit, libcap   |
| `afc `    |   AFC protocol for iphone|  libimobiledevice  |
| afp     |   AFP protocol |   |
|afpbrowse|AFP devices on the network||
|archive| Archive files|libarchive|
|`burn`|Burning optical disks||
|`cdda`|Audio disks|libcdio|
|computer|List of devices connected to the PC||
|dav|WebDAV protocol|libsoup, libxml|
|dnssd|DNS-SD devices on the network|libavahi|
|ftp|FTP protocol||
|google|Google Drive|libgdata, libgoa|
|`gphoto2`|PTP protocol|libgphoto2|
|http|HTTP protocol|libsoup|
|localtest|Local filesystem for testing purposes||
|`mtp`|MTP protocol for andriod |libmtp|
|`network`|Devices on the network|libavahi, libsmbclient|
|nfs|NFS v2 and v3 protocols|libnfs|
|recent|Recent used files||
|sftp|SFTP protocol||
|`smb`|SMB/CIFS protocol|libsmbclient|
|`smbbrowse`|SMB devices on the network|libsmbclient|
|test|Filesystem for testing purposes||
|trash|Trash||

可以在/usr/lib/gvfs/目录下看到很多gvfs开头的应用程序

+ `gvfsd-*` 为每个对应协议protocol的后端daemon程序 
+ `gvfs-*-volume-monitor`为每种对应协议的volume监视器,目前支持udisks2、afc、mtp、gphoto2、goa等协议的设备监控
+ `gvfsd-network`网络邻居
+ `gvfsd-smb-browse`为smb://192.168.1.100浏览对应ip地址下所有共享的目录
+ `gvfsd-fuse` 为Fuse提供支持，保证传统使用标准 POSIX IO 的程序无须改动代码即可访问远程设备

####4. 各个协议支持的文件操作(GVfs 1.24.0)

+ 读操作

| **Backend**   |  **open_for_read/read/close_read**| **read**　| **seek_on_read**|**pull**|
| :------ | :------:| :------: |:------:|:------:|
|afc |√**do** |√**do** |√**do** | |
|afp |√**try** |√**try**|√**try**||
|archive |√**do** |√**do**|||
|cdda |√**do** |√**do**|√**do**||
|dav |√**try**|√**try**|√**try**||
|ftp |√**do** |√**do**||√**do**|
|gphoto2 |√**do** |√**try**|√**try**|√**do**|
|http |√**try** |√**try**|√**try**||
|mtp |√**do** |√**do**|√**do**|√**do**|
|nfs |√**try** |√**try**|√**try**||
|sftp |√**try** |√**try**|√**try**||
|smb |√**do** |√**do**|√**do**||

+ 写操作

| **Backend**   |  **create/replace/write/close_write**| **append_to**　| **seek_on_write/truncate** | **delete** | **copy** | **move** | **push** |
| :------  |  :------: | :------:  | :------:  |  :------:  |  :------:  | :-----: |  :------:  |
|afc |√**do** |√**do** |√**do** | √**do**||√**do**||
|afp |√**try** |√**try**|√**try**|√**try**|√**try**|√**try**|
|archive ||||||
|cdda ||||||
|dav |√**try**|||√**do**|||||
|ftp |√**do**|√**do**||√**do**||√**do**||
|gphoto2 |√**do**|√**do**|√**do**|√**do**||√**do**||
|http ||||||
|mtp |√**do**|√**do**|√**do**|√**do**|||√**do**|
|nfs |√**try** |√**try**|√**try**|√**try**||√**try**||
|sftp |√**try** |√**try**|√**try**|√**try**||√**try**|√**try**|
|smb |√**do**|√**do**|√**do**|√**do**||√**do**||
+ 获取Metadata操作

| **Backend**   |  **query_info/enumerate**| **query_fs_info**　| **set_display_name**|**set_attribute**|
| :------ | :------:| :------: |:------:|:------:|:------:|
|afc |√**do** |√**do** |√**do** | √**do**|
|afp |√**try** |√**try**|√**try**|√**try**|
|archive |√**do**|√**try**||||
|cdda |√**do**|√**do**|||
|dav |√**do**|√**do**|√**do**||
|ftp |√**do**||√**do**|√**do**|
|gphoto2 |√**do/try**|√**do/try**|√**do**||
|http |√**do**|||||
|mtp |√**do**|√**do**|√**do**||
|nfs |√**try** |√**try**|√**try**|√**try**|
|sftp |√**try** |√**try**|√**try**|√**try**|
|smb |√**do** |√**do** |√**do** | √**do**|

####5. gvfs-mount等工具
| **cmd**   |  **Description**|  **notes**| 
| :------ | ------:| :------: |
|gvfs-less|Execute less on the output of gvfs-cat||
|gvfs-cat|Concatenate files||
|gvfs-copy|Copy files||
|gvfs-info|Show information about files||
|gvfs-ls|List files||
|gvfs-mime|Get or set mime handlers||
|gvfs-mkdir|Create directories||
|gvfs-monitor-dir|Monitor directories for changes||
|gvfs-monitor-file|Monitor files for changes||
|gvfs-mount|Mounts the locations||
|gvfs-move|Copy files||
|gvfs-open|Open files with the default handler||
|gvfs-rename|Rename a file||
|gvfs-rm|Delete files||
|gvfs-save|Save standard input||
|gvfs-set-attribute|Set file attributes||
|gvfs-trash|Move files or directories to the trash||
|gvfs-tree|List contents of directories in a tree-like format||

####6. nautilus文件管理器的重要bugs

+ 拷贝文件到可移动设备如U盘时，拷贝大文件时(几个G)到99%后需要等待很久，拷贝任务才完成；拷贝过程是将内容写到内存中，并没有实时的flush到设备中去，故而很快到拷贝完成到99%,在任务最后进行flush数据花费大量时间；特别严重的是剪切文件到可移动设备时，如果任务中途拔掉U盘，会出现文件丢失问题。
+ 拷贝文件到远程设备如smb时，不会刷新视图
