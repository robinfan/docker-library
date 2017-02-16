FROM registry.cn-hangzhou.aliyuncs.com/robinfan/docker-baseos
MAINTAINER RobinFan <robinfan@live.cn>

RUN curl  -o nginxrepo.rpm  http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm && \
    rpm -Uvh nginxrepo.rpm && \
    yum -y install epel-release  && \
    rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm


# install php  && nginx
RUN yum -y install mod_php71w php71w-bcmath php71w-cli php71w-common php71w-dba php71w-devel php71w-enchant php71w-embedded  php71w-fpm php71w-gd php71w-imap php71w-intl  php71w-ldap php71w-mbstring php71w-mcrypt php71w-mysql  && \
yum -y install  php71w-opcache php71w-pdo  php71w-pdo_dblib php71w-pear php71w-pecl-apcu php71w-pecl-mongodb php71w-pecl-redis php71w-phpdbg php71w-pgsql && \
yum -y install  php71w-process php71w-pspell php71w-recode php71w-snmp php71w-soap php71w-tidy php71w-xml php71w-xmlrpc && \
yum -y install nginx gcc git openssl-devel

# tweak nginx config
RUN sed -i -e"s/worker_processes  1/worker_processes 5/" /etc/nginx/nginx.conf && \
sed -i -e"s/keepalive_timeout\s*65/keepalive_timeout 2/" /etc/nginx/nginx.conf && \
sed -i -e"s/keepalive_timeout 2/keepalive_timeout 2;\n\tclient_max_body_size 100m/" /etc/nginx/nginx.conf && \
echo "daemon off;" >> /etc/nginx/nginx.conf

# tweak php-fpm config
RUN sed -i -e "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g" /etc/php.ini && \
sed -i -e "s/upload_max_filesize\s*=\s*2M/upload_max_filesize = 100M/g" /etc/php.ini && \
sed -i -e "s/post_max_size\s*=\s*8M/post_max_size = 100M/g" /etc/php.ini && \
sed -i -e "s/user = apache/user = www/g" /etc/php-fpm.d/www.conf && \
sed -i -e "s/group = apache/group = www/g" /etc/php-fpm.d/www.conf && \
sed -i -e "s/;daemonize\s*=\s*yes/daemonize = no/g" /etc/php-fpm.conf && \
sed -i -e "s/;catch_workers_output\s*=\s*yes/catch_workers_output = yes/g" /etc/php-fpm.d/www.conf && \
sed -i -e "s/pm.max_children = 5/pm.max_children = 9/g" /etc/php-fpm.d/www.conf && \
sed -i -e "s/pm.start_servers = 2/pm.start_servers = 3/g" /etc/php-fpm.d/www.conf && \
sed -i -e "s/pm.min_spare_servers = 1/pm.min_spare_servers = 2/g" /etc/php-fpm.d/www.conf && \
sed -i -e "s/pm.max_spare_servers = 3/pm.max_spare_servers = 4/g" /etc/php-fpm.d/www.conf && \
sed -i -e "s/pm.max_requests = 500/pm.max_requests = 200/g" /etc/php-fpm.d/www.conf

#install redis extented
#RUN git clone https://github.com/igbinary/igbinary7.git && \
#cd igbinary7 && phpize && ./configure && make &&  make install && \
#echo "extension=igbinary.so" > /etc/php.d/70-igbinary.ini && \
#pecl install redis  && \
#echo "extension=redis.so" > /etc/php.d/70-redis.ini && \
#pecl install mongodb  && \
#echo "extension=mongodb.so" > /etc/php.d/70-mongodb.ini


ADD ./container-files /


# Setup Volume

RUN useradd -s /sbin/nologin www && \
    chown -R www:www /opt/www

# add test PHP file

# Expose Ports
EXPOSE 443
EXPOSE 80
