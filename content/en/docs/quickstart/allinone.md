---
title: "All in One Installation"
linkTitle: "All in One Installation"
weight: 1
description: >
  Quickly deploy Cloudpods on an existing Linux box using the ansible playbook
---

## Preparation

### Requirements

- OS: Depending on the CPU architecture, the supported distributions are different
    - X86_64: [CentOS 7](http://isoredirect.centos.org/centos/7/isos/x86_64/)
    - ARM64: [Debian 10(buster)](https://www.debian.org/releases/buster/arm64/)
    - ARM64: [Debian 11(bullseye)](https://www.debian.org/releases/bullseye/arm64/)
- Minimum Configuration Requirements: CPU 4 cores, Memory 8GiB, Storage 100GiB

As an example, the following is the environment of the Linux box to be deployed:

| IP   | Login User | OS |
|:----:|:--------:|:--------:|
|10.168.26.216| root | CentOS 7.6|

{{% alert title="Tips" %}}
> 10.168.26.216 is the IP of this test environment, please modify accordingly according to your own environment.
{{% /alert %}}

### Local environment

The local environment is the environment where the user performs the actual operation. The local environment for this installation is executing on a MAC OS laptop. The installation can also be executed on the Linux box to be deployed.

#### Prepare passwordless SSH login

```bash
# Generate the local ssh keypair
# (SKIP this stekp if you already have ~/.ssh/id_rsa.pub locally)
$ ssh-keygen

# Copy the generated ~/.ssh/id_rsa.pub public key to the machine to be deployed
$ ssh-copy-id -i ~/.ssh/id_rsa.pub root@10.168.26.216

# Try to login to the machine to be deployed without password,
# should be able to get the hostname of the deployed machine
# without entering the login password
$ ssh root@10.168.26.216 "hostname"
```

#### Install ansible and git

We first install ansible and git. The commends depends on the OS of the box.

{{< tabs name="ocboot_install" >}}
{{% tab name="CentOS 7" %}}
```bash
# Install ansible locally
$ yum install -y epel-release git python3-pip
$ python3 -m pip install --upgrade pip setuptools wheel
$ python3 -m pip install --upgrade ansible
```
{{% /tab %}}

{{% tab name="Kylin v10" %}}
```bash
# Install ansible locally
$ yum install -y git python3-pip
$ python3 -m pip install --upgrade pip setuptools wheel
$ python3 -m pip install --upgrade ansible
```
{{% /tab %}}


{{% tab name="Debian 10" %}}
```bash
# Install ansible locally
$ apt install -y git python3-pip
$ python3 -m pip install --upgrade pip setuptools wheel
$ python3 -m pip install --upgrade ansible
```
{{% /tab %}}

{{% tab name="Others" %}}
```bash
# Install ansible locally
$ python3 -m pip install --upgrade pip setuptools wheel
$ python3 -m pip install --upgrade ansible
```
{{% /tab %}}

{{< /tabs >}}

## Install Cloudpods

The installation tool is https://github.com/yunionio/ocboot, then according to the configuration of the machine to be deployed, use ansbile playbook to install and configure the Cloudpods. The following commands are performed on the local environment and steps are as follows:

```bash
# Git clone the ocboot installation tool locally
$ git clone -b release/3.8 https://github.com/yunionio/ocboot && cd ./ocboot
```

### Quick installation

Simplely execute run.py with the IP address of the Linux box to start the AllInOne installation.

```bash
./run.py 10.168.26.216
```

This command will automatically generate a configuration yaml config-allinone-current.yaml in the working directory. You may check the default values of the installation parameters in this YAML.

### Customized installation

You may also use a customized installation YAML to start the installation. An example configuration file is shown as following.

```bash
# Write config-allinone.yml configuration file
$ cat <<EOF >./config-allinone.yml
# mariadb_node indicates the node where the mariadb service needs to be deployed
mariadb_node:
  # IP of the machine to be deployed
  hostname: 10.168.26.216
  # SSH Login username of the machine to be deployed
  user: root
  # Username of mariadb
  db_user: root
  # Password of mariadb
  db_password: your-sql-password
# primary_master_node indicates the machine running Kubernetes and OneCloud Platform
primary_master_node:
  hostname: 10.168.26.216
  user: root
  # Database connection address
  db_host: 10.168.26.216
  # Database connection username
  db_user: root
  # Database connection password
  db_password: your-sql-password
  # IP of Kubernetes controlplane
  controlplane_host: 10.168.26.216
  # Port of Kubernetes controlplane
  controlplane_port: "6443"
  # Cloudpods version
  onecloud_version: 'v3.7.2'
  # Cloudpods login username
  onecloud_user: admin
  # Cloudpods login user's password
  onecloud_user_password: admin@123
  # This machine serves as a Cloudpods private cloud computing node
  as_host: true
EOF
```

Once the config-allinone.yml deployment configuration file is filled in, you can execute the ocboot file `. /run.py . /config-allinone.yml` to deploy the cluster.

```bash
# Start deployment
$ ./run.py ./config-allinone.yml
....
# The following output will be displayed when the deployment is completed,
# indicating successful operation
# Open with your browser at https://10.168.26.216
# Login with admin/admin@123 user password to access the Web UI
Initialized successfully!
Web page: https://10.168.26.216
User: admin
Password: admin@123
```

## Done

Then use your browser to visit https://10.168.26.216, enter `admin` for username and `admin@123` for password and you will see the Web UI.

![Login UI](../images/index.png)

## Start using Cloudpods

### Create the first virtual machine

The first virtual machine is created in three steps as follows.

#### 1. Import Images

Browse to [CentOS 7 cloud hosting image](https://cloud.centos.org/centos/7/images/), select a GenericCloud image, and copy the image URL.

In the `Compute` menu, select `Images` then select `Upload` menu. Enter the image name, select `Enter the image URL`, paste the above CentOS 7 image URL and select `OK`.

Additional virtual machine images can be obtained by visiting https://docs.openstack.org/image-guide/obtain-images.html.

#### 2. Create networks (VPC and IP subnets)

- Create VPC

In the `Network` menu, select the `VPC` submenu and choose `Create`. Enter a name, e.g. `vpc0`, and select the target segment, e.g. `192.168.0.0/16`. Click `Create`.

- Create IP Subnetes

After the VPC is created, select the `IP Subnets` submenu and choose `Create`. Enter a name, for example `vnet0`, select VPC as the VPC you just created `vpc0`, select the available zone. Enter `subnet segment`, e.g. `192.168.100.0/24`. Click `Create`.

#### 3. Create Virtual Machine

In the `Compute` menu, select `Servers` and choose `Create`. In this UI, enter the hostname, select the mirror and IP subnet, and create the virtual machine.

### Import public cloud or other private cloud platform resources

Cloudpods itself is a complete suite of private cloud, and can also unify and manage resources from other cloud platforms.

In the `Multicloud` menu, select `Accounts` and create a new one, fill in the authentication information of the corresponding cloud platform according to your needs, and after configuring the cloud account, the Cloudpods service will synchronize the resources of the corresponding cloud platform, and you can view them in the Web UI after the synchronization is completed.

![Multicloud Management](../images/cloudaccount.png)

## FAQ

### 1. Can't find the virtual machine menu in Web UI?

Machine deployed using the All in One way is treated as Cloudpods private cloud computing node by default, which has the ability to create and manage virtual machines.

If there is no virtual machine creation button it should be that the private cloud computing node is not enabled.

Please go to the Web UI, click `Compute/Physical Resources/Hosts` to view the list of hosts, enable the corresponding hosts, refresh the interface and the virtual machine creation button will appear.

{{% alert title="Attention" color="warning" %}}
If you want to use Cloudpods private cloud, you need ensure the computing machine use the kernel compiled by us. You can use the following command to check whether the host uses the kernel including the `yn` keyword.

```bash
# Check if yn keyword kernel is used
$ uname -a | grep yn
Linux office-controller 3.10.0-1160.6.1.el7.yn20201125.x86_64
# If the kernel is not the version with the yn keyword,
# it may be the first time you install it using ocboot,
# and you can reboot into the yn kernel
$ reboot
```
{{% /alert %}}

![Host](../images/host.png)

### 2. Change the hostname of the node, some services fail to start

CLoudpods uses Kubernetes management node, depends on hostname, please change it back.

### 3. How to reinstall

1. SSH login the remote machine, execute the command `ocadm reset -f`.

2. Rerun ocboot `run.py` script.

3. Waiting for the run to finish，use command `kubectl edit deployment onecloud-operator -n onecloud`. Add the following parameters, then save to close.

![](../images/oo_syncuser.png)

4. The modification in step 3 will affect the performance of onecloud-operator, so you can restore the parameters in step 3 when all services are started.

### 4. Other questions？

Other questions are welcome to submit Cloudpods github issues: https://github.com/yunionio/onecloud/issues , we will reply as soon as possible.
