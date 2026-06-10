# F_Replace

## Usage

Download all contents from:

https://github.com/Jekyell/mappings

Place them into:

`/sdcard/Android/data/com.aniplex.fategrandorder/files/Mod/`

Then set the permissions of the `Mod` folder and all its subfolders/files to `777`.

---

## Module Configuration

After launching the game, a configuration file named `frep.config` will be automatically generated in:

`Mod/configs/`

You can manually enable (`1`) or disable (`0`) individual features by editing this file.

---

## Portrait Replacement

- **Directory:** `Mod/Figure/Faces/`
- **Supported Format:** PNG
- **Supported Resolution:** 256×256 or 512×512
- **Filename Format:** `ServantID@ImageIndex.png`

Example:

`204300@1.png`

→ Replaces Baobhan Sith's first portrait.

---

## Story Text Packaging Tool (FGSB Tool)

Download:

https://github.com/Jekyell/F_Replace/releases/tag/tools

### Pack (Folder → FGSB)

```bash
./fgsb_tool.exe pack ./StoryFolder 20250209.fgsb
```

### Unpack (FGSB → Folder)

```bash
./fgsb_tool.exe unpack 20250209.fgsb ./StoryFolder
```

Place the generated `.fgsb` file into:

`Mod/ScriptBundle/`

This enables single-file story text replacement.

The feature is fully compatible with the original TXT-based replacement system. If both exist, the module will prioritize TXT files located in:

`Mod/Script/`

---

## Custom Loading Animation

### Setup

1. Create the directory:

   `Mod/Figure/Load`

2. Place your sprite sheet image and JSON configuration file into this folder.

Example files:

- `animation.json`
- `animation.png`

Download example assets:

https://github.com/Jekyell/F_Replace/releases/tag/load

### frep.config Settings

```ini
# Custom loading animation position settings (DP)

# Enable/Disable
LoadingOverlay=1

# Right margin
MARGIN_RIGHT=45

# Bottom margin
MARGIN_BOTTOM=14

# Image size
IMAGE_SIZE=150

# Animation FPS
LoadingFPS=24
```

---

## Animated / Static Card Art Replacement

> **Experimental Feature**
>
> This feature is experimental and may cause memory overflow, VRAM overflow, crashes, or other unexpected issues.

### Supported Categories

- Face
- NarrowFigure
- CharaGraph
- CharaFigure (static images only)
- Status (static images only)

### Naming Convention

All replacement files must follow:

`[ServantID]@[AscensionStage]`

Example:

`4000100@4`

→ Olga Marie (Final Ascension)

Ascension stage range: **1–4**

### Recommendations

For both static and animated replacements, use images that are scaled proportionally from the original card art resolution.

High-resolution images are supported.

For animated replacements, pay attention to:

- Resolution
- Frame rate

Excessively high specifications may cause severe lag, memory overflow, or game crashes.

Recommended settings:

- Around **2× upscaled original resolution**
- **30 FPS**

### HQ Servant Card Art Collection

Telegram:

https://t.me/fgomod_1

After downloading, create the corresponding archive folder inside:

`Mod/Figure/`

Then extract the images into it.

If the card art appears blurry in an emulator, increase the emulator's rendering resolution.

### Supported Formats

| Type | Supported Formats | Priority | Notes |
|--------|--------|--------|--------|
| `CharaGraph` | `.mp4`, `.webp`, `.png` | `mp4 → webp → png` | - |
| `CharaFigure` | `.png`, `.astc`, `.astc.zstd` | `png → astc → astc.zstd` | Static only |
| `NarrowFigure` | `.mp4`, `.webp`, `.png` | `mp4 → webp → png` | Four-corner crop |
| `Face` | `.mp4`, `.webp`, `.png` | `mp4 → webp → png` | Top-left + top-right crop |
| `Status` | `.webp`, `.png`, `.astc`, `.astc.zstd` | `webp → png → astc → astc.zstd` | Static only |

---

## FPS Unlock

```ini
# Enable FPS unlock (0 = Disabled, 1 = Enabled)
UnlockFPS=0

# Target FPS
FPS=60
```

---

## Telegram Channel

https://t.me/fgomod
