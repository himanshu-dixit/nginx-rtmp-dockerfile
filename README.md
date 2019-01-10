NGINX RTMP Dockerfile
=====================

This Dockerfile installs NGINX configured with `nginx-rtmp-module`, ffmpeg
and some default settings for HLS live streaming.

**Note: in the current state, this is just an experimental project to play with
RTMP and HLS.**


How to use
----------

1. Build and run the container (`docker build -t nginx_rtmp .` &
   `docker run -p 1935:1935 -p 8080:80 -v $(pwd)/nginx.conf:/config/nginx.conf -it --rm nginx_rtmp`).

2. Stream your live content to `rtmp://localhost:1935/encoder/stream_name` where
   `stream_name` is the name of your stream. For instance you can use ffmpeg with a local file: `ffmpeg -stream_loop 1 -i /path/to/bunny_1080p_60fps.mp4 -c:v libx264 -x264opts keyint=30:min-keyint=30:scenecut=-1 -tune zerolatency -s 1280x720 -b:v 1400k -bufsize 1400k -f flv rtmp://localhost:1935/encoder/stream_name`

3. In Safari, VLC or any HLS compatible browser / player, open
   `http://localhost:8080/hls/stream_name.m3u8` or the mpeg-dash version of it `http://localhost:8080/dash/stream_name_360p878kbs/index.mpd`.
   Note that the first time, it might take a few (10-15) seconds before the stream works. This is because
   when you start streaming to the server, it needs to generate the first
   segments and the related playlists.


Links
-----

* http://nginx.org/
* https://github.com/arut/nginx-rtmp-module
* https://www.ffmpeg.org/
* https://obsproject.com/
