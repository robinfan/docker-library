FROM registry.cn-hangzhou.aliyuncs.com/robinfan/docker-baseos

MAINTAINER RobinFan <robinfan@live.container>

RUN curl  -o nginxrepo.rpm  http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm && \
    rpm -ivh nginxrepo.rpm  && \
    yum -y install nginx java-1.8.0-openjdk



# tweak nginx config
RUN sed -i -e"s/worker_processes  1/worker_processes 5/" /etc/nginx/nginx.conf && \
sed -i -e"s/keepalive_timeout\s*65/keepalive_timeout 2/" /etc/nginx/nginx.conf && \
sed -i -e"s/keepalive_timeout 2/keepalive_timeout 2;\n\tclient_max_body_size 100m/" /etc/nginx/nginx.conf && \
echo "daemon off;" >> /etc/nginx/nginx.conf


ADD ./container-files /


#RUN  /opt/elasticsearch/bin/plugin install  file:///opt/x-pack-5.4.2.zip

#add an single user
RUN useradd  elasticuser && \
    chown -R elasticuser:elasticuser /opt

EXPOSE 80 9300

VOLUME /opt/elasticdb /opt/logs
