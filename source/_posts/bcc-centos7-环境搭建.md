---
title: 'bcc[centos7]环境搭建'
date: 2019-05-10 17:46:11
categories: Linux
tags: 
    - CentOS7
    - bcc
    - docker
mp3: https://datashat.net/music_for_programming_1-datassette.mp3
cover: https://images.unsplash.com/photo-1555949963-ff9fe0c870eb?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1650&q=80
---

本隐趁着百度云搞活动，花了`x RMB`抢了台最最最基础的服务器(`bcc`)，用来胡乱折腾。下文没有一点废话（废话都在开篇这里说完了）的阐述了怎么调教一台全裸的`bcc`，让其好用好看耐用耐看经用经看，折腾不止则更新不断`ing...`

- [基本环境](#%E5%9F%BA%E6%9C%AC%E7%8E%AF%E5%A2%83)
- [开发环境](#%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83)
  - [安装docker](#%E5%AE%89%E8%A3%85docker)

## 基本环境
更新源，安装 `git` /`zsh & oh-my-zsh & plugins`/`pyenv & anaconda`/`nodejs`/`常用shell命令`等。

1. 更新源 & 安装系统工具
    
```bash
yum update

sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

2.  安装git
    
```bash
yum install git
```

3.  安装zsh & oh my zsh
    
```bash
yum install -y zsh
chsh -s /bin/zsh


git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
```

4.  安装oh my zsh plugins
    
```bash
## 开启vim代码高亮
echo 'syntax on' >> ~/.vimrc

## 安装插件
cd ~/.oh-my-zsh/custom/plugins
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
git clone https://github.com/wting/autojump.git

## 更换主题、开启插件
vim ~/.zshrc
ZSH_THEME="ys"
plugins=(git zsh-syntax-highlighting)
```

5.  安装nodejs
    

```bash
yum install nodejs
yum install npm
```

6.  安装pyenv & anaconda
    

```bash
# 安装依赖
sudo yum install -y libffi-devel zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel gcc

# 安装pyenv
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshenv
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >>~/.zshenv  
echo 'eval "$(pyenv init -)"' >>~/.zshenv
source ~/.zshenv

# pyenv install 3.7.3
# 离线安装
v=3.7.3; wget https://www.python.org/ftp/python/$v/Python-$v.tar.xz -P "$PYENV_ROOT"/cache/;pyenv install $v

pyenv global 3.7.3

# 安装集成包
pyenv install anaconda3-2019.03
```

7.  安装autojump

```bash
cd ~/.oh-my-zsh/custom/plugins
git clone https://github.com/wting/autojump.git

cd autojump && ./install.py

# 编辑.zshrc文件
vim ~/.zshrc

[[ -s /root/.autojump/etc/profile.d/autojump.sh ]] && source /root/.autojump/etc/profile.d/autojump.sh
autoload -U compinit && compinit -u

plugins=(git autojump zsh-syntax-highlighting)
```

## 开发环境

### 安装docker

1. 添加软件源信息：
```bash
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

2. 更新 yum 缓存：
```bash
sudo yum makecache fast
```

3. 安装 Docker-ce：
```bash
sudo yum -y install docker-ce
```

4. 启动 Docker 后台服务
```bash
sudo systemctl start docker
```

5. 镜像加速:
```bash
echo '{"registry-mirrors": ["http://hub-mirror.c.163.com"]}' > /etc/docker/daemon.json
```