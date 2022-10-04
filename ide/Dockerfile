FROM amazonlinux:2
LABEL maintainer "kioto"

# install common tools
RUN yum update -y
RUN yum install -y sudo bash wget git man-db nano vim bash-completion \
    tmux  gcc gcc-c++ make tar unzip libuv python python3

# Enable the Docker container to communicate with AWS Cloud9 by
# installing SSH.
RUN yum install -y openssh-server

# Ensure that Node.js is installed.
RUN curl -sL https://rpm.nodesource.com/setup_12.x | sudo bash -
RUN yum install -y --enablerepo=nodesource nodejs

# Greengrass
RUN yum install -y \
    java-17-amazon-corretto-headless \
    java-17-amazon-corretto \
    java-17-amazon-corretto-devel \
    java-17-amazon-corretto-jmods

RUN cd /tmp && \
    curl -s https://d2s8p88vqu9w66.cloudfront.net/releases/greengrass-nucleus-latest.zip > greengrass-nucleus-latest.zip && unzip greengrass-nucleus-latest.zip -d GreengrassCore && \
    java -Droot="/greengrass/v2" -Dlog.store=FILE -jar ./GreengrassCore/lib/Greengrass.jar --aws-region ap-northeast-1 --thing-name gg-device-001 --thing-group-name gg-group --component-default-user ggc_user:ggc_group --provision true --setup-system-service true --deploy-dev-tools true

# create docker user
RUN useradd -d /home/docker -m -s /bin/bash -u 1000 -g 50 -p $(perl -e 'print crypt("tcuser", "\$6\$salt03")') docker && \
    usermod --append -G wheel docker && \
    echo "%wheel    ALL=(ALL)    ALL" >> /etc/sudoers

USER 1000
RUN git clone https://github.com/c9/core.git /home/docker/cloud9 \
    && cd /home/docker/cloud9 \
    && ./scripts/install-sdk.sh


# Python environment
RUN echo 'alias python=python3' >> ~/.bashrc
RUN python3 -m pip install --upgrade pip
RUN pip3 install asyncua

USER root
RUN mkdir -p /workspace
VOLUME ["/workspace"]
WORKDIR /workspace

USER 1000
