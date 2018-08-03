## Example Setup for Streaming: ARTIK Live Webcam

For this example, ffmpeg and the WebSocket relay run on the same system. This allows you to view the stream in your local network, but not on the public internet.

This example assumes that your webcam is compatible with Video4Linux2 and appears as `/dev/video6` in the filesystem. 
(https://developer.artik.io/documentation/developer-guide/multimedia/camera-input-guide-a10.html)

1) Install the Node.js packages:
`npm install`

2) Start the Websocket relay. Provide a password and a port for the incomming HTTP video stream and a Websocket port that we can connect to in the browser:
`node websocket-relay.js supersecret 8081 8082`

3) Open the streaming website in your browser. 
`http://192.168.[...]`

4) In a second terminal window, start ffmpeg to capture the webcam video and send it to the Websocket relay. Provide the password and port (from step 7) in the destination URL:
```
ffmpeg \
	-f v4l2 \
		-framerate 25 -video_size 640x480 -i /dev/video6 \
	-f mpegts \
		-codec:v mpeg1video -s 640x480 -b:v 1000k -bf 0 \
	http://localhost:8081/supersecret
```

## Links

https://github.com/phoboslab/jsmpeg


