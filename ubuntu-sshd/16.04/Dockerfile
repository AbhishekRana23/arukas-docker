FROM       ubuntu:16.04
MAINTAINER Aleksandar Diklic "https://github.com/rastasheep"

RUN apt-get update

RUN apt-get install -y openssh-server
RUN mkdir /var/run/sshd

RUN echo 'root:root' |chpasswd

RUN sed -ri 's/^PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config

EXPOSE 22

# My custom
WORKDIR /root/

RUN echo 'deb http://archive.ubuntu.com/ubuntu/ xenial-security multiverse' >> /etc/apt/sources.list
RUN echo 'deb-src http://archive.ubuntu.com/ubuntu/ xenial-security multiverse' >> /etc/apt/sources.list

RUN apt-get update && apt-get install m2crypto git vim  mtr vnstat supervisor curl wget cron screen -y 

RUN apt-get install firefox lxde-core lxterminal tightvncserver xrdp proxychains cpulimit flashplugin-installer htop -y

# download extra
RUN wget https://addons.mozilla.org/firefox/downloads/file/3675678/ebesucher_addon-1.0.35.6-an+fx.xpi \
&& wget https://addons.mozilla.org/firefox/downloads/file/3664370/vpn_free_betternet_vpn_proxy-6.4.8-fx.xpi


# xvnc server porst, if $DISPLAY=:1 port will be 5901	
EXPOSE 15900

ADD scripts /root

RUN chmod a+x /root/*.sh  
RUN echo '*/1 * * * * root bash /root/check.sh' >> /etc/crontab
RUN service cron restart

CMD    ["/usr/sbin/sshd", "-D"]
