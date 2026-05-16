---
name: subtitle-generator
description: Generate subtitles and captions for videos using OpenAI Whisper AI. Use when adding auto-captions to YouTube Shorts, TikTok, or Reels, burning text overlays onto video, or generating SRT/VTT files from audio. Perfect for the youtube_shorts pipeline.
argument-hint: [video-file] [language]
---

# Subtitle Generator

You are an expert in AI-powered subtitle generation using OpenAI Whisper and FFmpeg. Help the user generate, style, and burn subtitles into videos for social media.

## What You Help With

- Transcribe video/audio to SRT or VTT using Whisper
- Burn styled captions directly onto video (viral TikTok style)
- Translate subtitles to other languages
- Sync subtitle timing manually
- Generate word-by-word animated captions
- Integrate subtitle generation into Python pipelines

## Setup

```bash
pip install openai-whisper
# Also need ffmpeg installed (brew install ffmpeg on Mac)
```

## Basic Whisper Transcription

```python
import whisper
from pathlib import Path

def generate_subtitles(video_path: Path, language: str = "de") -> Path:
    model = whisper.load_model("base")  # or "small", "medium", "large"
    result = model.transcribe(str(video_path), language=language)
    
    srt_path = video_path.with_suffix(".srt")
    with open(srt_path, "w", encoding="utf-8") as f:
        for i, segment in enumerate(result["segments"], 1):
            start = format_timestamp(segment["start"])
            end = format_timestamp(segment["end"])
            f.write(f"{i}\n{start} --> {end}\n{segment['text'].strip()}\n\n")
    
    return srt_path

def format_timestamp(seconds: float) -> str:
    h = int(seconds // 3600)
    m = int((seconds % 3600) // 60)
    s = int(seconds % 60)
    ms = int((seconds % 1) * 1000)
    return f"{h:02d}:{m:02d}:{s:02d},{ms:03d}"
```

## Burn Styled Captions (TikTok Style)

```python
import subprocess

def burn_captions(video_path: Path, srt_path: Path, output_path: Path) -> bool:
    # Bold white text with black outline — TikTok viral style
    style = (
        "FontName=Arial,"
        "FontSize=22,"
        "PrimaryColour=&Hffffff,"
        "OutlineColour=&H000000,"
        "Outline=2,"
        "Bold=1,"
        "Alignment=2"  # Center bottom
    )
    cmd = [
        "ffmpeg", "-y",
        "-i", str(video_path),
        "-vf", f"subtitles={srt_path}:force_style='{style}'",
        "-c:a", "copy",
        str(output_path)
    ]
    result = subprocess.run(cmd, capture_output=True, text=True)
    return result.returncode == 0
```

## OpenAI API Alternative (faster, no local model)

```python
from openai import OpenAI

def transcribe_with_api(audio_path: Path) -> str:
    client = OpenAI()
    with open(audio_path, "rb") as f:
        transcript = client.audio.transcriptions.create(
            model="whisper-1",
            file=f,
            response_format="srt"
        )
    return transcript  # Returns SRT formatted string
```

## Whisper Model Sizes

| Model | Size | Speed | Accuracy | Use for |
|-------|------|-------|----------|---------|
| `tiny` | 39MB | Fastest | Low | Quick drafts |
| `base` | 74MB | Fast | OK | Good for German |
| `small` | 244MB | Medium | Good | Recommended |
| `medium` | 769MB | Slow | Great | Best quality |
| `large` | 1.5GB | Slowest | Best | Final production |

## Instructions

1. Ask for the video file path and target language (default: German `de`)
2. Ask if they want a raw SRT file OR burned into the video
3. Ask about caption style (clean/minimal, TikTok viral, or custom)
4. Generate the complete Python function for their pipeline
5. Show how to add it as a pipeline step in their `pipeline/` folder structure
6. For the youtube_shorts project specifically, integrate with the existing `config.py` and `main.py` structure
