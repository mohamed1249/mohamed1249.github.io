---
title: "Tool: Phone Video Compressor"
date: 2026-05-24
status: "Completed"
tools:
  - Python
  - ADB
  - FFmpeg
  - ffmpeg-python
  - PyInstaller
  - Windows CLI
github: "https://github.com/mohamed1249/phone-video-compressor"
demo:
kaggle:
excerpt: "A Windows command-line utility that scans an Android phone for large videos, compresses them on a laptop with FFmpeg, and safely replaces the originals through a two-step ADB workflow."
---

## The Short Version

Phone Video Compressor is a practical desktop utility for people who record large videos on Android and need to free up phone storage without manually copying, compressing, and replacing files one by one.

It connects to the phone through ADB, finds videos above a chosen size threshold, copies them to the laptop, compresses them with FFmpeg, then optionally pushes the compressed versions back to the original phone locations.

## Problem

Phone videos can become huge quickly, especially when recording in high resolution. The manual workflow is annoying and risky:

- find the large videos,
- copy them to a laptop,
- compress them correctly,
- check the results,
- move them back,
- delete the original files,
- refresh the phone gallery.

The project turns that process into a controlled two-step tool so the user can compress first, inspect the output, and only then decide whether to replace the originals.

## Approach

**ADB integration** - The tool uses Android Debug Bridge to detect the connected phone, verify USB debugging authorization, scan common media folders, pull selected videos, push compressed files back, and refresh the gallery.

**FFmpeg compression** - Videos are compressed through FFmpeg with encoder choices for both GPU and CPU workflows:

- `hevc_nvenc` for NVIDIA GPU H.265 compression.
- `h264_nvenc` for faster GPU H.264 output.
- `libx265` for CPU H.265 compression.
- `libx264` for CPU H.264 compression.

**User-controlled settings** - The program asks for an output folder, minimum file size, encoder, quality level, and speed preset. It also checks whether the selected GPU encoder is available and falls back to CPU compression when needed.

**Two-step safety design** - Step 1 copies and compresses videos. Step 2 replaces the originals on the phone. Keeping these separate is the important product decision: it gives the user a chance to review compressed files before deleting anything.

**Executable packaging** - The project includes a `build.bat` and PyInstaller spec so the script can be distributed as a single `PhoneVideoCompressor.exe`. Users still need ADB and FFmpeg installed separately.

## Result

The finished tool can:

- scan Android phone folders such as `DCIM`, `Movies`, `Download`, `Pictures`, and WhatsApp videos,
- filter videos by size,
- compress large files with a selected encoder and quality level,
- save compressed files with `_compressed` in the name,
- push compressed versions back to the phone,
- delete originals only in the replacement step,
- refresh the gallery so the phone sees the updated files.

The README also explains setup for non-developer users: installing ADB, installing FFmpeg, enabling USB debugging, and running the packaged executable.

## What I Learned

This project is less about machine learning and more about building a useful local automation tool around real constraints. The hardest part is not just compressing a video; it is making the workflow safe enough that a person can trust it with personal media.

I also learned the value of dependency checks and fallbacks. A tool like this has to explain missing ADB, missing FFmpeg, unauthorized phones, unavailable GPU encoders, and quality tradeoffs in plain language.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
