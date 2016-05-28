#操作系统安装以及初始化规范
> V1.0
#操作系统安装流程

##系统初始化规范
1. 目前我司使用的操作系统为<font color=ff0000>Ubuntu12.04和14.04；CentOS6和CentOS7</font>
2. 版本选择，数据库统一使用cobbler上CentOS-7-DB这个专用的profile，其他web应用统一使用Cobbler上的CentOS-7-web
	
### 初始化操作
* 设置DNS 192.168.56.111 192.168.56.112
* 安装Zabbix Agent
* 安装saltstack minior


###安全操作
* HISTSIZE=50      
* HISTFILESIZE=200       
* HISTCONTROL=ignoreboth
* PS1="\[\e[36;1m\][\u@\h \W]\\$ \[\e[m\]"
* HISTTIMEFORMAT="%F %T `whoami` "
* export PROMPT_COMMAND='{ msg=$(history 1 | { read x y; echo $y; });logger "[euid=$(whoami)]":$(who am i):[`pwd`]"$msg"; }'

###主机命名规范
**机房名称-项目-角色-集群-节点.域名**

例如：`idc01-xxshop-bj-node1.shop.com`
###目录规范
####脚本目录规范
* 脚本日志目录:/opt/shell
* 脚本日志目录:/opt/shell/log
* 脚本锁文件目录:/opt/shell/lock

####程序服务安装规范
1. 源码安装路径 /usr/local/appname.version
2. 创建软连接 ln -s /usr/local/appname.version /usr/local/appname

####服务启动服务统一规范

#cobbler如何自动化
1. 服务器采购
2. 服务器验收并设置raid
3. 服务商提供验收单，运维人员验收负责人签字
4. 服务器上架
5. 资产录入
6. 开始自动化安装
    * 将服务器划入装机vlan
    * 根据资产清单上的mac地址，自定义安装
        1. 机房
        2. 机房区域
        3. 机柜
        4. 服务器位置
        5. 服务器网线接入端口
        6. 服务器的该端口MAC地址
        7. 操作系统 分区 预分配的ip地址、主机名、子网、网关、DNS、角色
    * 自动化装机平台，安装。

>例如：新增一台机器
MAC：00:50:56:27:8E:99
IP:192.168.56.12
主机名：linux-node2.oldboyedu.com
掩码：255.255.255.0
网关：192.168.56.2
DNS：192.168.56.2

<pre>cobbler system add --name=linux-node2.oldboyedu.com --mac=00:50:56:27:8E:99  --profile=CentOS-7-x86_64 \
--ip-address=192.168.56.12 --subnet=255.255.255.0 --gateway=192.168.56.2 --interface=eth0 \
--static=1 --hostname=linux-node2.oldboyedu.com --name-servers="192.168.56.2" \
--kickstart=/var/lib/cobbler/kickstarts/CentOS-7-x86_64.cfg</pre>