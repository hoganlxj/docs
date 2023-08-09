---
title: "宿主机升级内核"
edition: ce
weight: 3
description: >-
     介绍宿主机如何升级内核
---

## 宿主机内核从yn20191203升级yn20201125

1、查看内核版本

```bash
[root@yunion ~]# uname -r
3.10.0-1062.4.3.el7.yn20191203.x86_64
```

2、升级内核

```bash
[root@yunion ~]# yum install -y kernel-3.10.0-1160.6.1.el7.yn20201125 kernel-devel-3.10.0-1160.6.1.el7.yn20201125 kernel-headers-3.10.0-1160.6.1.el7.yn20201125
[root@yunion ~]# awk -F\' '$1=="menuentry " {print i++ " : " $2}' /etc/grub2.cfg
0 : CentOS Linux (3.10.0-1160.6.1.el7.yn20201125.x86_64) 7 (Core)
1 : CentOS Linux (3.10.0-1062.4.3.el7.yn20191203.x86_64) 7 (Core)
2 : CentOS Linux (3.10.0-957.12.1.el7.x86_64) 7 (Core)
3 : CentOS Linux (0-rescue-c022f8e6249f48fc92f7743f5f6290c9) 7 (Core)
```

3、切换新内核

```bash
[root@yunion ~]# vim /etc/default/grub # 把GRUB_DEFAULT改为新内核的序号，GRUB_DEFAULT=0
[root@yunion ~]# grub2-mkconfig -o /boot/grub2/grub.cfg
[root@yunion ~]# reboot
```
4、查看新内核版本

```bash
[root@yunion ~]# uname -r
3.10.0-1160.6.1.el7.yn20201125.x86_64
```
