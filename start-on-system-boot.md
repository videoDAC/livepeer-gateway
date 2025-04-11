## Start on system boot

This section explains how to configure Livepeer Gateway to start when the underlying system starts.

The instructions use `systemd` on Linux (Ubuntu), and require root / `sudo` access. 

[Return to main page](./README.md#next-steps)

### `livepeer-gateway.service`

0. Ensure the Livepeer Gateway has been [downloaded and installed on the underlying system](#minimum-setup)

1. Run the following commands to fetch a `livepeer-gateway.service` file from `/etc/systemd/system` folder
```
cd /etc/systemd/system
sudo wget https://raw.githubusercontent.com/videoDAC/livepeer-gateway/master/systemd/livepeer-gateway.service
```

2. Run the following commands to enable and start `livepeer-gateway.service`:
```
sudo systemctl enable livepeer-gateway.service
sudo systemctl start livepeer-gateway.service
```

**Livepeer Gateway is now running.**

3. Run the following command to tail the logs:
```
sudo journalctl -f --unit=livepeer-gateway.service
```

Note: by default, the Livepeer Gateway will only accept inbound `rtmp` content and serve outbound `http` content _from and to localhost_ (`127.0.0.1`). To open ports to remote hosts, change the corresponding configuration to `0.0.0.0` and run `sudo systemctl daemon-reload`, then `sudo systemctl restart livepeer-gateway.service`.

[Return to main page](./README.md#next-steps)

### `publish-test-source.service`

0. Ensure the `FFmpeg` has been [downloaded and installed on the underlying system](#minimum-setup)

1. Run the following commands to fetch a `publish-test-source.service` file from `/etc/systemd/system` folder
```
cd /etc/systemd/system
sudo wget https://raw.githubusercontent.com/videoDAC/livepeer-gateway/master/systemd/publish-test-source.service
```

2. Run the following commands to enable and start `publish-test-source.service`:
```
sudo systemctl enable publish-test-source.service
sudo systemctl start publish-test-source.service
```

**a test source is now being published into Livepeer Gateway**

3. Run the following command to tail the logs:
```
sudo journalctl -f --unit=publish-test-source.service
```

4. Run `curl http://127.0.0.1:8935/stream/hello_consumer.m3u8` to see
```
#EXTM3U
#EXT-X-VERSION:3
#EXT-X-STREAM-INF:PROGRAM-ID=0,BANDWIDTH=4000000,RESOLUTION=1000x1000
hello_consumer/source.m3u8
```

[Return to main page](./README.md#next-steps)

### `local-transcoding.service`

0. Ensure the Livepeer software has been [downloaded and installed on the underlying system](#minimum-setup)

1. Run the following commands to fetch a `local-transcoding.service`:
```
cd /etc/systemd/system
sudo wget https://raw.githubusercontent.com/videoDAC/livepeer-gateway/master/systemd/local-transcoding.service
```

2. Run the following commands to enable and start `local-transcoding.service`:
```
sudo systemctl enable local-transcoding.service
sudo systemctl start local-transcoding.service
```

**Livepeer Gateway is now running with local transcoding service.**

3. Run the following command to tail the logs:
```
sudo journalctl -f --unit=local-transcoding.service
```

4. Ensure `publish-test-source.service` is running, then run `curl http://0.0.0.0:8935/stream/hello_consumer.m3u8` to see
```
#EXTM3U
#EXT-X-VERSION:3
#EXT-X-STREAM-INF:PROGRAM-ID=0,BANDWIDTH=4000000,RESOLUTION=1000x1000
hello_consumer/source.m3u8
#EXT-X-STREAM-INF:PROGRAM-ID=0,BANDWIDTH=400000,RESOLUTION=256x144
hello_consumer/P144p30fps16x9.m3u8
#EXT-X-STREAM-INF:PROGRAM-ID=0,BANDWIDTH=600000,RESOLUTION=426x240
hello_consumer/P240p30fps16x9.m3u8
```

[Return to main page](./README.md#next-steps)
