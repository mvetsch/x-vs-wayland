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
| Capture a full screenshot| :thumbsdown: captured | :thumbsup: error| 

The following error message appears when trying to get a screenshot with scrot on the X socket: 
````bash
]$ docker run --rm -e "DISPLAY:$DISPLAY" -v /tmp/.X11-unix:/tmp/.X11-unix -u 1000 screenshot > screenshot.png
X Error of failed request:  BadMatch (invalid parameter attributes)
  Major opcode of failed request:  73 (X_GetImage)
  Serial number of failed request:  23
  Current serial number in output stream:  23
cat: /tmp/screenshot.png: No such file or directory
Warning: Couldn't read data from file "/tmp/screenshot-", this makes an empty 
Warning: POST.
````
The sway documentation says that scrot is not working anyways on wayland. Grim also throws an error message, not sure why
````bash
[vetsch@euklid x-vs-wayland]$ docker run --rm  -e "DISPLAY:$DISPLAY" -v /tmp/.X11-unix:/tmp/.X11-unix -u 1000 -it  screenshot --entrypoint /bin/bash 
X Error of failed request:  BadMatch (invalid parameter attributes)
  Major opcode of failed request:  73 (X_GetImage)
  Serial number of failed request:  23
  Current serial number in output stream:  23
failed to create display
cat: /tmp/screenshot.png: No such file or directory
````



