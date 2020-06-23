# ffmpeg

### Generate a clip

`ffmpeg -y --ss start_time -to end_time -i input_file output_file`

start\_time / end\_time format: 0:01:12.123 \(1min 12 seconds 123ms\)

-y: skip confirmation, can override file.

### Merge clips

`ffmpeg -y -f concat -safe 0 -i files.txt -c copy output_file`

```bash
# files.txt
file ./clip1.mp4
file ./clip2.mp4
```

