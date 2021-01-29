# ffmpeg

## 4.3.1

### Trim

```bash
ffmpeg -i input.mp4 -ss 00:02:22 -to 00:03:33 -c copy out.mp4
```

### Scale

```bash
ffmpeg -i input.mp4 -vf scale=1920:1080 out.mp4
```

### Change FPS

```bash
ffmpeg -i input.mp4 -vf fps=25 out.mp4
```
