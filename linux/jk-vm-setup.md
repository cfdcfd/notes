# JKOM CentOS VM Setup

This article lists steps of how to setup a newly created CentOS VM in JKOM. Requirements are:

- create user `jk` with root privilage
- disable `root` login from remote
- disable firewall as it's in local network
- dev-tools such as git
- python3.6
- node8.9.3

## Basics

```bash
ssh root@server_ip_address
```

### Fix locale warning

```bash
sudo vim /etc/environment

# add these lines...

LANG=en_US.utf-8
LC_ALL=en_US.utf-8
```

re-login

### Package update

```bash
yum update
```

### Disable firewall

```bash
systemctl stop firewalld.service
systemctl disable firewalld.service
```

### add user:jk with sudo permission

```bash
adduser jk
passwd jk
usermod -aG wheel jk
```

### disable root login

```bash
sudo yum install vim
sudo vim /etc/ssh/sshd_config

PermitRootLogin no
PubkeyAuthentication yes

sudo service sshd restart
```

re-login with `ssh jk@server_ip_address`

### PubKey login

```bash
ssh-keygen -t rsa
cd
vim .ssh/authorized_keys

```

### Install oh-my-zsh

```bash
sudo yum install zsh
sudo yum install git
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

## Code Env

### Python3

```bash
sudo yum -y update
sudo yum -y install yum-utils
sudo yum -y groupinstall development
sudo yum -y install https://centos7.iuscommunity.org/ius-release.rpm
sudo yum -y install python36u
sudo yum -y install python36u-pip
sudo yum -y install python36u-devel

cd
mkdir .pip
vim .pip/pip.conf


```

### Nodejs

```bash
cd
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash

# re-login

nvm install 8.9.3
npm install -g cnpm --registry=https://registry.npm.taobao.org
cnpm install -g pm2
sudo env PATH=$PATH:/home/jk/.nvm/versions/node/v8.9.3/bin /home/jk/.nvm/versions/node/v8.9.3/lib/node_modules/pm2/bin/pm2 startup systemd -u jk --hp /home/jk
```