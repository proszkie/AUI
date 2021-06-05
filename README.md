# Adaptive Streaming with nginx and ffmpeg

## What is that
A project that use nginx and ffmpeg for adaptive streaming. Users of it can stream a video to the server and watch it live. 
Most of it is taken from this project:
https://github.com/alfg/docker-nginx-rtmp

However it didn't work for us and we needed to make some changes.

## How to run the project

### Prerequisites
1. Docker has to be installed on the machine
2. Machine has to have available GPUs and appropriate driver installed (for sure it works for CUDA version 11.2 and Driver Version 460.80)

If above is fulfilled, one has to build the image:
```
docker build . -t adaptive-streaming
```

and then run it:

```
docker run --name adaptive-streaming --rm --gpus all -p 1936:1935 -p 8080:80 adaptive-streaming
```

To stream a video following url has to be provided:

rtmp://<IP-ADDRESS>:1936/stream

and stream key should be specified.

Stream can be watched then at:
http://<IP-ADDRESS>:8080/player.html?url=http://<IP-ADDRESS>:8080/live/<STREAM-KEY>.m3u8

## Troubleshooting
In case when something goes wrong (video cannot be streamed or watched) it is worth to enter the container:
```
docker exec -it adaptive-streaming bash
```

and look for error from ffmpeg:
```
cat ffmpeg_error_log.txt
```

It's worth knowing that data should appear under /opt/data/hls/<STREAM-KEY>
