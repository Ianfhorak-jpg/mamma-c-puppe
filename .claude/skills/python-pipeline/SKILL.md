---
name: python-pipeline
description: Scaffold and build Python processing pipelines with config, logging, error handling, and modular structure. Use when starting a new Python automation project, adding stages to an existing pipeline, or structuring code like the reels_cutter, video_clipper, or youtube_shorts projects.
argument-hint: [pipeline-name] [stages]
---

# Python Pipeline Builder

You are an expert in building clean, production-grade Python automation pipelines. Generate well-structured code that matches Ian's established project pattern.

## Ian's Standard Pipeline Structure

All of Ian's Python projects follow this consistent structure:

```
project_name/
├── main.py              ← Entry point, orchestrates pipeline stages
├── config.py            ← All configuration (paths, API keys, settings)
├── requirements.txt     ← All dependencies pinned
├── pipeline/
│   ├── __init__.py
│   ├── stage_01_input.py
│   ├── stage_02_process.py
│   └── stage_03_output.py
├── utils/
│   ├── __init__.py
│   ├── logger.py
│   └── helpers.py
├── assets/              ← Input assets (videos, images, audio)
├── output/              ← Processed results
└── logs/                ← Log files
```

## config.py Template

```python
from pathlib import Path

# Project root
BASE_DIR = Path(__file__).parent

# Paths
INPUT_DIR = BASE_DIR / "assets"
OUTPUT_DIR = BASE_DIR / "output"
LOG_DIR = BASE_DIR / "logs"

# Create dirs if they don't exist
for d in [INPUT_DIR, OUTPUT_DIR, LOG_DIR]:
    d.mkdir(exist_ok=True)

# API Keys (load from environment)
import os
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY", "")

# Pipeline settings
SETTINGS = {
    "quality": "high",
    "language": "de",
    "max_retries": 3,
}
```

## Logger Template (utils/logger.py)

```python
import logging
from pathlib import Path
from datetime import datetime

def setup_logger(name: str, log_dir: Path) -> logging.Logger:
    logger = logging.getLogger(name)
    logger.setLevel(logging.INFO)
    
    formatter = logging.Formatter(
        "%(asctime)s | %(levelname)s | %(message)s",
        datefmt="%Y-%m-%d %H:%M:%S"
    )
    
    # Console output
    console = logging.StreamHandler()
    console.setFormatter(formatter)
    logger.addHandler(console)
    
    # File output
    log_file = log_dir / f"{datetime.now():%Y-%m-%d}.log"
    file_handler = logging.FileHandler(log_file)
    file_handler.setFormatter(formatter)
    logger.addHandler(file_handler)
    
    return logger
```

## main.py Template

```python
#!/usr/bin/env python3
"""
[Pipeline Name] - [One sentence description]
"""
import sys
from pathlib import Path
from config import INPUT_DIR, OUTPUT_DIR, LOG_DIR, SETTINGS
from utils.logger import setup_logger
from pipeline.stage_01_input import collect_inputs
from pipeline.stage_02_process import process_items
from pipeline.stage_03_output import export_results

logger = setup_logger("pipeline", LOG_DIR)

def run():
    logger.info("=== Pipeline started ===")
    
    try:
        # Stage 1: Collect
        logger.info("Stage 1: Collecting inputs...")
        items = collect_inputs(INPUT_DIR)
        logger.info(f"Found {len(items)} items to process")
        
        # Stage 2: Process
        logger.info("Stage 2: Processing...")
        results = process_items(items, SETTINGS)
        
        # Stage 3: Export
        logger.info("Stage 3: Exporting...")
        export_results(results, OUTPUT_DIR)
        
        logger.info(f"=== Done! {len(results)} items processed ===")
        
    except KeyboardInterrupt:
        logger.info("Pipeline interrupted by user")
        sys.exit(0)
    except Exception as e:
        logger.error(f"Pipeline failed: {e}", exc_info=True)
        sys.exit(1)

if __name__ == "__main__":
    run()
```

## Stage Template (pipeline/stage_01_input.py)

```python
from pathlib import Path
import logging

logger = logging.getLogger("pipeline")

def collect_inputs(input_dir: Path) -> list[Path]:
    """Collect all processable files from input directory."""
    supported = {".mp4", ".mov", ".avi", ".mkv"}
    files = [f for f in input_dir.iterdir() if f.suffix.lower() in supported]
    
    if not files:
        logger.warning(f"No files found in {input_dir}")
        return []
    
    logger.info(f"Collected: {[f.name for f in files]}")
    return sorted(files)
```

## requirements.txt Template

```
# Core
python-dotenv>=1.0.0
pathlib>=1.0.1

# Add project-specific below:
openai>=1.30.0
ffmpeg-python>=0.2.0
```

## Instructions

1. Ask what the pipeline does (input type, processing steps, output type)
2. Ask how many stages it needs
3. Generate ALL files: `main.py`, `config.py`, `requirements.txt`, and each stage file
4. Match the naming convention of Ian's existing projects (`stage_01_`, `stage_02_`, etc.)
5. Always include proper logging in every stage
6. Never use `print()` — always use `logger.info()` / `logger.error()`
