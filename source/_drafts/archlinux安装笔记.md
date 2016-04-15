---
title: archlinux安装笔记
date:  2016-04-14 13:23
categories:
    - 笔记
tags:
    - archlinux
---

## 前面

安装的过程应该有正面几个大的步骤:

 1. 下载arch iso
 2. 载入,分区
 3. 设置
 4. 安装系统
 5. 安装bootloader

设置`archlinux.iso`为第一起动,启动完成后会以 root 登录并进入zsh命令提示,使用`vim`编辑文件

**分区类型选择**,当然是GPT了,虽然难了一点,使用`parted`工具来分区

## 安装过程



### 分区

我们使用`parted`手动分区


  - /boot：引导分区
  - swap：交换区
  - /：根分区
  - /home：用户目录区

```
parted /dev/sdx      # 打开一个设置,进行分区
(parted) mklabel gpt # 为UEFI系统创建GPT分区表
(parted) mkpart ESP fat32 1M 513M #创建一个ESP-EFI启动分区
(parted) set 1 boot on #启动目录
(parted) mkpart primary ext4 513M 20.5G  #20G给/
(parted) mkpart primary linux-swap 20.5G 24.5G # 4G swap
(parted) mkpart primary ext4 24.5G 100%      #剩下给 /home

```

 > 更好用的` cfdisk`


格式化文件系统

```
lsblk /dev/sdx          #查看所有系统
mkfs.vfat -F32 /dev/sdxI
mkfs.ext4 /dev/sdxI

mkswap /dev/sdxI
swapon /dev/sdxI
```

挂载分区

```
mount /dev/sdxI  /mnt

mkdir /mnt/home
mount /dev/sdxI /mnt/home

mkdir -p /mnt/boot
mount /dev/sdxI /mnt/boot
```


### 安装系统镜像

从 `/etc/pacman.d/mirrorlist` 中定义的镜像站中下载安装包到本地

```
vim /etc/pacman.d/mirrorlistk
pacman -Syy #强制刷新

pacstrap -i /mnt base base-devel #安装基本包

```



## 引用
 
 - [wiki新手指南](https://wiki.archlinux.org/index.php/Beginners%27_guide_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))
 - [打造完美的archlinux桌面](https://linuxtoy.org/archives/the-perfect-linux-desktop-arch-linux-2007-08-2-1.html)
