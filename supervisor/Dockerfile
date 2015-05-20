FROM centos:6
MINTAINER HwangJR

# patch the system
RUN rpm -ivh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm & \
    yum clean all && \
    yum -y update

# Install supervisor
RUN yum -y install python-pip && pip install supervisor && echo_supervisord_conf > /etc/supervisord.conf

EXPOSE 80
CMD ["supervisord", "-n"]
