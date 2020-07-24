# v4l2loopback
- https://github.com/umlaeute/v4l2loopback.git

# v4l2tools
- https://github.com/mpromonet/v4l2tools

# Install tools
- sudo apt install v4l-utils

# How to enable videodev in sandbox?
- sudo apt-get install linux-generic
- reboot sandbox
- sudo modprobe videodev
- cd v4l2loopback
- sudo make
- sudo make install
- sudo depmod -a

# Launch four virtual v4l2 interface devices (/dev/video0 ~ /dev/video3)
- sudo modprobe v4l2loopback devices=4

# Build v4l2tools
- sudo apt-get install liblog4cpp5-dev libvpx-dev libx264-dev libx265-dev libjpeg-dev
- cd v4l2tools
- make

# Build capture code for obtaining h264 frames
- g++ capture.c -o capture

# input video to loopback 
- sudo ffmpeg -re -i input.mp4 -map 0:v -f v4l2 /dev/video1

# convert YU12 video to H264
- ./v4l2compress -f H264 /dev/video1 /dev/video0

# check video format
- sudo v4l2-ctl --device /dev/video0 --all

# run h264 frame capture
- ./capture -f -o

# check frame is correct (should choose an I-frame)
- ffmpeg -loglevel trace  -f h264 -i frame-73.raw  frame-73.jpg

