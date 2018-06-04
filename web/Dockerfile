# Base image to use, this nust be set as the first line
FROM ubuntu:16.04

# Maintainer: docker_user <docker_user at email.com> (@docker_user)
MAINTAINER docker_user lixiang3024@163.com

# Commands to update the image
RUN echo "deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse" >> /etc/apt/sources.list

RUN apt-get update && apt-get upgrade -yf
# 安装supervisor
RUN apt-get install -y supervisor
# 修改root密码
RUN echo 'root:8~W5!RAx?u6B'|chpasswd
#修改系统时区
RUN apt-get install tzdata
#定义环境变量
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
#安装必要的软件包

#ifconfig 
RUN apt-get install -y net-tools
#ping
RUN apt-get install -y iputils-ping
#vim
RUN apt-get install -y vim
#ssh
RUN apt-get install -y openssh-server

RUN mkdir /var/run/sshd

RUN sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config
RUN mkdir /root/.ssh
EXPOSE 22

#ftp
RUN apt install -y vsftpd
RUN sed -i 's/#write_enable=YES/write_enable=YES/g' /etc/vsftpd.conf
RUN sed -i 's/#root/root/g' /etc/ftpusers


#采用进程管理工具实现镜像run时自动启动ssh服务
RUN mkdir -p /var/log/supervisor

COPY supervisord.conf /etc/supervisord.conf

# 清除缓存
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
#启动supervisord服务
CMD ["/usr/bin/supervisord"]



