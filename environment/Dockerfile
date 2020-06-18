FROM ubuntu:xenial

# 更新数据源
WORKDIR /etc/apt
RUN echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse' > sources.list
RUN echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse' >> sources.list
RUN echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse' >> sources.list
RUN echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse' >> sources.list
RUN apt-get update

# 安装依赖
RUN apt-get install git wget vim --assume-yes\
&& apt-get install  libffi6  zlib1g-dev libbz2-dev libssl-dev libncurses5-dev libsqlite3-dev libreadline-dev tk-dev libgdbm-dev libdb-dev libpcap-dev xz-utils libexpat1-dev liblzma-dev libffi-dev libc6-dev  --assume-yes 
#&& apt-get install gnutls-bin --assume-yes \
#&& git config --global http.sslVerify false \
#&& git config --global http.postBuffer 1048576000

# 复制工具包
ADD requirements.txt /usr/local/src 
ADD  .py-solc.tar.gz /root
ADD applicationContext.xml /usr/local/src
ADD ca.crt /usr/local/src
ADD node.crt /usr/local/src
ADD node.key /usr/local/src
ADD sdk.crt /usr/local/src
ADD sdk.key /usr/local/src



RUN  cd ~  &&  wget http://cdn.npm.taobao.org/dist/python/3.7.4/Python-3.7.4.tgz \
&&  tar -zxvf Python-3.7.4.tgz \
&& cd Python-3.7.4 \
&& mkdir -p /usr/local/python3 \
&&  ./configure --prefix=/usr/local/python3 --enable-optimizations \
&& make && make install \
&& rm -rf /usr.bin/python \
&& rm -rf /usr/bin/pip \
&& ln -s /usr/local/python3/bin/python3.7 /usr/bin/python \
&& ln -s /usr/local/python3/bin/pip3.7 /usr/bin/pip

RUN  cd ~ && pip install -i https://pypi.tuna.tsinghua.edu.cn/simple -r /usr/local/src/requirements.txt \
&& pip install -i https://pypi.tuna.tsinghua.edu.cn/simple flask flask-restful uwsgi request \
&& ln -s /usr/local/python3/bin/uwsgi /usr/bin/uwsgi


WORKDIR /
CMD ["/bin/bash"]