---
layout: post
title:  "windows 与 ubuntu 双系统 grub rescue 故障修复"
date:   2018-01-27 17:03:25 +0800
categories: ubuntu grub 
---

## 具体使用场景

系统环境：

- windows 10、ubuntu 16.04双系统
- grub 2 引导

故障现象：

- windows更新造成，开机无法引导出现`grub rescue`

## 修复方法
### live cd 修复方法
准备Ubuntu live cd，可以在windows下利用 *usb universial installer*与空优盘制作 ubuntu live 启动优盘

试用ubuntu，在终端中输入如下指令:

```
sudo i # 切换到root模式
sudo fdisk -l # 列出所有硬盘分区，在没有手动配置boot分区时，一般ex*为boot所在分区
mount /dev/sda_ /mnt # 挂在boot所在的分区
grub-install --root-directory=/mnt /dev/sda_ # 安装grub引导
update-grub # 更新 grub 引导
```
### grub rescue 修复方法

- 使用 ls 命令找到ubuntu分区，ls 会列出所有分区，如`(hd0,msdos1)，(hd0,msdos2)`，其中 hd0 中的 0 代表第 1 块硬盘（硬盘号从 0 开始），msdos1 中的 1 代表第 1 个分区
- ls (hd0,msdos1) 可以列出具体分区的格式，这样可以找到grub所在的分区
- 执行以下命令：
```
set root=(hd0,msdos5)
set prefix=(hd0,msdos5)/boot/grub
insmod normal
normal 
```
进入Ubuntu更新grub:
```
sudo update-grub2
sudo grub-install /dev/sda
```
