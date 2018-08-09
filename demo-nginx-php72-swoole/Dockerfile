FROM centos:6.10

# 初始设置
RUN mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup &&\
    curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo &&\
    yum makecache &&\
    yum install -y vim tar wget curl bzip2 net-tools lsof sysstat cronie python-setuptools &&\
    yum clean all &&\
    easy_install supervisor &&\
    \cp -f /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

# 安装软件
RUN yum clean all &&\
    curl -o /etc/yum.repos.d/shopex-lnmp.repo http://mirrors.shopex.cn/shopex/shopex-lnmp/shopex-lnmp.repo &&\
    yum install epel-release -y &&\
    yum install php-fpm72 ngx_openresty php-memcache72 -y &&\
    yum install perl perl-Data-Dumper libaio* libnuma* -y  &&\
    yum clean all && rm -rf /tmp/* && rm -rf /var/tmp/*

ENV fpm_conf /usr/local/php72/etc/php-fpm.conf
RUN sed -i \
        -e "s/pm.max_children = 128/pm.max_children = 5/g" \
        ${fpm_conf}


ADD deploy/swoole_loader72.so /usr/local/php72/lib/php/extensions/no-debug-non-zts-20170718/swoole_loader72.so
ADD deploy/libsodium.so.23 /usr/lib64/libsodium.so.23
ADD conf/swoole_loader.ini /usr/local/php72/etc/php.d/swoole_loader.ini

#ADD link
RUN ln -s /usr/local/php72/bin/php /usr/local/bin/php &&\
ln -s /usr/local/nginx/sbin/nginx /usr/local/sbin/nginx


#copy supervisord
RUN rm -Rf /etc/supervisord.conf
ADD conf/supervisord.conf /etc/supervisord.conf


# Add Scripts
RUN rm -Rf /start.sh
ADD scripts/start.sh /start.sh
RUN chmod 755 /start.sh




EXPOSE 443 80 22

CMD ["/start.sh"]
