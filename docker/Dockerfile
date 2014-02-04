FROM ubuntu:12.04
MAINTAINER Anton Romanovich anthony.romanovich@gmail.com

RUN echo "deb http://archive.ubuntu.com/ubuntu precise universe" > /etc/apt/sources.list.d/universe.list
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 6E63A3B44CF61F28A8A477DE5563540C431533D8  # syslog
RUN echo "deb http://ppa.launchpad.net/tmortensen/rsyslogv7/ubuntu precise main" >> /etc/apt/sources.list
RUN apt-get update -qq
RUN apt-get install -y python-apt ca-certificates git python-pip python-dev libev4 libev-dev \
                       libssh-dev libpcre3-dev rsyslog lxc iptables aufs-tools
RUN git clone https://github.com/aromanovich/kozmic-ci.git /src
RUN pip install -r /src/requirements/kozmic.txt
RUN pip install -r /src/requirements/tailer.txt
RUN pip install supervisor
RUN groupadd docker
RUN useradd -m -d /home/kozmic -G docker -s /bin/bash kozmic
ADD https://get.docker.io/builds/Linux/x86_64/docker-latest /bin/docker
ADD ./files/rsyslog.conf /etc/rsyslog.d/50-default.conf
ADD ./files/supervisor.conf /etc/supervisor.conf
ADD ./files/kozmic-uwsgi.ini /etc/kozmic-uwsgi.ini
ADD ./files/tailer-uwsgi.ini /etc/tailer-uwsgi.ini
ADD ./files/wrapped-docker /bin/wrapped-docker
ADD ./files/run.sh /run.sh
RUN chmod +x /bin/wrapped-docker /bin/docker /run.sh
VOLUME /var/lib/docker