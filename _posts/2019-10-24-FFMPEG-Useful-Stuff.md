---
layout: post
title: "FFMPEG Gems"
categories:
  - guide
tags:
  - guide
---
**Create a video from a single image**
```
ffmpeg -loop 1 -i img.jpg -c:v libx264 -t 30 -pix_fmt yuv420p out.mp4
```

**Create a video from multiple images:**

Order your images manually and specify a duration in a text file
```
file '/path/to/dog.png'
duration 5
file '/path/to/cat.png'
duration 1
file '/path/to/rat.png'
duration 3
file '/path/to/tapeworm.png'
duration 2
file '/path/to/tapeworm.png'
```
(Due to a quirk, the last image has to be specified twice - the 2nd time without any duration directive)
Then run ffmpeg command
```
ffmpeg -f concat -i input.txt -vsync vfr -pix_fmt yuv420p output.mp4
```

**Merge audio and video with re-encoding:**
```
ffmpeg -i video.mp4 -i audio.wav -c:v copy -c:a aac -strict experimental output.mp4
```
**References:**
- https://trac.ffmpeg.org/wiki/Slideshow#Concat%20demuxer
