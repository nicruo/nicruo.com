+++ 
date = 2024-01-15T14:15:00+00:00
title = "Optimizing Videos in PowerPoint Presentations with FFmpeg Commands"
+++

A few days ago, I was preparing a PowerPoint presentation that included some videos. These are the following ffmpeg commands that I used to optimize the presentation.

## Remove audio track

```bash
ffmpeg -i input.mp4 -c copy -an output.mp4
```

Note: May change video duration. I don't know why yet.

## Trim video

```bash
ffmpeg -i input.mp4 -c copy -ss 00:00:01.000 -to 00:00:04.000 output.mp4
```

Note: Check the trim result, sometimes I had to adjust the starting point to be close to perfect.

## Merge videos

Merge multiple videos in sequence:
```bash
ffmpeg -f concat -safe 0 -i join.txt -c copy output.mp4
```

Where the join.txt contains:
```bash
file input1.txt
file input2.txt
```

## Convert from avi to mp4

```bash
ffmpeg -i input.avi -c:v copy output.mp4
```