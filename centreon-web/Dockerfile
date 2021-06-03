FROM centos:latest

#UPDATE CentOS
RUN yum -y update && yum clean all

# Set up base packages that are expected
RUN dnf -y install openssh-server crontabs NetworkManager firewalld selinux-policy httpd vim curl git

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

# Centreon install
RUN curl -L -s https://raw.githubusercontent.com/centreon/centreon/21.04.x/unattended.sh | sh

# Centreon configuration
RUN echo "date.timezone = Europe/Paris" >> /etc/php.d/50-centreon.ini

STOPSIGNAL SIGRTMIN+3

EXPOSE 80
EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
CMD ["/sbin/init"]
