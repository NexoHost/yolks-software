FROM        ubuntu:latest

LABEL author="NexoHost" maintainer="support@nexohost.es"

RUN         apt update \
            && apt -y upgrade
RUN         dpkg --configure -a \
            && apt-get install -f

RUN         DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC apt-get -y install tzdata

RUN         apt update \
            && apt upgrade -y \
            && apt install -y \
            tar \
            curl \
            unzip \
            iproute2 \
            openssl \
            ca-certificates \
            dnsutils \
            zip \
            gdb \
            libncursesw5-dev \
            libncursesw5 \
            libreadline-dev \
            libmysqlclient-dev \
            default-libmysqlclient-dev \
	        libncurses5-dev \
	        libncurses5 \
            git \
            lsb-release \
            wget \
            gnupg \
            && useradd -d /home/container -m container

RUN         mkdir -p /run/systemd && echo 'docker' > /run/systemd/container
RUN         rm -rf /var/lib/apt/lists/*

RUN curl -OL https://dev.mysql.com/get/mysql-apt-config_0.8.16-1_all.deb && \
    echo "mysql-apt-config mysql-apt-config/select-server select mysql-8.0" | debconf-set-selections && \
    DEBIAN_FRONTEND=noninteractive dpkg -i mysql-apt-config_0.8.16-1_all.deb && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 467B942D3A79BD29

RUN apt-get update && \
    apt-get install -y mysql-client

COPY        ./libmysqlclient.so.16 /usr/lib

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

COPY        ./entrypoint.sh /entrypoint.sh
CMD         [ "/bin/bash", "/entrypoint.sh" ]
