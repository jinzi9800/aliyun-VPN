FROM centos
MAINTAINER jinzi9800 <33827226@qq.com>
#安装vpn三宝
RUN yum install ppp pptpd iptables iptables.service -y \
#配置pptp
    && echo 'localip 192.168.0.1
remoteip 192.168.0.234-238,192.168.0.245'>> /etc/pptpd.conf
#编辑 options.pptpd
    && echo "ms-dns 8.8.8.8
ms-dns 8.8.4.4">> /etc/ppp/options.pptpd
