## CentOS-7虚拟机下安装Gitlab服务器

-----

### 一、配置虚拟机

详见：[配置CentOS-7虚拟机为本地服务器](配置CentOS-7虚拟机为本地服务器.md)

### 二、安装Gitlab

#### 1. 官方安装命令

```
sudo yum install -y curl policycoreutils-python openssh-server cronie
sudo lokkit -s http -s ssh
sudo yum install postfix
sudo service postfix start
sudo chkconfig postfix on
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash
sudo EXTERNAL_URL="https://gitlab.example.com" yum -y install gitlab-ee
```

由于安装时，需要联网下载几百M的安装文件，非常耗时，又很难在虚拟机上保证文件下载的正确性，所以应提前把所需 RPM 包下载并安装好。
[下载地址 ](https://packages.gitlab.com/gitlab/gitlab-ce/packages/el/7/gitlab-ce-10.8.2-ce.0.el7.x86_64.rpm)

#### 2. 调整后的安装命令

```
sudo rpm -ivh /opt/gitlab-ce-10.8.2-ce.0.el7.x86_64.rpm
sudo lokkit -s http -s ssh
sudo yum install postfix
sudo service postfix start
sudo chkconfig postfix on
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
sudo EXTERNAL_URL="https://gitlab.example.com" yum -y install gitlab-ce
```

#### 3. 上传文件gitlab-ce.rpm至/opt

将下载好的gitlab-ce上传到Linux

![将下载好的gitlab-ce上传到Linux](../pictures/在CentOS-7下安装Gitlab服务器/将下载好的gitlab-ce上传到Linux.png)

右键 -> 上传

**上传文件后，执行调整后的安装命令**
