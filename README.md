# X vs Wayland

To restrict the attack surface for all the software I run on my computer, I try to lock them up in containers. 
Unfortunately, when they have a GUI I have to mount the Xorg socket into the container. Access to that socket allows allows application to 
* Log Keyboard Events
* Create screenshots of all displays
* Access the clipboard
* (Unsure) send keyboard events to other windows. 

Wayland is an alternative Xorg thing. I tried to figure out if those things are more restrictive there. 
I compared the behaviour on two different devices: 
* Fedora -> Xorg -> i3wm
* Fedora -> Wayland -> sway

I did not check if there is any configuration option for Wayland nor did I read any documentation. 

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

It seems that mounting XDG runtime dir changes the error message, but grim is still not able to do a screenshot

````bash
[vetsch@euklid x-vs-wayland]$ docker run --rm  -e "DISPLAY:$DISPLAY" -v /tmp/.X11-unix:/tmp/.X11-unix -v /run/user/1000:/run/user/1000 -u 1000 -it  screenshot
X Error of failed request:  BadMatch (invalid parameter attributes)
  Major opcode of failed request:  73 (X_GetImage)
  Serial number of failed request:  23
  Current serial number in output stream:  23
wl_registry@2: error 0: invalid version for global wl_output (12): have 2, wanted 3
warning: zxdg_output_manager_v1 isn't available, guessing the output layout
compositor doesn't support wlr-screencopy-unstable-v1
cat: /tmp/screenshot.png: No such file or directory

````

## Clipboard
````bash
docker build -t clipboard clipboard
echo somedata | xclip -i 
wl-copy "sway is an i3-compatible Wayland compositor."
docker run --rm  -e "DISPLAY:$DISPLAY" -v /tmp/.X11-unix:/tmp/.X11-unix -u 1000 -it clipboard
````

|Test| Xorg	| Wayland | 
|----|-------|---------|
| read clipboard  | :thumbsdown: captured | :thumbsdown: captured | 
| write to clipboard | untested | untested| 

Works perfectly on sway: 
````bash
docker run --rm  -e "DISPLAY:$DISPLAY" -v /tmp/.X11-unix:/tmp/.X11-unix -u 1000 -it  clipboard
sway is an i3-compatible Wayland compositor.
````


## Things to test
* Sending KeyPress events. (Trying to break out by sending keyboard events to open a terminal and run code , Rubber Ducky like) 
