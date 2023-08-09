---
title: "升级服务"
linkTitle: "升级服务"
edition: ce
weight: 90
description: >
  介绍如何升级服务版本到指定版本
---

本文介绍从 v3.8.x 升级到 v3.9.x 的步骤以及注意事项。

版本升级建议从相邻的版本升级，比如从 v3.2.x 升级到 v3.8.x 需要以下的步骤：

1. v3.2.x => v3.3.x
2. v3.3.x => v3.4.x
3. v3.4.x => v3.6.x
4. v3.6.x => v3.7.x
5. v3.7.x => v3.8.x

直接跨多个版本升级可能会出现问题，建议参考以下的内容选择升级步骤:

- [v3.1.x 升级到 v3.2.x](https://www.cloudpods.org/v3.2/docs/setup/upgrade)
- [v3.2.x 升级到 v3.3.x](https://www.cloudpods.org/v3.3/docs/setup/upgrade)
- [v3.3.x 升级到 v3.4.x](https://www.cloudpods.org/v3.4/docs/setup/upgrade)
- [v3.4.x 升级到 v3.6.x](https://www.cloudpods.org/v3.6/zh/docs/setup/upgrade)
- [v3.6.x 升级到 v3.7.x](https://www.cloudpods.org/v3.7/zh/docs/setup/upgrade)
- [v3.7.x 升级到 v3.8.x](https://www.cloudpods.org/v3.8/zh/docs/setup/upgrade)

总体来说，升级的步骤如下:

使用我们编写的 [ocboot](https://github.com/yunionio/ocboot) 工具进行升级，这个工具主要是调用 ansible 来升级集群里面的所有节点。

1. 使用 git 拉取最新的 [ocboot](https://github.com/yunionio/ocboot) 代码，切换到 {{<release_version>}} tag
2. 使用 [ocboot](https://github.com/yunionio/ocboot) 里面的 `./ocboot.py upgrade` 命令进行版本升级

## 查看当前版本

可以使用 kubectl 查看当前集群的版本

```bash
# 使用 kubectl 获得当前集群的版本为 {{<pre_release_version>}}
$ kubectl -n onecloud get onecloudclusters default -o=jsonpath='{.spec.version}'
{{<pre_release_version>}}
```

## 拉取 ocboot 工具

如果本地已经有 ocboot 工具可以跳过此步，只用把代码更新到对应的分支。

```bash
# 本地安装 ansible
$ yum install -y python3-pip
$ python3 -m pip install --upgrade pip setuptools wheel
$ python3 -m pip install --upgrade ansible paramiko

# 下载 ocboot 工具到本地
$ git clone -b {{<release_branch>}} https://github.com/yunionio/ocboot && cd ./ocboot
```

## 更新 ocboot 代码

```bash
$ git fetch
$ git checkout {{<release_version>}}
```

## 升级 Cloudpods 服务

更新服务的原理是通过本机 ssh 免密码远程登录到集群的第一个控制节点，获取所有节点的信息后，然后通过 ansible 执行 playbook 更新，所以有以下要求：

1. 本机能够 ssh 远程登录 PRIMARY_MASTER_HOST
2. PRIMARY_MASTER_HOST 能够 ssh 免密码登录集群中的其它节点

如果没有设置免密码登录，请使用 *ssh-copy-id -i ~/.ssh/id_rsa.pub root@PRIMARY_MASTER_HOST* 命令把公钥下发到自己环境对应的节点。

升级的版本号可以到 [CHANGELOG {{<release_branch>}} 页面](../../development/changelog/release-3.10/) 查询。

```bash
# 使用 ocboot 相关服务升级到 {{<release_version>}} 版本
# 该步骤会因为拉取 docker 镜像等待较长时间，请耐心等待
# PRIMARY_MASTER_HOST 是指部署集群的第一个节点的 ip 地址
# 需要本机能够使用 ssh 密钥登录上去
$ ./ocboot.py upgrade <PRIMARY_MASTER_HOST> {{<release_version>}}

# 另外可以使用 `./ocboot.py upgrade --help` 查看其它可选参数
# 比如:
# --user 可以指定其它 ssh 用户
# --port 指定 ssh 端口
# --key-file 指定另外的 ssh private key
# --as-bastion 可以让 PRIMARY_MASTER_HOST 作为堡垒机部署在内网的宿主机

# 另外可以在升级的过程中可以登录到 PRIMARY_MASTER_HOST， 使用 kubectl 查看对应 pods 的升级情况
$ kubectl get pods -n onecloud --watch
```

## 降级相关

如果升级后遇到功能不符合预期或者 bug 之类的问题，可以通过下面的命令降级回滚。

{{% alert title="注意" color="warning" %}}
- 一般小版本降级没有问题，比如从 {{<release_version>}} 降级到 v3.10.0
- 跨版本降级可能会有问题，比如从 {{<release_version>}} 降级到 {{<pre_release_version>}}

如果遇到问题请到 [GitHub 提 issue](https://github.com/yunionio/cloudpods/issues) 或者 [联系我们](/zh/docs/contact) 获取帮助。
{{% /alert %}}

```bash
# 降级的原理是修改各个服务的 image version
# 比如现在的版本是 {{<release_version>}}，然后想要降级到 {{<pre_release_version>}}
# 在第一个控制节点执行如下命令降级会 {{<pre_release_version>}}
$ /opt/yunion/bin/ocadm cluster update --version {{<pre_release_version>}} --wait

# 降级会重新拉取新的镜像，可以再开一个窗口
# 使用下面的命令查看每个 pod 的更新情况
$ kubectl -n onecloud get pods -w
```

## 回退操作

ocboot 从 v3.9.7 版本起，每次升级集群服务的时候，会把 operator 的 deployment yaml 和 onecloudcluster 的 yaml 文件备份到 `/opt/yunion/ocboot/_upgrade/` 里面，方便以后手动回滚组件。

当出现升级失败，或者升级后想要回退到升级前的版本，可以使用以下的操作：

```bash
# 查看 /opt/yunion/ocboot/_upgrade/ 目录里面的升级备份目录
# 里面的子目录以升级前时间作为目录名
$ ls  /opt/yunion/ocboot/_upgrade/
2023.02.20-10:10:07

# 比如想要回退到 2023.02.20-10:10:07 前的版本，就进行以下操作
$ cd /opt/yunion/ocboot/_upgrade/2023.02.20-10\:10\:07/
# 查看相关 yaml 文件
$ ls -lh
-rw-r--r-- 1 root root  61K Feb 20 10:10 oc.yml # oc.yaml 为 onecloudcluster 描述文件
-rw-r--r-- 1 root root 3.9K Feb 20 10:10 operator.yml # operator.yaml 为 operator deployment 描述文件

# 先回退 operator
$ kubectl apply -f operator.yml
deployment.extensions/onecloud-operator configured
# 等到新创建的 operator 变成 Running，如果是 ContainerCreating 状态就等待一段时间
$ kubectl get pods -n onecloud | grep operator
onecloud-operator-5557d86d74-7s5sl                   1/1     Running   0          2s

# 再回退 onecloudcluster 就可以了
$ kubectl apply -f oc.yaml
```
