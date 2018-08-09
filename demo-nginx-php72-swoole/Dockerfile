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
    curl -o /etc/yum.repos.d/shopex-lnmp.el7.repo http://mirrors.shopex.cn/shopex/shopex-lnmp/shopex-lnmp.repo &&\
    yum install epel-release -y &&\
    yum install php-fpm56 ngx_openresty php-memcache56  redis -y &&\
    yum install perl perl-Data-Dumper libaio* libnuma* -y  &&\
    yum clean all && rm -rf /tmp/* && rm -rf /var/tmp/*

ENV fpm_conf /usr/local/php56/etc/php-fpm.conf
RUN sed -i \
        -e "s/pm.max_children = 128/pm.max_children = 5/g" \
        ${fpm_conf}


ADD deploy/swoole_loader72.so /usr/local/php56/lib/php/extensions/no-debug-non-zts-20131226/swoole_loader72.so
ADD deploy/libsodium.so.23 /usr/lib64/libsodium.so.23


