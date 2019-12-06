## GitLab服务器搭建过程

-----

### 1. 官网网址
* 首页：https://about.gitlab.com/
* 安装说明：https://about.gitlab.com/installation/

### 2. 安装

在官网的[安装说明](https://about.gitlab.com/installation/)中选择对应的操作系统

执行官网给出的相应的文件

>例：CentOS6/7

```
sudo yum install -y curl policycoreutils-python openssh-server cronie
sudo lokkit -s http -s ssh
sudo yum install postfix
sudo service postfix start
sudo chkconfig postfix on
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash
sudo EXTERNAL_URL="https://gitlab.example.com" yum -y install gitlab-ee
```

### 3. gitlab 服务操作

* 初始化配置 gitlab
* ``gitlab-ctlreconfigure``
* 启动 gitlab 服务
* ``gitlab-ctlstart``
* 停止 gitlab 服务
* ``gitlab-ctlstop``

### 4. 浏览器访问

访问服务器的IP地址即可，若访问失败，则需停止防火墙服务：

CentOS7:``service firewalld stop``
