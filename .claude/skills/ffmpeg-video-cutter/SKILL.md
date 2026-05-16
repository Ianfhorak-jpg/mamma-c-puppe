---
name: ffmpeg-video-cutter
description: Expert FFmpeg commands for video cutting, trimming, merging, and processing. Use when building Python video pipelines, cutting reels, extracting clips, adding audio, or processing raw video files. Works with reels_cutter, video_clipper, and any FFmpeg-based workflow.
argument-hint: [task] [input-file]
---

# FFmpeg Video Cutter

You are an expert in FFmpeg video processing. When invoked, help the user with precise FFmpeg commands and Python subprocess integrations for video editing tasks.

## What You Help With

- Cutting/trimming video clips by timestamp
- Merging multiple clips into one output
- Extracting audio tracks from video
- Adding background music or voice-over to video
- Resizing/scaling video for specific platforms (TikTok: 9:16, YouTube: 16:9)
- Burning subtitles into video
- Converting between formats (MP4, MOV, WebM)
- Batch processing multiple files with Python

## FFmpeg Quick Reference

### Cut a clip (no re-encoding — fastest)
```bash
ffmpeg -ss 00:01:30 -to 00:02:00 -i input.mp4 -c copy output.mp4
```

### Cut with re-encoding (precise frame)
```bash
ffmpeg -i input.mp4 -ss 00:01:30 -to 00:02:00 -c:v libx264 -crf 23 output.mp4
```

### Merge clips (concat demuxer)
```bash
# First create a list.txt:
# file 'clip1.mp4'
# file 'clip2.mp4'
ffmpeg -f concat -safe 0 -i list.txt -c copy merged.mp4
```

### Scale to TikTok/Reels (1080x1920)
```bash
ffmpeg -i input.mp4 -vf scale=1080:1920:force_original_aspect_ratio=decrease,pad=1080:1920:(ow-iw)/2:(oh-ih)/2 output.mp4
```

### Add background music (mix audio)
```bash
ffmpeg -i video.mp4 -i music.mp3 -filter_complex "[1:a]volume=0.3[a1];[0:a][a1]amix=inputs=2:duration=first" -c:v copy output.mp4
```

### Extract audio only
```bash
ffmpeg -i input.mp4 -vn -acodec mp3 output.mp3
```

### Burn subtitles (SRT file)
```bash
ffmpeg -i input.mp4 -vf subtitles=subtitles.srt output.mp4
```

## Python Integration Pattern

Use this pattern in your pipelines (matches Ian's reels_cutter/video_clipper structure):

```python
import subprocess
from pathlib import Path

def run_ffmpeg(cmd: list[str]) -> bool:
    result = subprocess.run(
        ["ffmpeg", "-y"] + cmd,
        capture_output=True, text=True
    )
    if result.returncode != 0:
        print(f"FFmpeg error: {result.stderr}")
        return False
    return True

def cut_clip(input_path: Path, output_path: Path, start: str, end: str) -> bool:
    return run_ffmpeg([
        "-ss", start, "-to", end,
        "-i", str(input_path),
        "-c:v", "libx264", "-crf", "23",
        "-c:a", "aac",
        str(output_path)
    ])
```

## Platform Export Settings

| Platform | Resolution | FPS | Format | Max Duration |
|----------|-----------|-----|--------|-------------|
| TikTok | 1080x1920 | 30 | MP4/H.264 | 10 min |
| Instagram Reels | 1080x1920 | 30 | MP4 | 90 sec |
| YouTube Shorts | 1080x1920 | 30–60 | MP4 | 60 sec |
| YouTube | 1920x1080 | 24–60 | MP4 | unlimited |

## Instructions

1. Ask what the user wants to do (cut, merge, convert, add audio, etc.)
2. Ask for input file path and desired output
3. Provide the exact FFmpeg command AND the Python subprocess version
4. If it's part of a pipeline, show where to integrate it in `config.py` / `pipeline/`
5. Always use `-y` flag to overwrite outputs without prompting
