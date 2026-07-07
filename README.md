# WatermarkRemover-AI-Revamped

**AI-Powered Watermark Removal Tool using Florence-2 and LaMA/IOPaint Models**

🇬🇧 English | 🇬🇷 Ελληνικά | 🇫🇷 Français | 🇨🇳 中文 | 🇧🇷 Português

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

A fork of [D-Ogi/WatermarkRemover-AI](https://github.com/D-Ogi/WatermarkRemover-AI), cleaned up and hardened for everyday use: startup crashes fixed, a proper Corporate UI, a choice of inpainting models, and no more novelty theming or joke copy anywhere in the app.

---

## Overview

`WatermarkRemover-AI-Revamped` uses AI models for precise watermark detection and seamless removal. It works well for removing watermarks from AI-generated video (Sora, Sora 2, Runway, and similar tools) as well as regular images and video.

Florence-2 (Microsoft) handles watermark detection, and an IOPaint inpainting model fills in the removed region naturally. The app has a GUI built with PyWebview, plus a full CLI for scripted or headless use.

---

## What's Different from Upstream

### Reliability fixes
- **Fixed a startup freeze/hang** - launching via `run.bat` (pythonw.exe, no console) could crash the print/log handler and leave the window "Not Responding." Output is now routed through a safe stream wrapper that can't raise.
- **Non-blocking CUDA/GPU detection** - detection used to run on the UI thread and freeze the window for several seconds on launch; it now runs in the background and the UI polls for the result.
- **Pinned a verified-working CUDA torch build** (`torch==2.6.0+cu124` / `torchvision==0.21.0+cu124`) - the previous open version range could silently resolve to a CPU-only wheel.

### New features
- **Inpainting model picker** - choose from all 8 IOPaint models (LaMa, LDM, ZITS, MAT, FcF, Manga, OpenCV, MI-GAN) instead of being locked to LaMa. Wired through both the GUI and the CLI (`--model`).
- **Full Greek localization** - Greek is now a complete, first-class translation alongside English.

### Cleanup
- **Removed all novelty UI themes** (Slay Queen, Sigma, WitchTok, Coquette, Windows XP, Anime, "Brainrot") - the app now ships with a single clean Corporate theme.
- **Removed the "Brainrot" language pack** and the scrolling marquee banner.
- **Removed the Japanese language pack entirely** - it contained coded language sexualizing minors, not just crude humor. This isn't part of the app in any form.
- **Rewrote the French, Chinese, and Portuguese translations from scratch** - the originals were slang/meme copy (crime jokes, absent-father jokes, etc.); all UI strings are now professional and consistent with the English and Greek versions.
- **Version reset to v2** and the window/app title cleaned up (no more "Ohio Edition").

---

## Features

- **Smart Detection** - AI-powered watermark detection using Florence-2
- **Choice of Inpainting Models** - pick the IOPaint model that fits your image (LaMa, LDM, ZITS, MAT, FcF, Manga, OpenCV, MI-GAN)
- **Seamless Removal** - natural-looking inpainting results
- **Video Support** - two-pass detection with audio preservation
- **AI Video Ready** - remove watermarks from Sora, Sora 2, Runway, and similar AI-generated video
- **Batch Processing** - handle entire folders at once
- **Preview Mode** - preview detected watermarks before processing
- **Fade In/Out Handling** - extend masks for watermarks that fade in/out
- **GPU Acceleration** - CUDA support for faster processing
- **Multi-Language UI** - English (default), Greek, French, Chinese, and Portuguese
- **Clean Corporate UI** - a single, professional theme

---

## Installation

### Windows

The setup script downloads a portable Python environment automatically - no system Python required.

```powershell
git clone https://github.com/SpyrosTheBoss/WatermarkRemover-AI-Revamped.git
cd WatermarkRemover-AI-Revamped
.\setup.ps1
```

After setup, double-click `run.bat` to launch the app.

### Linux / macOS

Requires Python 3.10+ installed on your system.

```bash
git clone https://github.com/SpyrosTheBoss/WatermarkRemover-AI-Revamped.git
cd WatermarkRemover-AI-Revamped
chmod +x setup.sh
./setup.sh
```

After setup, run `./run.sh` to launch the app.

### Optional: FFmpeg

Install FFmpeg to preserve audio when processing videos:
- **Windows**: Download from [ffmpeg.org](https://ffmpeg.org/download.html) and add to PATH
- **Linux**: `sudo apt install ffmpeg`
- **macOS**: `brew install ffmpeg`

---

## Usage

### GUI Mode

1. Run the app (`run.bat` on Windows, `./run.sh` on macOS/Linux)
2. Select your preferred language from the top-right corner
3. Select your mode (Single File or Batch)
4. Set input and output paths
5. Configure settings as needed, including the inpainting model
6. Hit **Start Processing**

Your settings are automatically saved and restored on next launch.

### CLI Mode

```bash
# Basic usage
python remwm.py input.png output_folder/

# With options
python remwm.py ./images ./output --overwrite --max-bbox-percent=15 --force-format=PNG

# Choose an inpainting model
python remwm.py input.png ./output --model=migan

# Process video with two-pass detection
python remwm.py video.mp4 ./output --detection-skip=3 --fade-in=0.5 --fade-out=0.5

# Preview mode (detect without processing)
python remwm.py input.png --preview
```

### CLI Options

| Option | Description |
|--------|-------------|
| `--overwrite` | Overwrite existing files |
| `--transparent` | Make watermark regions transparent (images only) |
| `--max-bbox-percent` | Max detection size as % of image (default: 10) |
| `--force-format` | Force output format (PNG, WEBP, JPG, MP4, AVI) |
| `--detection-prompt` | Custom detection prompt (default: "watermark") |
| `--detection-skip` | Detect every N frames for videos (1-10, default: 1) |
| `--fade-in` | Extend mask backwards by N seconds (for fade-in watermarks) |
| `--fade-out` | Extend mask forwards by N seconds (for fade-out watermarks) |
| `--model` | Inpainting model to use: lama, ldm, zits, mat, fcf, manga, cv2, migan (default: lama) |
| `--preview` | Preview detected watermarks without processing |

---

## Video Processing

- **Supported formats:** MP4, AVI, MOV, MKV, FLV, WMV, WEBM
- **Audio preservation:** Requires FFmpeg installed
- **Two-pass mode:** Faster processing with `--detection-skip` > 1
- **Fade handling:** Use `--fade-in` / `--fade-out` for watermarks that appear/disappear gradually

---

## Tech Stack

- **Florence-2** - Microsoft's vision model for watermark detection
- **IOPaint** - Inpainting model suite (LaMa, LDM, ZITS, MAT, FcF, Manga, OpenCV, MI-GAN)
- **PyWebview** - Cross-platform webview wrapper
- **Alpine.js** - Lightweight JavaScript framework for UI
- **PyTorch** - Deep learning backend

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
