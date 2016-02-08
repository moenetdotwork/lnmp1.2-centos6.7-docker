#
# MAINTAINER        Max
# DOCKER-VERSION    1.9.1
# CENTOS-VERSION    6.7
# LNMP-VERSION      1.2 www.lnmp.org
# VERSION           1.4
# DATE              02/08/2016
# Dockerfile for building LNMP image base on Centos6.7
#

FROM centos:6.7
MAINTAINER Max

ENV TZ "Asia/Shanghai"
ENV TERM xterm

ADD aliyun-mirror.repo /etc/yum.repos.d/CentOS-Base.repo
ADD aliyun-epel.repo /etc/yum.repos.d/epel.repo

# Update
RUN yum -y update

RUN yum -y install openssh-server && \
    yum -y install pwgen && \
    rm -f /etc/ssh/ssh_host_ecdsa_key /etc/ssh/ssh_host_rsa_key && \
    ssh-keygen -q -N "" -t dsa -f /etc/ssh/ssh_host_ecdsa_key && \
    ssh-keygen -q -N "" -t rsa -f /etc/ssh/ssh_host_rsa_key && \
    sed -i "s/#UsePrivilegeSeparation.*/UsePrivilegeSeparation no/g" /etc/ssh/sshd_config && \
    sed -i "s/UsePAM.*/UsePAM yes/g" /etc/ssh/sshd_config
    
# Install wget tar screen htop passwd nano packages
RUN yum -y install wget tar screen htop passwd nano

# copy scripts
COPY scripts /root/scripts

# Download and install lnmp1.2.
RUN wget -c https://api.sinas3.com/v1/SAE_lnmp/soft/lnmp1.2-full.tar.gz --no-check-certificate && tar zxf lnmp1.2-full.tar.gz -C root && rm -rf lnmp1.2-full.tar.gz
    cd /lnmp1.2-full && \
    yes | cp -fr /root/scripts/centos.sh ./ && \
    ./centos.sh

# install supervisor
RUN yum -y install python-pip && pip install supervisor && \
    yes | cp -fr /root/scripts/supervisord.conf /etc/supervisord.conf && \
    mkdir /etc/supervisord.conf.d && \
    cp -fr /root/scripts/supervisord.conf.d/* /etc/supervisord.conf.d/

EXPOSE 80 21 22 3306 6379 11211
CMD ["supervisord", "-n"]
