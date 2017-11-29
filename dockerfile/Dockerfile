FROM centos
MAINTAINER jinzi9800 <33827226@qq.com>
#安装vpn三宝
RUN yum install epel-release -y \
    && yum update -y \
    && yum install ppp pptpd iptables iptables-service -y \
#配置pptp
    && sed -i -e 's/#\(localip 192.168.0.1\)/\1/' -e 's/#\(remoteip 192.168.0.234-238,192.168.0.245\)/\1/' /etc/pptpd.conf
#编辑 options.pptpd
    && sed -i -e 's/#ms-dns 10.0.0.1/ms-dns 8.8.8.8/' -e 's/#ms-dns 10.0.0.2/ms-dns 8.8.4.4/' /etc/ppp/options.pptpd \
