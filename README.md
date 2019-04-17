# X vs Wayland

## Keylogger
````bash
docker build -t keylogger keylogger
docker run --rm -e "DISPLAY:$DISPLAY" -v /tmp/.X11-unix:/tmp/.X11-unix -u 1000 keylogger
````

|Test| Xorg	| Wayland | 
|----|-------|---------|
| Keyboard Events from Root Windows| :thumbsdown: captured | :thumbsdown: captured | 


## Screenshot
````bash
docker build -t screenshot screenshot
docker run --rm -e "DISPLAY:$DISPLAY" -v /tmp/.X11-unix:/tmp/.X11-unix -u 1000 screenshot > screenshot.png
````

|Test| Xorg	| Wayland | 
|----|-------|---------|
| Capture a full screenshot| :thumbsdown: captured | unknown | 



