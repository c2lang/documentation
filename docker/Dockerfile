FROM ubuntu:16.04

MAINTAINER b.van.den.berg.nl@gmail.com

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y mkdocs && \
    apt-get clean && \
    useradd -m dummy && \
    useradd -m user && echo "user:user" | chpasswd

USER user

COPY --chown=user .ssh /home/user/.ssh

WORKDIR /build

