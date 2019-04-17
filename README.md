# X vs Waylanda

# Keylogger
````bash
docker build -t keylogger keylogger
docker run --rm -e "DISPLAY:$DISPLAY" -v /tmp/.X11-unix:/tmp/.X11-unix -u 1000
````

| Xorg	| Wayland | 
|-------|---------|
| Events from from all Windows captured | unknown| 




