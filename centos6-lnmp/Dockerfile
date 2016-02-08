FROM centos:6
MAINTAINER HwangJR

# patch the system
RUN rpm -ivh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
#    yum clean all && \
#    yum -y update

# set timezone to PRC
RUN mv /etc/localtime /etc/localtime.bak && \
    ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

# install NTP
RUN yum -y install ntp && \
    service ntpd start && \
    chkconfig ntpd on

# install tar
RUN yum -y install tar

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
