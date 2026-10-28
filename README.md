This is a quick and kinda dirty script to batch convert video files via FFmpeg to play on the GP2X. It can be tweaked to work for other devices as well. With the default options, the script produces a 320x240 video in AVI format with XviD and MP3 being the video and audio codecs respectively. Video that is not 4:3 is letterboxed with either horizontal or vertical black bars.

There are some shortcomings of the script which exist because I don't really like writing shell scripts and am lazy:
- Parameters can be modified only via environment variables (see below). There are no command-line options.
- You can only pass files as arguments. Directories will break the script.
- Use of `nproc` and `mktemp` mean the script may not work on every platform.

The parameters you can change are as follows:

| Variable        | Default Value                                                          | Purpose                                                       |
|-----------------|------------------------------------------------------------------------|---------------------------------------------------------------|
| `FORMAT`        | `avi`                                                                  | Output file format                                            |
| `VID_CODEC`     | `libxvid`                                                              | Passed to `-c:v`                                              |
| `AUD_CODEC`     | `mp3`                                                                  | Passed to `-c:a`                                              |
| `VID_BITRATE`   | `300k`                                                                 | Passed to `-b:v`                                              |
| `AUD_BITRATE`   | `128k`                                                                 | Passed to `-b:a`                                              |
| `VID_FPS`       | `30`                                                                   | Passed to `-r`                                                |
| `AUD_SAMPLE_HZ` | `44100`                                                                | Passed to `ar`                                                |
| `AUD_CHANNELS`  | `2`                                                                    | Passed to `-ac`                                               |
| `VID_FILTER`    | `scale=320:240:force_original_aspect_ratio=decrease,pad=320:240:-1:-1` | Passed to `-vf`                                               |
| `AUD_FILTER`    | `aresample=async=$AUD_SAMPLE_HZ`                                       | Passed to `-af`                                               |
| `OVERWRITE`     | `n`                                                                    | Setting to `y` will overwrite output files if already present |
| `MAX_JOBS`      | `$(nproc)`                                                             | Max number of concurrent FFmpeg processes to run at a time    |

Examples of usage are as follows:

```bash
mp2xconv in1/* in2/*.mp4 in3/foo.mkv out
VID_CODEC=libx264 mp2xconv in.mp4 out
OVERWRITE=y MAX_JOBS=4 mp2xconv in/* out
```

Special thanks to Todespreis on the Pyra forums for providing the initial FFmpeg command from which this script evolved [here](https://pyra-handheld.com/boards/threads/video-audio-desync-in-homebrew-video-players.100247/post-1727082).
