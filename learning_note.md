# 一、任务理解

1. 对工程协作环境搭建的理解
   
   - 工程协作环境搭建是为团队或个人的工程开发工作构建一套**统一**、**高效**以及**稳定**的基础运行环境，涵盖操作系统部署、开发工具安装、虚拟环境配置、跨系统协作等核心环节
   - 其核心目标是**消除不同设备**、**系统之间的环境差异**，**降低开发过程中因环境不一致导致的本地运行正常、协作端运行异常问题**，同时通过标准化的环境配置**提升开发效率、简化协作流程**，是工程开发的基础前置工作

2. 为什么工程开发需要 Linux、虚拟环境与命令行
   
   - **Linux 系统**：Linux 具备开源、稳定、安全、可定制性强的特性。相比Windows，Linux对权限管理、进程控制更贴合工程开发需求，写代码开发更加方便
   - **虚拟环境**：工程开发中不同项目对编程语言版本(如 Python 3.8/3.10)、第三方库版本(如 TensorFlow 2.6/2.10)的要求不同，虚拟环境可实现“一个系统、多套隔离环境”，避免不同项目的依赖包冲突，同时便于协作时快速复刻一致的开发环境
   - **命令行**：命令行操作相比图形界面更高效、可自动化、易追溯。工程开发中的批量文件操作、环境配置、软件安装、远程服务器操作等场景，通过命令行可快速完成（如`apt install`安装软件、`conda create`创建虚拟环境）

3. 对后续工程训练环境的初步认识
   
   后续工程训练环境应是一套以Ubuntu为核心的标准化环境：
   
   - 底层基于Ubuntu系统提供稳定的运行基础，通过命令行完成日常开发操作
   - 针对不同训练任务搭建独立的Python虚拟环境，隔离不同任务的依赖
   - 同时可能通过Windows虚拟机解决部分仅支持Windows 的工具 / 软件使用需求

---

# 二、Ubuntu系统安装

## 2.1制作安装盘

- 制作前Ubantu安装盘前的准备
  
  - [Download Ubuntu Desktop | Ubuntu](https://ubuntu.com/download/desktop)
  - 下载[Rufus](https://rufus.ie/zh/)(用于制作Ubantu安装盘，Windows专用)

- 制作Ubuntu安装U盘
  
  - 插入U盘
    
    > - 确认U盘无重要数据，**制作会彻底清空**
    > - 插入 “8GB+、空白”U盘 到Windows电脑USB 
  
  - 运行Rufus
    
    - 打开 `rufus-x.x.exe`，弹出用户账户控制，点**是**
    
    - 生成一个界面，按如下步骤选：
      
      1. 设备选择插入的U盘
      
      2. 引导类型选择下载的ubuntu-24.04.4-desktop-amd64
      
      3. 分区类型
         
         <kbd>win</kbd>+<kbd>R</kbd>，输入`diskmgmt.msc`
         
         <div align="center">
             <img src="https://github.com/You-yj/official_week1/blob/main/typora.%E5%9B%BE%E7%89%87%E6%94%B6%E7%BA%B3/image-20260414205323635.png?raw=true" width="666" />
         </div>
         
         
         
         故选**GPT**类型
         
      4. 其余选项默认即可，最终得到页面设置如下
         
         <div align="center">
             <img src="https://github.com/You-yj/official_week1/blob/main/typora.%E5%9B%BE%E7%89%87%E6%94%B6%E7%BA%B3/image-20260414205544349.png?raw=true" width = "325" />
         </div>
      
    - 点击 **开始**，确认，确认
      
      此阶段我遇到一个问题，根据问题提示得到解决，具体见[问题一：安装盘制作出现错误](##问题一：安装盘制作出现错误)
    
    - 等待进度条走完（绿色满格，显示**准备就绪**）
      
      <div align="center">
          <img src="https://github.com/You-yj/official_week1/blob/main/typora.%E5%9B%BE%E7%89%87%E6%94%B6%E7%BA%B3/image-20260414213326470.png?raw=true" width = "320" />
          <img src="https://github.com/You-yj/official_week1/blob/main/typora.%E5%9B%BE%E7%89%87%E6%94%B6%E7%BA%B3/image-20260414213513906.png?raw=true" width = "320" />
      </div>
      
      完成后，点**关闭**，**安全弹出U盘**（别直接拔！）

## 2.2安装系统前的设置

### 查看/更改引导方式

<kbd>Win</kbd>+<kbd>R</kbd> 呼出运行界面，输入：`msinfo32`。进入界面后，在项目栏找到**BIOS模式**，看对应的值，**是否为UEFI**，如果是就OK！

### 查看/更改磁盘的分区格式

磁盘分区形式，是否为**GPT格式**，如果是就OK！

### 关闭BitLocker加密

这一步是为了避免装双系统过程中进入烦人的BitLocker恢复模式

- 如果在上面的操作中打开磁盘管理后，C盘D盘没有写着：(BitLocker 已加密)这些字就OK！

- 若写了，这大概和下图一样:

<div align="center">
    <img src="https://github.com/You-yj/official_week1/blob/main/typora.%E5%9B%BE%E7%89%87%E6%94%B6%E7%BA%B3/image-20260415155742826.png?raw=true" width = "666" />
</div>


此时需要:在【设置】-->【隐私和安全性】-->【设备加密】，把设备加密的开关关闭就好。(不过，磁盘已存储的数据越多，关闭设备加密耗时越长，如果电脑里面东西比较多的话最好合理安排一段空闲时间并接通电源再进行哦！)

--关闭后再进入磁盘管理就会发现没有 (BitLocker 已加密) 这行信息了。

<div align="center">
    <img src="https://github.com/You-yj/official_week1/blob/main/typora.%E5%9B%BE%E7%89%87%E6%94%B6%E7%BA%B3/image-20260415162958982.png?raw=true" width = "666" />
</div>


### 压缩分盘

若是安装 Window、Linux-Ubuntu 双系统，则首先需要在 Windows 系统下预先分出安装 Linux-Ubuntu 的空间

右键单击【**此电脑**】(-->【**显示更多选项**】) -->【**管理**】-->【**磁盘管理**】

右键点击要分出空间的卷，点击压缩卷，输入要压缩的空间，此空间即为为Linux-Ubuntu分出的空间，建议至少分**150G**以上

推荐一款比较好用的软件--**傲梅分区助手**，非常简单操作

<div align="center">
    <img src="https://github.com/You-yj/official_week1/blob/main/typora.%E5%9B%BE%E7%89%87%E6%94%B6%E7%BA%B3/image-20260417142557805.png?raw=true" width = "666" />
</div>


## 2.3正式安装ubuntu系统

- 关机插入启动盘

- 开机进启动菜单（快捷键）
  
  开机瞬间，狂按主板对应的快捷键：我是按<kbd>Enter</kbd>

- 选择U盘启动
  
  弹出启动菜单，选带USB/U盘名称/UEFI: U盘名的选项
  按回车，电脑从U盘启动，进入Ubuntu安装界面

- 后面按照提示即可

**结果：**

<div align="center">
    <img src="https://github.com/You-yj/official_week1/blob/main/typora.%E5%9B%BE%E7%89%87%E6%94%B6%E7%BA%B3/135bce3f2f322d2380fed9975b7bade1.png?raw=true" width = "666" />
</div>
---

# 三、linux基础命令

## 文件与文件夹基础命令

| 命令    | 英文全称                    | 作用说明              | 常用用法                                              | 重要注意         |
| ----- | ----------------------- | ----------------- | ------------------------------------------------- | ------------ |
| pwd   | print working directory | 打印当前工作目录，显示所在文件夹  | `pwd`                                             | —            |
| cd    | change directory        | 切换目录              | `cd /etc` `cd ..` `cd ~`                          | 后面必须加空格      |
| ls    | list                    | 查看目录文件            | `ls` `ls  -a` `ls /etc`                           | -a 显示隐藏文件    |
| find  | find                    | 按文件名、路径、大小、时间搜索文件 | `find 路径 -name 文件名` `find / -size +1000M`         | 权限不足加 sudo   |
| mkdir | make directory          | 创建文件夹             | `mkdir 文件夹名` `mkdir 名 1 名 2 名 3`  `mkdir -p 多层目录` | 禁用 * ? / 等符号 |
| touch | —                       | 创建空文件             | `touch 文件名 + 后缀` `touch 名 1 名 2`                  | 文件名与后缀用.连接   |
| rm    | remove                  | 删除文件 / 文件夹        | `rm 文件名` / `rm -r 文件夹名`                           | 直接删除，无法撤销    |
| cp    | copy                    | 复制文件 / 文件夹        | `cp 文件 路径` / `cp -r 文件夹 路径`                       | 文件夹需加 -r     |
| mv    | move                    | 移动 / 重命名          | `mv 源 目标` / `mv 旧名 新名`                            | 文件夹不用 -r     |
| cat   | concatenate             | 查看 / 合并文件         | `cat 文件名` / `cat 文件名1 文件名2 > 文件名3`                | 适合内容较少文件     |

## 系统基础命令

- **df** (disk free) 查询磁盘空闲空间
  
  就是看看硬盘里面每个分区有多大，用了多少，还剩多少
  df   (出来的结果全是字节，即一长串数字)
  df -h    (人类看得懂的，会给出k M G)

- **sudo** (super user do) 管理员执行某个操作，临时成为管理员，办完事就收回
  
  sudo 命令行  :以管理员身份执行命令行
  常用:
  
  1. 安装软件: sudo apt install 软件名
  2. 修改系统文件: sudo gedit /etc/hosts   (/etc/hosts 为Ubantu主机文件)
     在网页上下载了软件的deb包，可使用如下命令进行安装：sudo dpkg -i deb包全称   (注意要先cd到下相应文件夹中)

- **apt** (Ubantu里面的超级应用商店，可装软件 升系统 清垃圾)
  
  1. 更新商店库存(以免装到旧版本): sudo apt update
  2. 装软件: sudo apt install 软件名
  3. 删除软件: sudo apt remove 软件名
  4. 升级所有软件: sudo apt upgrade

- **kill** 结束程序进程(杀进程)
  
  1. kill PID :杀掉ID为100的进程：kill 100   (默认这个程序自动退出，有时候会未响应)
  2. kill -9 PID :让这个程序立即终止

---

# 四、常用工程软件安装

利用[系统基础命令](##系统基础命令)，就可以下载绝大多数工程软件

<div align="center">
    <img src="https://github.com/You-yj/official_week1/blob/main/typora.%E5%9B%BE%E7%89%87%E6%94%B6%E7%BA%B3/image-20260522160848580.png?raw=true" width = "666" />
</div>

<div align="center">
    <img src="https://github.com/You-yj/official_week1/blob/main/typora.%E5%9B%BE%E7%89%87%E6%94%B6%E7%BA%B3/Screenshot%20from%202026-05-22%2016-21-32.png?raw=true" width = "666" />
</div>
主要包括：vscode、QQ、飞书、Typora、Vim、Git、Firefox、watt toolkit等

---

# 五、Vim 编辑器基础使用

- vim 的基本模式
  
  - 一般模式 ：一打开文件的时候所处模式，可以进行删除、复制、黏贴等操作
  - 编辑模式 ：可以编辑内容，按<kbd>ｉ</kbd>进入编辑模式，<kbd>Esc</kbd> 进入一般模式
  - 命令行模式：进行数据查找，大量字符替换等操作，按”/””?””:” 进入

- 文件创建与编辑
  
  1. `touch 文件名`
  2. `vim 文件名`
  3. 按<kbd>ｉ</kbd>进入编辑模式，编辑文件

- 保存与退出操作

编辑好文件后，按<kbd>Esc</kbd> 进入一般模式，输入`:wq`保存推出
`:w`保存；`:q`退出

---

# 六、Python 虚拟环境搭建

## Anaconda下载

之前是在windows系统中使用pycharm编辑器，Anaconda进行环境搭建，现在需要在ubuntu系统下使用vscode编辑器，Anaconda进行环境搭建

- 在网页上搜索anaconda或根据网址[Anaconda下载官网](https://www.anaconda.com/download) 下载得到Anaconda3-2025.12-2-Linux-x86_64.sh文件

- 下载完后打开终端，`cd Downloads` 进入Downloads文件中，输入以下命令运行安装脚本(将Anaconda3-2025.12-2-Linux-x86_64.sh文件替换为所下载的文件名)：`bash Anaconda3-2025.12-2-Linux-x86_64.sh`

- 按照提示操作：按照提示阅读许可协议，输入yes同意；选择安装位置，默认路径通常为～/anaconda

- 安装完后，会询问是否将Anaconda添加到PATH中，输入yes

- 验证安装：打开终端，输入命令`conda --version`

<div align="center">
    <img src="https://github.com/You-yj/official_week1/blob/main/typora.%E5%9B%BE%E7%89%87%E6%94%B6%E7%BA%B3/Screenshot%20from%202026-05-22%2016-31-33.png?raw=true" width = "666" />
</div>
## 环境管理与包安装

操作与windows系统中的操作没有区别

- **创建环境**‌：使用 `conda create -n 环境名 python=版本号` 命令创建隔离环境，例如 `conda create -n py_base python=3.9` 
- ‌**激活与退出**‌：输入 `conda activate 环境名` 进入环境，使用 `conda deactivate` 退出 
- ‌**查看与删除**‌：`conda env list` 查看所有环境，`conda remove -n 环境名 --all` 删除环境 

<div align="center">
    <img src="https://github.com/You-yj/official_week1/blob/main/typora.%E5%9B%BE%E7%89%87%E6%94%B6%E7%BA%B3/Screenshot%20from%202026-05-22%2016-34-19.png?raw=true" width = "666" />
</div>
- ‌**安装包**‌：在激活的环境下，用 `conda install 包名` 或 `pip install 包名` 安装所需库
- **指定查看**该环境下安装的包：输入命令`conda list | grep package_name`，将`package_name`替换为想要查找的包名。

<div align="center">
    <img src="https://github.com/You-yj/official_week1/blob/main/typora.%E5%9B%BE%E7%89%87%E6%94%B6%E7%BA%B3/image-20260522165234958.png?raw=true" width = "666" />
</div>
---

# 七、Windows 虚拟机安装与使用

根据群里所给的安装流程，部分问题与解决见[安装windows虚拟机遇到问题](##安装虚windows拟机遇到问题)，安装与使用结果如下：

1. 启动虚拟机命令：`sudo virt-manager`
2. 运行虚拟机

<div align="center">
    <img src="https://github.com/You-yj/official_week1/blob/main/typora.%E5%9B%BE%E7%89%87%E6%94%B6%E7%BA%B3/image-20260522171604081.png?raw=true" width = "325" />
</div>
3. 主机-虚拟机共享文件

<div align="center">
    <img src="https://github.com/You-yj/official_week1/blob/main/typora.%E5%9B%BE%E7%89%87%E6%94%B6%E7%BA%B3/image-20260522170747652.png?raw=true" width = "666" />
</div>
---

# 八、问题记录与解决过程

## 问题一：安装盘制作出现错误

- 问题现象

<div align="center">
   <img src="https://github.com/You-yj/official_week1/blob/main/typora.%E5%9B%BE%E7%89%87%E6%94%B6%E7%BA%B3/image-20260414205842507.png?raw=true" width="320" />
    <img src="https://github.com/You-yj/official_week1/blob/main/typora.%E5%9B%BE%E7%89%87%E6%94%B6%E7%BA%B3/image-20260414210602928.png?raw=true" width="335" />
</div>


- 排查问题
  
  由于是找不到路径，那么大概率是【环境变量】中有无效路径

<div align="center">
    <img src="https://github.com/You-yj/official_week1/blob/main/typora.%E5%9B%BE%E7%89%87%E6%94%B6%E7%BA%B3/image-20260414212127441.png?raw=true" width = "666" />
</div>
- 解决方案
  
  发现确实有路径是无效的，**将该路径删除后，重新制作一遍**，制作成功，见[2.1制作安装盘](##2.1制作安装盘)

## 问题二：双系统重新分配磁盘问题

- 问题现象：前期分配给Ubuntu系统的100G磁盘空间不够

- 解决方案：
  
  1. 提前在windows中分出未分配空间
  2. 利用[二、Ubuntu系统安装](#二、Ubuntu系统安装)进入到try unbuntu / install ubuntu,选择**try unbuntu**
  3. 然后安装**gparted**：`sudo apt install gparted`
  4. 打开gparted，找到Ubuntu分区（**显示小钥匙，代表挂载**），右键选择**nmount / swopoff**，小钥匙消失
  5. 将未分配空间拖动到Ubuntu分区后，应用更改
  6. 重启系统，执行`df -h`确认 Ubuntu 分区空间增加至 200+G，问题解决

- 参考资料来源
  
  [Ubuntu系统根目录空间不足解决办法](https://blog.csdn.net/weixin_37669024/article/details/121673889)
  
  [Windows + Ubuntu双系统之为重新为ubuntu划分（增加或减小）磁盘空间](https://blog.csdn.net/sinat_41877285/article/details/133656622)
  
  [【linux基础】小白超详细 Ubuntu安装教程（AI提供）](https://blog.csdn.net/2501_90561511/article/details/159856180)

## 安装虚windows拟机遇到问题

### 问题三：安装虚拟机相关包时依赖报错

在==Ubuntu 系统安装 Windows 虚拟机.pdf**/**安装虚拟机软件**/**2.安装QEMU + KVM相关包时==直接复制命令，执行有问题：

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

### 问题四：启动 Windows 虚拟机报错 “unable to find a satisfying virtiofs”

- 问题现象：
  
  在==Ubuntu 系统安装 Windows 虚拟机.pdf**/**创建文件共享**/**1.宿主机端配置 (Ubuntu)==后,重新运行虚拟机
  报错:`error starting domain: unable to find a satisfying virtiofs`

- **询问AI**，输入指令:virt-manager error starting domain: unable to find a satisfying virtiofs

- 问题原因：virtiofs 是 KVM 虚拟机和宿主机高速共享文件的协议，依赖 virtiofsd 守护进程运行，系统默认不会预装，手动安装即可解决

- 解决方案：在终端输入命令`sudo apt install -y virtiofsd`，再次运行虚拟机，正常运行了

### 问题五：virtio-win.iso下载太慢

- 问题现象   
  
  ==Ubuntu 系统安装 Windows 虚拟机.pdf==所给virtio-win.iso下载地址下载太慢，下载速度仅 20KB/s，预估耗时超 10 小时

- 排查过程
  
  1. 尝试更换网络，下载速度无明显提升；
  2. 搜索“virtio-win.iso 网盘下载”，找到可信的网盘资源。

- 解决方案:
  
  通过网盘资源下载virtio-win.iso，下载速度提升至10MB/s，3分钟内完成下载，正常配置虚拟机驱动。

---

# 九、学习总结与个人感受

1. 自己对 Linux 与命令行的理解变化
   
   - 安装Linux系统、使用命令行操作之前
     
     - 对windows、Linux、macos这些系统还没有概念，我甚至都不知道有Linux系统
     - 之前在windows上配置Python环境时使用过一点命令行操作，感觉一般，因为一直接触的是图形化界面，习惯了windows系统万物皆可用**鼠标**的操作
   
   - 目前的理解
     
     - 与熟悉的Windows图形化界面不同，Linux依赖命令行完成绝大多数操作。
     
     - Unbuntu系统的界面相比于windows系统更加干净、简洁，更符合我的审美，感觉运行软件的速度都快一点
     
     - 使用了Ubuntu系统一段时间后，认为命令行操作更有逻辑性、系统性，对自己的文件位置、内容、处理有更好的认知和处理习惯
     
     - 感觉自己对于电脑的语言更加了解了

2. 哪些内容最困难
   
   最困难的是 Ubuntu 与 Windows 双系统的磁盘分区调整。一方面，不了解 “挂载的分区无法修改” 的特性，前期直接在 Ubuntu 系统内尝试调整分区失败；另一方面，`gparted`工具的“解锁（unmount）”操作涉及权限、分区格式等细节，参考的别人的操作又跟自己的情况不一样，多次操作后才找到正确步骤，过程中担心误操作导致数据丢失，心理压力较大

3. 最有收获的内容
   
   Windows 虚拟机的配置则让我掌握了“跨系统协作”的方法，既保留 Ubuntu 的开发优势，又能兼容仅支持 Windows 的工具，形成了完整的开发环境闭环。

4. 仍不理解的问题
   
   - 虽能按教程操作，但未完全理解背后的设计逻辑与底层逻辑与原理
   - [问题一：双系统重新分配磁盘问题](##问题一：双系统重新分配磁盘问题)我看别人重新分配后又右键那个磁盘->swop后将小钥匙复原，但我的选择不了，不过还是分配好了，不知道有什么弊端

5. 对后续工程训练的预期
   
   后续工程训练中，希望能将本次搭建的环境落地使用
