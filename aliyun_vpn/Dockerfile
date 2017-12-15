FROM centos
MAINTAINER jinzi9800 <33827226@qq.com>
#安装vpn三宝
RUN yum install epel-release -y \
    && yum update -y \
    && yum install ppp pptpd iptables iptables-service -y \
#配置pptp
    && sed -i -e 's/#\(localip 192.168.0.1\)/\1/' -e 's/#\(remoteip 192.168.0.234-238,192.168.0.245\)/\1/' /etc/pptpd.conf \
#编辑 options.pptpd
    && sed -i -e 's/#ms-dns 10.0.0.1/ms-dns 8.8.8.8/' -e 's/#ms-dns 10.0.0.2/ms-dns 8.8.4.4/' /etc/ppp/options.pptpd \
#修改内核设置，使其支持转发
 # vi /etc/sysctl.conf
 # 将net.ipv4.ip_forward=0
 # 改成net.ipv4.ip_forward=1
 # 将net.ipv4.tcp_syncookies= 1
 # 改成 #net.ipv4.tcp_syncookies= 1
 # 保存并退出
    && sed -i 's/\(net.ipv4.ip_forward\) = 0/\1 = 1/' \
    && sed -i 's/\(net.ipv4.tcp_syncookies= 1\)/#\1/' \
 # 执行以下命令使修改后的内核生效
 # [root@kuro ~]#sysctl -p
    && sysctl -p \
 #添加iptables转发规则
    && iptables -t nat -A POSTROUTING -s 192.168.0.0/24 -o eth1 -jMASQUERADE \
 #（注意：由于阿里云是双网卡，内网eth0，外网eth1，所以这块特别容易误写为eth0，这也是为什么很多杂乱的教程无法配置成功的原因之一）
 #
 #添加好转发规则后保存一下并重启iptables
 #[root@kuro ~]#/etc/init.d/iptables save
    && iptables save \
 #[root@kuro ~]#/etc/init.d/iptablesrestart
    && systemctl restart iptables.service \
 #重启pptp服务
 #[root@kuro ~]#/etc/init.d/pptpd restart
    && systemctl restart pptpd \
 #这里要注意一下，其实此时pptp还没运行起来。所以使用restart重启，会显示Shutting down pptp [FAILED]。还会有一个警告，可以忽略。如不放心可以再用以上命令重启一下pptp就非常顺利的运行了。
 #设置pptp和iptables随系统启动
 #[root@kuro ~]#chkconfig pptpd on
 #[root@kuro ~]#chkconfig iptables on
 #至此，pptp服务端安装结束。
 #在PC端的“网络和共享中心”，设置新的连接或网络，即可访问VPN。
    && systemctl enable pptpd \
    && systemctl enable iptables
COPY /aliyun-VPN/chap-secrets /aliyun-VPN/chap-secrets
