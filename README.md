# 小米Xiaomi路由器 AX3000E 解锁SSH和科学上网教程
本教程一共分为两个部分，第一个是解锁SSH，第二个是安装ShellClash科学上网。在小米路由器原来的固件上安装科学上网软件，不影响路由器原有的功能，并且支持全屋科学上网。BE3600和AX3000T和BE6500也可以参考本教程。

## 一、小米路由器 BE6500 解锁SSH教程

### 1、Windows搜索cmd，以管理员身份运行：

    curl -X POST http://192.168.31.1/cgi-bin/luci/;stok=xxx/api/xqsystem/start_binding -d "uid=1234&key=1234'%0Anvram%20set%20ssh_en%3D1'"
    curl -X POST http://192.168.31.1/cgi-bin/luci/;stok=xxx/api/xqsystem/start_binding -d "uid=1234&key=1234'%0Anvram%20commit'"
    curl -X POST http://192.168.31.1/cgi-bin/luci/;stok=xxx/api/xqsystem/start_binding -d "uid=1234&key=1234'%0Ased%20-i%20's%2Fchannel%3D.*%2Fchannel%3D%22debug%22%2Fg'%20%2Fetc%2Finit.d%2Fdropbear'"
    curl -X POST http://192.168.31.1/cgi-bin/luci/;stok=xxx/api/xqsystem/start_binding -d "uid=1234&key=1234'%0A%2Fetc%2Finit.d%2Fdropbear%20start'"

### 2、登录路由器
工具下载：[Putty下载>>](https://github.com/kjqq/AX3000E/releases/download/rom/putty.zip)
通过SSH登录，用户名是：root，密码通过网站计算，[密码计算网站>>](https://miwifi.dev/ssh)
更改SSH登录密码，执行以下指令：（修改之后的用户名是：root，密码是：admin）

    echo -e 'admin\nadmin' | passwd root

### 3、固化SSH

    nvram set ssh_en=1
    nvram set telnet_en=1
    nvram set uart_en=1
    nvram set boot_wait=on
    nvram commit

永久开启SSH（重启不会关闭）

    mkdir /data/auto_ssh && cd /data/auto_ssh
    curl -O https://fastly.jsdelivr.net/gh/lemoeo/AX6S@main/auto_ssh.sh
    chmod +x auto_ssh.sh

    uci set firewall.auto_ssh=include
    uci set firewall.auto_ssh.type='script'
    uci set firewall.auto_ssh.path='/data/auto_ssh/auto_ssh.sh'
    uci set firewall.auto_ssh.enabled='1'
    uci commit firewall

## 二、小米路由器 BE6500 安装 ShellCrash 科学上网教程
视频教程：▶https://youtu.be/Z9hr7bGhA7M

**Clash安装源：**

    export url='https://fastly.jsdelivr.net/gh/juewuy/ShellCrash@master' && sh -c "$(curl -kfsSl $url/install.sh)" && source /etc/profile &> /dev/null

**备用安装源：**

    export url='https://gh.jwsc.eu.org/master' && sh -c "$(curl -kfsSl $url/install.sh)" && source /etc/profile &> /dev/null

**Clash管理地址：** http://192.168.31.1:9999/ui/ (如果打不开请按Ctrl+F5 刷新)

