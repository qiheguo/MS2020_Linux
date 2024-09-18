# CentOS安装Materials studio

[TOC]

## 安装yum源（如果yum源失效）

参考方法：【CentOS 7安装配置阿里云yum源】 https://www.bilibili.com/video/BV1euMseUEZa/?share_source=copy_web&vd_source=d4a284ec36521608a5ed2b50d95a867c

#### 1.使用root切换到指定文件

```
cd /etc/yum.repos.d
```

#### 2. 备份原文件

```
 mkdir yum.repos.d.backup
 mv *.repo yum.repos.d.backup/
```

#### 3.安装镜像库

```
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```

#### 4.清除缓存，载入新缓存

```
yum clean all
yum makecache
```

## 安装依赖库

安装依赖库方法：

```Linux
yum install -y glibc* compat* libgcc* libstdc* redhatcsh* gcc gcc-c++ gcc-gfortran
```

安装依赖库需要root权限

## 关闭防火墙和SELINUX

```
systemctl disable firewalld
systemctl stop firewalld
sed -i 's/^SELINUX=.*$/SELINUX=disabled/' /etc/selinux/config
setenforce 0
```

## 上传安装包和许可文件

拖拽上传即可，不需要 root权限

安装过程不需要root

## 解压

退出root.快捷键ctrl+d

切换到安装包文件夹之后解压文件

```
[stuHc@node01 MaterialsStudio2020]$ tar -xvf MS2020.tar
```

## 更改破解文件

解压完成之后输入代码破解文件

```
[stuHc@node01 MaterialsStudio2020]$ chmod 777 -R MaterialsStudio2020/
```

## 运行解压文件中install文件进行安装

切换目录

```
[stuHc@node01 ms2020]$ cd MaterialsStudio2020/
```

运行文件：

```
[stuHc@node01 MaterialsStudio2020]$ ./install
```

安装过程中一路全部默认，一直按回车即可

<img src="E:\note\z\image-20240918112622133.png" alt="image-20240918112622133" style="zoom:50%;" />

出现上面内容代表文件正在安装，注意，不推荐使用Root用户安装。

出现以下面内容代表安装成功，选择99退出安装

<img src="E:\note\z\image-20240918112912340.png" alt="image-20240918112912340" style="zoom:50%;" />

**查看网关号码**，安装过程中会弹出网关号

<img src="E:\note\z\image-20240918114948865.png" alt="image-20240918114948865" style="zoom:50%;" />

记住这个网管号码“18888”，一般情况下默认即是18888

## 修改许可文件

#### 1.查看主机名称

```
[stuHc@node01 MaterialsStudio2020]$ hostname
```

#### 2.修改上传的文件

```
%切换到上一级文件
[stuHc@node01 MaterialsStudio2020]$ cd ..
%编辑msi.lic
[stuHc@node01 ms2020]$ vim msi.lic
```

打开之后按i进入编辑模式，将下图中红框位置切换为刚刚查到的hostname。修改完成之后按Esc退出编辑模式，再按shift+:，输入wq保存并推出。

<img src="E:\note\z\image-20240918113542576.png" alt="image-20240918113542576" style="zoom:50%;" />

#### 3.移动修改之后的文件到指定目录

将修改之后的文件复制到安装文件中，如果不是以root用户安装的，安装文件位置一般都在用户文件下的BIOVIA文件。首先复制刚刚修改的文件路径。

<img src="E:\note\z\image-20240918114413393.png" alt="image-20240918114413393" style="zoom:50%;" />

```
%%切换到安装文件中的/BIOVIA/BIOVIA_LicensePack/Licenses/文件路径，具体路径自己找
[stuHc@node01 ms2020]$ cd "/home/stuHc/BIOVIA/BIOVIA_LicensePack/Licenses/"
%%复制文件到指定文件夹
[stuHc@node01 Licenses]$ cp "/home/stuHc/ms2020/msi.lic" .
%%查看是否复制成功
[stuHc@node01 Licenses]$ ll
%%如果列表中有msi.lic文件即复制成功
```

到此安装成功，使用Windows上MS软件测试网关

## 测试网关

#### 创建网关

这里我以我自己电脑链接为例子

<img src="E:\note\z\image-20240914205936344.png" alt="image-20240914205936344" style="zoom: 33%;" />

点开之后创建新网关:

<img src="E:\note\z\image-20240914210005356.png" alt="image-20240914210005356" style="zoom: 33%;" />

输入IP地址, 下面的Port Number对于谢宜龙老师工作站Node07 IP: 172.24.71.79是18888,其他工作站有可能不一样,后面我再整理更新

<img src="E:\note\z\image-20240914210130275.png" alt="image-20240914210130275" style="zoom:50%;" />

#### 测试

<img src="E:\note\z\image-20240914210443848.png" alt="image-20240914210443848" style="zoom: 33%;" />

#### 运行

在配置完成参数之后, 选择Job Control,选择刚刚添加到网关,配置核心数目,最后运算,运算结果会自动下载到本机电脑上.

<img src="E:\note\z\image-20240914210545425.png" alt="image-20240914210545425" style="zoom:50%;" />

## 设置开机自启动

#### 找到gatway文件

```
cd /home/stuHc/BIOVIA/MaterialsStudio20.1/etc/Gateway
```

#### 设置自启动

需要root权限

```
//复制文件到目录
cp msgateway_control_18888 /etc/rc.d/init.d/
chkconfig --add msgateway_control_18888
//重启一下查看状态是否正常
```

