# Lipsync Studio
 
A browser-based lip-sync animation tool for 2D characters. Upload character art, provide audio and a phonetic transcript, and the tool generates a frame-accurate mouth animation timeline — no plugins, no server, no install.
 
**[Live Demo](https://lipsyncstudio.netlify.app)**
 
---
 
## Features
 
**Audio analysis**
- Energy-based voiced/unvoiced detection with local adaptive thresholding
- Configurable sensitivity slider (1–10) for different recording conditions
- Waveform visualisation
**Language modes**
- **Japanese** — romaji syllable input (space-separated), aiueo vowel mapping
- **English** — plain word input or CMU Arpabet phoneme input (auto-detected), 8 additional phoneme shapes (ə, æ, ʊ, ɔ, w, f, th, m), liaison/connected speech fill
- **Universal** — voiced/silent binary, suitable for any language
**Poetry mode** (Japanese/English)
- Word-internal segments connect end-to-end with no idle gap
- Adjustable minimum segment duration slider
**Timeline editor**
- Per-frame drag-to-resize editing
- Zoom (Ctrl+scroll or +/− keys), horizontal scroll
- Emotion track with 5 presets (happy, sad, bored, nervous, custom) and physics-based animation
- Sticker track (up to 8 frames, adjustable fps) mutually exclusive with blink
- Subtitle track with per-line visibility toggle and two alignment modes (even split or romaji-line)
**Multi-character mode**
- Two independent characters, each with position, scale, and layer controls
- Per-line language override in transcript (`[1:jp]`, `[2:en]`)
**Export**
- WebM (VP9), 2.5 Mbps
- Transparent background (VP9 alpha channel)
- Requires Chrome or Edge (Safari: editing supported, export unsupported)
---
 
## Usage
 
### 1. Prepare assets
 
Organise mouth shape images into a folder with the following filenames:
 
| Filename | Mouth shape |
|----------|-------------|
| `idle` | Closed / resting |
| `a` `i` `u` `e` `o` | Five vowels |
| `blink1` `blink2` `blink3` | Blink frames (sequential) |
| `open` | Open (universal mode only) |
| `schwa` `ae` `uu` `oo` `w` `f` `th` `m` | English extra phonemes (optional) |
 
Accepts `.png`, `.jpg`, `.webp`. Use the **📁 folder import** button to load all at once.
 
### 2. Upload audio
 
Supported formats: MP3, WAV, M4A, AAC, OGG. Best results with clean dry voice recordings — background music causes continuous mouth-open detection.
 
### 3. Enter transcript
 
**Japanese:** space-separated romaji syllables, one sentence per line.
```
ko n ni chi wa | wa ta shi no na ma e wa
```
Use `｜` to mark word boundaries in poetry mode.
 
**English (plain words):**
```
hello how are you today
```
 
**English (Arpabet):** for precise phoneme-level control, use CMU Arpabet format. Prompt any LLM with:
> *"Convert the following text to CMU Arpabet format. Space-separate each phoneme, use | between words, output only the result."*
 
```
HH AH0 L OW1 | HH AW1 AA1 R Y UW0 T AH0 D EY1
```
 
**Multi-character:** prefix each line with `[N]` or `[N:lang]`.
```
[1:jp]ko n ni chi wa
[2:en]HH AH0 L OW1
```
 
### 4. Generate and edit
 
Click **Analyse Audio & Generate Timeline**. The editor shows the lip-sync segments, blink events, emotion track, and subtitle track. Drag segment edges to adjust timing, or edit start/end times directly.
 
### 5. Export
 
Configure canvas resolution and background (solid colour, image, or transparent), then click **Export WebM**.
 
---
 
## Technical notes
 
Built with Vanilla JS and the Web Audio API. No dependencies, no build step — a single self-contained HTML file.
 
- Audio analysis: RMS energy computed in 20 ms windows with 10 ms hop; local adaptive threshold over a ±1.5 s sliding window
- Phoneme mapping: Arpabet → 13 mouth shape classes with fallback chain
- Export: `MediaRecorder` + `OffscreenCanvas` frame pipeline
- Project save/load: JSON with base64-encoded image data (`.lipsync` format)
Requires a Chromium-based browser (Chrome 94+, Edge 94+). Screen width ≥ 900 px recommended.
 
---
 
## Project structure
 
```
lipsync-tool.html   — entire application (single file)
README.md
```
 
---
 
## Author
 
Emily Su · BSc Biomedical Engineering, King's College London  
[Bilibili](https://space.bilibili.com/372684842) · [Xiaohongshu](https://www.xiaohongshu.com/user/profile/649febf1000000000a020352)
 
Developed with assistance from Claude (Anthropic).
 
