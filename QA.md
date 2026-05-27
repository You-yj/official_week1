# 2026 年培训过程中新生遇到的问题与解决方法记录

## 第一阶段

### 工程协作环境搭建

#### 问题 1：安装盘制作出现错误
- 问题现象

<div align="center">
   <img src="https://github.com/You-yj/official_week1/blob/main/typora.%E5%9B%BE%E7%89%87%E6%94%B6%E7%BA%B3/image-20260414205842507.png?raw=true" width="270" />
    <img src="https://github.com/You-yj/official_week1/blob/main/typora.%E5%9B%BE%E7%89%87%E6%94%B6%E7%BA%B3/image-20260414210602928.png?raw=true" width="310" />
</div>

- 排查问题
  由于是找不到路径，那么大概率是【环境变量】中有无效路径

<div align="center">
    <img src="https://github.com/You-yj/official_week1/blob/main/typora.%E5%9B%BE%E7%89%87%E6%94%B6%E7%BA%B3/image-20260414212127441.png?raw=true" width = "600" />
</div>

- 解决方案
  发现确实有路径是无效的，**将该路径删除后，重新制作一遍**，制作成功
#### 问题 2：Windows 磁盘管理工具无法从 D 盘压缩出足够空间 **/** 压缩 D 盘时可用压缩空间远小于可用空间
- 现象：D 盘有 200GB 空闲，可用压缩空间仅 997MB
- 解决：改用**傲梅分区助手**，成功拖出 160GB 空闲空间
#### 问题 3：安装 Ubuntu 系统时报错“The install media checksum verification failed”
- 排查过程：仔细检查后发现，下载的安装包是 Server 版本，而不是 Desktop 版本，前者对 U 盘读写质量、接口稳定性敏感。而且仔细检查后发现使用的 U 盘价格便宜，质量不好，极有可能是 U 盘的原因。
- 解决方法：重新换了一个 U 盘，重新制作系统盘，并且下载 Desktop 版本，最终成功下载。
- 参考资料来源：豆包
#### 问题 4：Ubuntu 安装时卡死报错
- 现象：安装过程中弹出错误提示，无法继续
- 排查过程：检查发现安装程序在后台尝试下载更新，网络连接国外服务器超时
- 解决方法：**关闭 Wi-Fi，断网安装，安装完成后再更新系统**
#### 问题 5：安装 Ubuntu 系统时不会分区，看不懂文件里的各个概念的意思
- 排查过程：慢慢看，稍微有点懂了。然后EFI分区分1.1G，swap分区分16G，/boot分1G，/与/home都分200多G。
- 解决方法：分完之后找学姐确认，学姐还提醒我安装启动引导器选择EFI分区所在设备，为了开机进入 Ubuntu 系统。
- 参考资料来源：学姐
#### 问题 6：双系统重新分配磁盘问题
- 问题现象：前期分配给Ubuntu系统的100G磁盘空间不够
- 排查过程：
    1. 提前在windows中分出未分配空间
    2. 利用启动盘进入到try unbuntu / install ubuntu,选择**try unbuntu**
    3. 然后安装**gparted**：`sudo apt install gparted`
    4. 打开gparted，找到Ubuntu分区（**显示小钥匙，代表挂载**），右键选择**nmount / swopoff**，小钥匙消失
    5. 将未分配空间拖动到Ubuntu分区后，应用更改
    6. 重启系统，执行`df -h`确认 Ubuntu 分区空间增加至 200+G，问题解决
- 参考资料来源：

    [Ubuntu系统根目录空间不足解决办法](https://blog.csdn.net/weixin_37669024/article/details/121673889)

    [Windows + Ubuntu双系统之为重新为ubuntu划分（增加或减小）磁盘空间](https://blog.csdn.net/sinat_41877285/article/details/133656622)

    [【linux基础】小白超详细 Ubuntu安装教程（AI提供）](https://blog.csdn.net/2501_90561511/article/details/159856180)
#### 问题 7：安装 Ubuntu 系统后下载不了微信
- 排查过程：一开始以为是原来的安装包有损，重新在官网上下载，结果一样安装不了。后来询问师姐才明白，Ubuntu 系统下用命令来安装软件，而不是像 Windows 下双击然后安装。
- 解决方法：在豆包上搜索相关命令下载微信，最终成功下载。
- 参考资料来源：豆包
#### 问题 8：安装搜狗输入法时提示依赖错误
- 现象：`sudo dpkg -i`报错缺少 fcitx 依赖包
- 解决：先`sudo apt install fcitx -y`，再安装搜狗输入法，最后`sudo apt install -f`
#### 问题 9：APT缓存锁（无法获得锁 /var/lib/dpkg/lock-frontend）
- 现象：执行 apt 命令时提示锁被占用，等待十分钟无响应
- 排查过程：ps aux | grep apt 查找占用进程，发现系统自动更新服务 unattended-upgrades 在运行
- 解决方法： 
```Python
sudo kill -9 [进程PID]
sudo rm /var/lib/dpkg/lock-frontend /var/lib/dpkg/lock /var/cache/apt/archives/lock
sudo dpkg --configure -a
```
#### 问题 10：微信启动后一直转圈，无法打开
- 现象：点击微信图标后光标转圈，然后无响应
- 排查过程：
    1. 尝试添加 `--no-sandbox` 参数，无效
    2. 使用 `ldd /usr/bin/wechat | grep "not found`" 检查，发现缺失依赖库

- 解决方法：补全依赖库 `libgconf-2-4 libappindicator1 libgtk-3-0 libxss1 libasound2 libnss3 libgbm1`
#### 问题 11：
- 问题现象：创建共享文件夹时要求在虚拟机上下载 WinFsp 安装包，在浏览器上下载后双击安装，走完安装程序但在终端检查发现根本没有安装好。
- 排查过程：怀疑安装包有损，在官网重新下载安装，结果一样安装不了，反复尝试多次确认。
- 解决方法：winget 一键安装，先以管理员身份打开 PowerShell，然后在终端输入
```
winget install --id=WinFsp.WinFsp -e #自动下载最新稳定版、自动安装、自动配置服务、装完不用重启
winget list WinFsp #验证是否成功，能看到版本号就对了
```
#### 问题 12：安装虚拟机相关包时依赖报错
- 问题现象
  1. 执行`sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients virt-managerbridge-utils`（命令漏空格），报错 “无法定位软件包 virt-managerbridge-utils”
  2. 修正命令后又报错 “apt --fix-broken install”，依赖关系损坏
- 排查过程
  1. 检查命令语法：发现`virt-manager`和`bridge-utils`之间缺少空格，是输入错误导致的第一个报错；
     2. 针对依赖报错，询问AI+查阅 APT 报错解决方案，确认`-f`选项可自动修复损坏的依赖，专门处理依赖出错的情况 
- 解决方案
  1. 修正命令：`sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients virt-manager bridge-utils`；
  2. 修复依赖：执行`sudo apt-get -f install`，自动补全缺失的包或移除冲突包，让系统恢复正常状态 
  3. 重新执行正确的安装命令，成功安装所有虚拟机相关包
- **额外收获**
  之前安装的飞书软件没有图标，且消息角标不会消失(即使已经点进软件查看消息了)，但做完上述操作就好了
#### 问题 13：KVM 安装时软件包依赖冲突
- 现象：`sudo apt install qemu-system-x86-hwe` 导致与 libvirt 依赖冲突
- 排查过程：错误信息显示 `ubuntu-virt` 和 `ubuntu-virt-hwe` 冲突
- 解决方法：改用标准版 `qemu-system-x86`，不使用 `-hwe` 版本
#### 问题 14：Ubuntu KVM 虚拟化环境安装报错
- 现象：执行安装命令后系统拒绝执行。报错信息：`Package 'qemu-kvm' has no installation candidate`
- 原因分析： 软件源未更新（次要原因），系统本地的软件包索引是旧的，不知道网络上有哪些最新的软件可以下；载包名变更（主要原因），在较新的 Ubuntu 版本中，qemu-kvm 已经变成了一个“虚拟包”，它不再直接指向一个可执行的安装文件，而是指向具体的实现包（qemu-system-x86）。系统提示需要显式地选择具体的包来安装
- 解决方法：
    1. 更新软件源列表
       ```
       sudo apt update  
       ```
    2. 安装正确的软件包，不再使用报错的 qemu-kvm，而是直接使用系统推荐的qemu-system-x86
       ```
       sudo apt install qemu-system-x86 libvirt-daemon-system virt-manager   
       ```
    3. 配置用户权限，默认情况下，只有 root 用户才能管理虚拟机。这一步将我的当前用户加入到虚拟化用户组，这样就不需要每次都用 sudo 来运行虚拟机了
       ```
       sudo usermod -aG libvirt USER
       sudo usermod -aG kvm USER     
       ```
    4. 使配置生效执行完上述命令后，直接重启电脑。只有这样，刚才加入的用户组权限才会真正生效
#### 问题15：启动 Windows 虚拟机报错 “unable to find a satisfying virtiofs”
- 问题现象：

  在Ubuntu 系统安装 Windows 虚拟机.pdf **/** 创建文件共享 **/** 1.宿主机端配置 (Ubuntu)后,重新运行虚拟机
  报错:`error starting domain: unable to find a satisfying virtiofs`

- **询问AI**，输入指令:virt-manager error starting domain: unable to find a satisfying virtiofs

- 问题原因：virtiofs 是 KVM 虚拟机和宿主机高速共享文件的协议，依赖 virtiofsd 守护进程运行，系统默认不会预装，手动安装即可解决

- 解决方案：在终端输入命令`sudo apt install -y virtiofsd`，再次运行虚拟机，正常运行了
#### 问题16：虚拟机 Virtio-FS Service 报错 1053
- 现象：Windows 虚拟机中 Virtio-FS Service 无法启动，错误代码 1053
- 排查过程：搜索得知该服务依赖 WinFSP 框架
- 解决方法： 
    1. 在手机下载 WinFSP 安装包，通过微信传到宿主机
    2. 通过 U 盘直通传入 Windows 虚拟机
    3. 安装 WinFSP，重启虚拟机，服务正常启动
#### 问题17：virtio-win.iso下载太慢 / GitHub下载速度极慢（10-40KB/s）或无法访问
- 解决方案:尝试换国内镜像源（清华、阿里云）
#### 问题18：Python 虚拟环境安装第三方库报错
- 现象：在激活 Python 虚拟环境之后，尝试运行`pip install`命令安装第三方库时，但是终端提示`Command 'pip' not found`，找不到 pip 命令
- 原因分析：在 Ubuntu (Linux) 系统中，Python 解释器和 pip 包管理工具是分开安装的，我当时只安装了Python，没有安装用于下载库的`pip`工具
- 解决方法：在终端执行安装命令
  ```
  sudo apt install python3-pip
  ```
  确认虚拟环境 (codetest) 处于激活状态，重新安装第三方库
  ```
  python -m pip install numpy scipy sympy matplotlib torch  
  ```
#### 问题19：明明装了第三方库，却提示找不到
- 现象：在终端里运行 Python 代码`import numpy as np`，系统报错`ModuleNotFoundError: No module named 'numpy'`。
- 原因分析：路径不匹配。我一开始安装的第三方库，是在没激活虚拟环境的情况下安装的，所以它们被放到了用户全局目录（User Base）这个“公共大仓库”里，而Python 的虚拟环境机制默认是隔离的，它看不见外面的库。
- 解决方法：在激活虚拟环境状态下，使用 Python 模块方式调用 pip 进行安装。
#### 问题20：vim编辑器模式混淆
- 现象：使用vim编辑note.txt文件后，执行cat note.txt查看，看不到刚写入的内容。
- 原因分析：输入完文本后，没有按Esc键退出插入模式，直接输入的:wq被当作普通文本写入了文件，而没有执行"保存并退出"命令。
- 解决方法：按Esc键退出插入模式，再输入的:wq并按回车键。