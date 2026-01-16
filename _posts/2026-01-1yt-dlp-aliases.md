---
title: YT-DLP aliases
date: 2026-01-16
categories: [TERMINAL, TIPS]
tags: [quality-of-life]     # TAG names should always be lowercase
---

Aliases for yt-dlp for 720p, 1080p and 4k. You can use them on Windows as well, just put yt-dlp in PATH and use GitBash or WSL. File goes to Documents, mp4 format.

```bash
# yt-dlp 1080p
alias ytdl1080='f(){ yt-dlp -f "bestvideo[height<=1080][ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best" -o "~/Documents/%(title)s.%(ext)s" "$1"; unset -f f; }; f'

# yt-dlp 720p
alias ytdl720='f(){ yt-dlp -f "bestvideo[height<=720][ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best" -o "~/Documents/%(title)s.%(ext)s" "$1"; unset -f f; }; f'

# yt-dlp 4k
alias ytdl4k='f(){ yt-dlp -f "bestvideo[height<=2160][ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best" -o "~/Documents/%(title)s.%(ext)s" "$1"; unset -f f; }; f'
```

For powershell

```powershell
function ytdl1080($url) { yt-dlp -f "bestvideo[height<=1080][ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best" -o "$HOME\Documents\%(title)s.%(ext)s" $url }
function ytdl720($url) { yt-dlp -f "bestvideo[height<=720][ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best" -o "$HOME\Documents\%(title)s.%(ext)s" $url }
function ytdl4k($url) { yt-dlp -f "bestvideo[height<=2160][ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best" -o "$HOME\Documents\%(title)s.%(ext)s" $url }
```

# Usage

```bash
ytdl1080 "https://www.youtube.com/watch?v=example"
```