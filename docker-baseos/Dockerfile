FROM centos:latest
MAINTAINER  RobinFan robinfan@live.cn
ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8

RUN  \
   curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo && \
   curl -o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo && \
   cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
   echo -e "ZONE=ASia/Shanghai\nUTC=false\nRTC=false" > /etc/sysconfig/clock


# - Install basic packages (e.g. python-setuptools is required to have python's easy_install)
# - Install net-tools, small package with basic networking tools (e.g. netstat)
# - Install inotify, needed to automate daemon restarts after config file changes
# - Install supervisord (via python's easy_install - as it has the newest 3.x version)
# - Install yum-utils so we have yum-config-manager tool available
RUN \
  yum clean all && \
  yum makecache && \
  yum update -y && \
  #yum install -y epel-release && \
  yum install -y net-tools python-setuptools hostname inotify-tools yum-utils && \
  yum clean all && \

  easy_install supervisor

# Add supervisord conf, bootstrap.sh files
ADD container-files /

VOLUME ["/data"]

ENTRYPOINT ["/config/bootstrap.sh"]
