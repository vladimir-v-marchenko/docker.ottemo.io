FROM centos:latest
MAINTAINER "Vladimir V. Marchenko <burzum>" <burzum@speroteck.com>
ENV container docker

RUN yum -y install gperftools-libs deltarpm yum-utils

RUN yum -y install epel-release
RUN /usr/bin/yum-config-manager --quiet --enable epel

RUN yum -y update
RUN yum -y install bash-completion nano git curl libpng-devel bzr tar gzip bzip2 \
    passwd openssh-server initscripts wget golang wget sqlite 

RUN yum -y swap -- remove fakesystemd -- install systemd systemd-libs
RUN systemctl mask dev-mqueue.mount dev-hugepages.mount \
    systemd-remount-fs.service sys-kernel-config.mount \
    sys-kernel-debug.mount sys-fs-fuse-connections.mount
RUN systemctl mask display-manager.service systemd-logind.service
RUN systemctl disable graphical.target; systemctl enable multi-user.target

#ADD dbus.service /etc/systemd/system/dbus.service
RUN echo 0t3m0d0K3r | passwd --stdin root
RUN echo "UseDNS no" >> /etc/ssh/sshd_config 
RUN sed -i 's/UsePrivilegeSeparation sandbox/UsePrivilegeSeparation no/' /etc/ssh/sshd_config
RUN yum -y clean all

RUN mkdir /opt/ottemo/
RUN mkdir /opt/ottemo/foundation
WORKDIR /opt/ottemo/foundation
RUN mkdir /opt/ottemo/foundation/media
RUN wget https://github.com/ottemo/foundation-stable/releases/download/v0.9.4-preview/OttemoFoundation-v0.9.4-preview-linux-amd64.tar.gz
RUN tar -xvzf OttemoFoundation-v0.9.4-preview-linux-amd64.tar.gz
RUN mv /opt/ottemo/foundation/release/* /opt/ottemo/foundation
ADD foundation.service /etc/systemd/system/foundation.service

EXPOSE 3000
EXPOSE 22

VOLUME ["/sys/fs/cgroup"]
VOLUME ["/run"]

RUN systemctl enable sshd
RUN systemctl enable foundation.service
CMD  ["/usr/lib/systemd/systemd"]

