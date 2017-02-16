FROM registry.aliyuncs.com/docker-baseos
MAINTAINER RobinFan <robinfan@live.cn>

# Install required packages and remove the apt packages cache when done.
RUN curl  -o nginxrepo.rpm  http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm && \
    rpm -Uvh nginxrepo.rpm && \
    yum -y install epel-release

RUN yum install -y gcc nginx MySQL-python python-devel uwsgi uwsgi-plugin-python && \
    easy_install -i http://pypi.douban.com/simple pip  && \
		pip install -i http://pypi.douban.com/simple --trusted-host pypi.douban.com django && \
    pip install -i http://pypi.douban.com/simple --trusted-host pypi.douban.com django-bootstrap3

# tweak nginx config
RUN sed -i -e"s/worker_processes  1/worker_processes 5/" /etc/nginx/nginx.conf && \
sed -i -e"s/keepalive_timeout\s*65/keepalive_timeout 2/" /etc/nginx/nginx.conf && \
sed -i -e"s/keepalive_timeout 2/keepalive_timeout 2;\n\tclient_max_body_size 100m/" /etc/nginx/nginx.conf && \
echo "daemon off;" >> /etc/nginx/nginx.conf


ADD ./container-files /

RUN useradd -s /sbin/nologin www && \
    chown -R www:www /opt/app

# Expose Ports
EXPOSE 443
EXPOSE 80
