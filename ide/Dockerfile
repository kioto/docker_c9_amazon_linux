FROM amazonlinux:2
LABEL maintainer "kioto"

# install common tools
RUN yum update -y
RUN yum install -y sudo bash wget git man-db nano vim bash-completion \
    tmux  gcc gcc-c++ make tar libuv python python3

# Enable the Docker container to communicate with AWS Cloud9 by
# installing SSH.
RUN yum install -y openssh-server

# Ensure that Node.js is installed.
RUN curl -sL https://rpm.nodesource.com/setup_12.x | sudo bash -
RUN yum install -y --enablerepo=nodesource nodejs

# create docker user
RUN useradd -d /home/docker -m -s /bin/bash -u 1000 -g 50 docker

USER 1000
RUN git clone https://github.com/c9/core.git /home/docker/cloud9 \
    && cd /home/docker/cloud9 \
    && ./scripts/install-sdk.sh

RUN echo 'alias python=python3' >> ~/.bashrc

USER root
RUN mkdir -p /workspace
VOLUME ["/workspace"]
WORKDIR /workspace

USER 1000
