#!/bin/sh

NAME=c2docs-builder

all: create_container

create_container:
	docker build -t ${NAME} .

run:
	docker run --rm -ti -p 127.0.0.1:8000:8000 --name docker-mkdocs -e DISPLAY -v `pwd`/:/build -v /tmp/.X11-unix:/tmp/.X11-unix ${NAME} bash

