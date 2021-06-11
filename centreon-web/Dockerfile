FROM centos:latest

#UPDATE CentOS
RUN dnf -y update && dnf clean all


# Set up base packages that are expected
RUN dnf -y install openssh-server crontabs NetworkManager firewalld selinux-policy httpd vim curl git sudo


RUN mkdir /var/run/sshd
RUN echo 'root:screencast' | chpasswd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile


RUN systemctl mask dev-mqueue.mount dev-hugepages.mount \
     systemd-remount-fs.service sys-kernel-config.mount \
     sys-kernel-debug.mount sys-fs-fuse-connections.mount \
     graphical.target systemd-logind.service \
     NetworkManager.service systemd-hostnamed.service

STOPSIGNAL SIGRTMIN+3

# Centreon preconfig
RUN dnf -y update
RUN sed -i s/^SELINUX=.*$/SELINUX=disabled/ /etc/selinux/config

#RUN systemctl stop firewalld
#RUN systemctl disable firewalld
RUN dnf -y install dnf-plugins-core epel-release
RUN dnf config-manager --set-enabled powertools

RUN dnf clean all
#RUN rm -r /var/cache/dnf
RUN dnf -y update

RUN dnf module enable php:7.3 -y

# Centreon install
RUN dnf install -y http://yum.centreon.com/standard/21.04/el8/stable/noarch/RPMS/centreon-release-21.04-4.el8.noarch.rpm

RUN dnf clean all
RUN dnf -y update 
RUN dnf -y upgrade

RUN dnf install -y centreon-base-config-centreon-engine centreon-widget\*
RUN dnf install -y centreon centreon-database

RUN dnf -y update 
RUN dnf -y upgrade 

# Centreon configuration
RUN echo "date.timezone = Europe/Paris" >> /etc/php.d/50-centreon.ini
RUN systemctl enable php-fpm httpd mariadb centreon cbd centengine gorgoned snmptrapd centreontrapd snmpd
RUN mkdir -p /var/run/php-fpm
CMD [ "php-fpm", "-F" ]
RUN sed -i 's/listen.acl_users/;listen.acl_users/g' /etc/php-fpm.d/www.conf
RUN sed -i 's/listen.acl_groups/;listen.acl_groups/g' /etc/php-fpm.d/www.conf


STOPSIGNAL SIGRTMIN+3

EXPOSE 80 22

CMD ["/usr/sbin/sshd", "-D"]
CMD ["/sbin/init"]
