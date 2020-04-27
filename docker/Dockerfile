FROM ubuntu:16.04

MAINTAINER b.van.den.berg.nl@gmail.com

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y mkdocs locales python-pygments && \
    apt-get clean && \
    useradd -m user && echo "user:user" | chpasswd && \
    useradd -m dummy && \
    locale-gen --purge en_US.UTF-8 && \
    echo -e 'LANG="en_US.UTF-8"\nLANGUAGE="en_US:en"\n' > /etc/default/locale && \
    echo 'export PS1="\e[0;31m[\u@\h \w]\$ \e[m"' >> /home/user/.bashrc

USER user
WORKDIR /build

ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en

