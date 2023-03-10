<p align="center"><b>Linux部分教程</b></p>

查看端口

```
sudo ufw status verbose 端口
```

开启端口

```
sudo ufw allow 端口
```

删除端口

```
sudo ufw delete allow 端口
```

<details><summary><b>Linux 创建用户</b></summary>
<p>

## linux通用

- debian使用`adduser`并且不用执行`passed`

```text
useradd -m 用户名
```

设置密码 > `passwd` 用户名

```text
 passwd
```

- 先登陆root账户，在root用户下更改`sudoers`文件(需要先登录刚才创建的用户才能生成此文件)

```bash
vim /etc/sudoers
```

在`## Allow root to run any commands anywhere`下添加以下内容，按I插入，插入完成后按ESC退出插入，输出:wq!保存退出，如下图所示

```bash
用户名 ALL=(ALL) NOPASSWD:ALL
```

保持长时间连接

```
vim /etc/ssh/sshd_config
```

重载配置文件生效

```bash
source /etc/profile
```

</p>
</details>
<details><summary><b>修改系统默认语言为中文</b></summary>
<p>

## Debian

下载语言包

```bash
apt-get install locales
```

设置语言，在弹出的窗口中找到`zh_CN.UTF-8 UTF-8`按空格进行选着

回车确定，在下个界面选着`zh_CN.UTF-8`设置默认语言

```bahs
dpkg-reconfigure locales
```

## centos

安装中文语言包

```bash
yum groupinstall fonts -y
yum install kde-l10n-Chinese
yum reinstall glibc-common
```

修改etc目录下`locale.conf`的内容为 `LANG="zh_CN.UTF-8"`

```bash
vim /etc/locale.conf
```

执行`sudo reboot`重启，或者执行以下指令重载配置文件

```bash
source /etc/locale.conf
```

</p>
</details>
<details><summary><b>ssh密钥链接</b></summary>
<p>

生成秘钥(一路按回车)

```bash
ssh-keygen
```

查看公钥并复制(以ssh-rsa开头的)

```bash
cat .ssh/id_rsa.pub
```

登录到服务器创建秘钥文件设置权限并(按i)编辑，粘贴刚才复制的公钥(按`esc`输入`:wq`保存)

```bash
mkdir .ssh
chmod 700 .ssh
vim .ssh/authorized_keys
```

```bash
chmod 600 .ssh/authorized_keys
```

重启ssh

```text
service sshd restart
```

</p>
</details>
<details><summary><b>换源和搭建宝塔面板</b></summary>
<p>

先安装面板再换源

## 宝塔面板

Centos安装脚本

```bash
yum install -y wget && wget -O install.sh https://download.bt.cn/install/install_6.0.sh && sh install.sh ed8484bec
```

Ubuntu/Deepin安装脚本

```bash
wget -O install.sh https://download.bt.cn/install/install-ubuntu_6.0.sh && sudo bash install.sh ed8484bec
```

Debian安装脚本

```bash
wget -O install.sh https://download.bt.cn/install/install-ubuntu_6.0.sh && bash install.sh ed8484bec
```

万能安装脚本

```bash
if [ -f /usr/bin/curl ];then curl -sSO https://download.bt.cn/install/install_panel.sh;else wget -O install_panel.sh https://download.bt.cn/install/install_panel.sh;fi;bash install_panel.sh ed8484bec
```

## 更换软件源通用

```bash
bash <(curl -sSL https://gitee.com/SuperManito/LinuxMirrors/raw/main/ChangeMirrors.sh)
```

</p>
</details>
<details><summary><b>Clash-Linux安装</b></summary>
<p>

下载[clash](https://github.com/Dreamacro/clash)

```text
wget https://github.com/Dreamacro/clash/releases/download/v1.12.0/clash-linux-amd64-v1.12.0.gz
```

解压

```text
gzip -d clash-linux-amd64-v1.12.0.gz
```

移动到/usr/local/bin/clash并重命名

```text
mv clash-linux-amd64-v1.12.0 /usr/local/bin/clash
```

给执行权限

```text
chmod +x /usr/local/bin/clash
```

设置成服务

```text
vim /etc/systemd/system/clash.service
```

```bash
[Unit]
Description=clash service
After=network.target
 
[Service]
Type=simple
User=root
ExecStart=/usr/local/bin/clash
Restart=on-failure # or always, on-abort, etc
 
[Install]
WantedBy=multi-user.target
```

设置开机自启

```text
systemctl daemon-reload
systemctl enable clash
```

启动

```text
service clash start
```

## 修改系统代理

设置代理

```text
vim /etc/profile
```

全局代理(开启`kclash`，关闭`gclash`)

```bash
alias kclash="export https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890 all_proxy=socks5://127.0.0.1:7891"
alias gclash="unset  http_proxy  https_proxy  all_proxy"
```

或者直接`ALL`

```bash
alias kclash="export ALL_PROXY=127.0.0.1:7890"
alias gclash="unset ALL_PROXY"
```

全局代理(无开关)

```text
export http_proxy=127.0.0.1:9090
export https_proxy=127.0.0.1:9090
export all_proxy=socks5://127.0.0.1:7891
```

重载配置文件

```text
source /etc/profile
```

查看代理是否代理

```text
env|grep -i proxy
```

测试访问外网和代理地址

```bash
curl www.google.com
```

```bash
curl cip.cc
```

## 配置面板

```text
cd ~/.config/clash
```

下载面板，解压，重命名

```text
wget https://github.com/haishanh/yacd/archive/gh-pages.zip
unzip gh-pages.zip
mv yacd-gh-pages/ dashboard/
```

配置面板在`config.yaml`中添加以下代码

```bash
external-ui: dashboard
```

secret: xxxx #设置访问密码

external-controller: 0.0.0.0:9090  #别忘记在服务器厂商开放端口号

external-ui: dashboard  #面板路径

## 其他指令

查看服务状态

```text
service clash status
```

重启服务

```text
service clash restart
```

停止服务

```text
service clash stop
```

</p>
</details>
<details><summary><b>编译安装</b></summary>
<p>


配置安装,指定路径`--prefix=`,高性能安装`--enable-optimizations`

```bash
./configure --prefix=/usr/local/ --enable-optimizations
```

编译 && 安装

```text
make && make install
```

</p>
</details>
<details><summary><b>安装nodejs,git,chromium,redis(centos)</b></summary>
<p>

添加仓库源

```text
curl -sL https://rpm.nodesource.com/setup_17.x | sudo bash -
```

安装gcc-c++ make nodejs

```text
sudo yum -y install gcc-c++ make nodejs
```

查看nodejs版本

```text
node -v
```

查看npm版本

```text
npm -v
```

## git安装

导入git源

```text
sudo yum -y install https://packages.endpointdev.com/rhel/7/os/x86_64/endpoint-repo.x86_64.rpm
```

安装git

```text
sudo yum install git
```

查看版本

```text
git --version
```

## 安装chromium

```text
yum -y install chromium
```

## 安装redis

```text
yum -y install redis
```

</p>
</details>
<details><summary><b>安装python3.10.9(centos)</b></summary>
<p>

安装依赖

```text
yum -y groupinstall "Development tools"
yum install -y ncurses-devel gdbm-devel xz-devel sqlite-devel tk-devel uuid-devel readline-devel bzip2-devel libffi-devel
yum install -y openssl-devel openssl11 openssl11-devel
```

[下载python](https://www.python.org/)

```text
wget https://www.python.org/ftp/python/3.10.9/Python-3.10.9.tgz
```

解压并进入目录

```text
tar zxvf Python-3.10.9.tgz && cd Python-3.10.9
```

设置编译FLAG，以便使用最新的openssl库。

```text
export CFLAGS=$(pkg-config --cflags openssl11)
export LDFLAGS=$(pkg-config --libs openssl11)
```

设置编译目录

```text
./configure --prefix=/usr/local/python3 --enable-optimizations
```

编译安装

```text
make && make install
```

配置

```text
ln -sf /usr/local/python3/bin/python3 /usr/bin/python
ln -sf /usr/local/python3/bin/pip3 /usr/bin/pip
```

检查是否配置正确

```text
python --version
pip --version
```

</p>
</details>

<details><summary><b>git代码</b></summary>
<p>

## git代码部分

```text
git init
```

```text
git add README.md
```

```text
git commit -m "first commit"
```

```text
git branch -M main
```

```text
git remote add origin https://github.com/Lycofuture/Centos7.6-initial.git
```

```text
git push -u origin main
```
</p>
</details>
