FROM centos:latest
MAINTAINER "Vladimir V. Marchenko <burzum>" <burzum@speroteck.com>
ENV container docker

RUN yum -y install gperftools-libs deltarpm yum-utils
RUN rpm -ivh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
RUN yum -y install epel-release
RUN /usr/bin/yum-config-manager --quiet --enable epel
RUN /usr/bin/yum-config-manager --quiet --enable nginx

RUN yum -y update
RUN yum -y install bash-completion nano git curl libpng-devel bzr tar gzip bzip2 \
    passwd openssh-server initscripts wget golang sqlite nginx nodejs npm

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

RUN npm update -g npm
RUN mkdir /opt/ottemo
WORKDIR /opt/ottemo
RUN git clone https://github.com/ottemo/store-ng.git -b develop /opt/ottemo/storefront
WORKDIR /opt/ottemo/storefront
RUN npm install
RUN npm install -g bower
RUN yes | bower install --allow-root
RUN npm install -g gulp
ENV FOUNDATION_URI=http://localhost:3000
RUN gulp build
RUN rm -f /etc/nginx/conf.d/default
RUN sed -i 's/80/8080/' /etc/nginx/nginx.conf
RUN sed -i 's/\/usr\/share\/nginx\/html/\/opt\/ottemo\/storefront\/dist/' /etc/nginx/nginx.conf
RUN echo "daemon off;" >> /etc/nginx/nginx.conf

EXPOSE 8080

VOLUME ["/sys/fs/cgroup"]
VOLUME ["/run"]

RUN systemctl enable nginx.service 
CMD  ["/usr/lib/systemd/systemd"]

