---
layout: post
title: "FFMPEG Gems"
categories:
  - guide
tags:
  - guide, ffmpeg, gems
---
A constantly updating set of useful ffmpeg commands. If there are any commands that you find yourself using quite commonly with ffmpeg, please share.

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
ffmpeg -i in.text -i audio.mp3 -c:a copy out.mp4
```

**Merge audio and video with re-encoding:**
```
ffmpeg -i video.mp4 -i audio.wav -c:v copy -c:a aac -strict experimental output.mp4
```

**Concatenating audio files**
```
ffmpeg -i "concat:audio1.aac|audio2.aac" -c copy result.aac
```

**Convert audio file to mp3**
```
ffmpeg -i file file.mp3
```
**Clipping audio files**
```
ffmpeg -ss 10 -t 6 -i input.mp3 output.mp3
```
`-ss` flag is for the starting point in the audio file

`-t` is the time from the audio spot until which you want to clip

So in this example we are clipping starting from 10 seconds until 16 seconds.

**References:**
- https://trac.ffmpeg.org/wiki/Slideshow#Concat%20demuxer
