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

# download and install lnmp
# wget https://api.sinas3.com/v1/SAE_lnmp/soft/lnmp1.1-full.tar.gz --no-check-certificate
#RUN curl -L http://soft.vpser.net/lnmp/lnmp1.1-full.tar.gz | tar -C / -xz && \
#    cd /lnmp1.1-full && \
#    yes | cp -fr /root/scripts/centos.sh ./ && \
#    ./centos.sh
COPY lnmp1.1-full.tar.gz /
RUN cd / && tar -xzf lnmp1.1-full.tar.gz && \
    cd /lnmp1.1-full && \
    yes | cp -fr /root/scripts/centos.sh ./ && \
    ./centos.sh

# install supervisor
RUN yum -y install python-pip && pip install supervisor && \
    yes | cp -fr /root/scripts/supervisord.conf /etc/supervisord.conf && \
    mkdir /etc/supervisord.conf.d && \
    cp -fr /root/scripts/supervisord.conf.d/* /etc/supervisord.conf.d/

EXPOSE 80
CMD ["supervisord", "-n"]
