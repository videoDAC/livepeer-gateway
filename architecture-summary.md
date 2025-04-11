## Architecture Summary

This section provides a high level summary of the logical and functional architecture of a Livepeer Gateway.

![Image](https://github.com/user-attachments/assets/f27a62b4-22c0-4b38-b63c-53979d843387)

Content can be published to Livepeer Gateway via `RTMP` to port `1935`.

Content can be consumed by requesting a URL with `.m3u8` extension, via `http` from port `9935`.

Livepeer Gateway will respond by serving a sequence of content segment files with `.hs` extensions over `http`, for playback.

The code for this software is available on [Livepeer's go-livepeer repository](https://github.com/livepeer/go-livepeer).

[Return to main page](./README.md#next-steps)
