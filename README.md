### Custom mediaMTX

# Ports changed on .yml
(default_port --> new_port)
tcp6       0      0 [::]:8554 --> 8555
tcp6       0      0 [::]:8888 --> 8886
tcp6       0      0 [::]:8887 --> 8885   
tcp6       0      0 [::]:9996 --> 9995   
tcp6       0      0 [::]:1935 --> 1934   
udp6       0      0 [::]:8000 --> 8002
udp6       0      0 [::]:8001 --> 8003
udp6       0      0 [::]:8189 --> 8187
udp6       0      0 [::]:8890 --> 8889
Api                     :9997 --> 9994

# To make a service:
```
sudo nano /etc/systemd/system/mediamtx.service
```

```
[Unit]
Description=MediaMTX Service
After=network.target

[Service]
ExecStart=/home/jbalao/mediamtx_v1.6.0_linux_amd64/mediamtx /home/jbalao/mediamtx_v1.6.0_linux_amd64/mediamtx.yml
Restart=on-failure
User=jbalao
Group=jbalao
WorkingDirectory=/home/jbalao
StandardOutput=journal
StandardError=journal
SyslogIdentifier=mediamtx

[Install]
WantedBy=multi-user.target
```

Reload systemd to apply the changes:
```
sudo systemctl daemon-reload
```

Enable the service to start automatically on boot:
```
sudo systemctl enable mediamtx.service
```

Start the service:
```
sudo systemctl start mediamtx.service
```

# To convert .mp4 to raw .h264 file:
gst-launch-1.0 filesrc location=video.mp4 ! qtdemux name=demux demux.video_0 ! queue ! decodebin ! x264enc ! h264parse ! filesink location=video.h264