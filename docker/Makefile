#!/bin/sh

NAME=c2docs-builder

all: create_container

create_container:
	docker build -t ${NAME} .

run:
	docker run --rm -ti -e DISPLAY -v `pwd`/:/build -v /tmp/.X11-unix:/tmp/.X11-unix ${NAME} bash

