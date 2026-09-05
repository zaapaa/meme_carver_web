# Meme Carver (Web)

Browser port of the seam-carving meme generator.  
Original project: [zaapaa/meme_carver](https://github.com/zaapaa/meme_carver)  
Live demo: [zaapaa.github.io/meme_carver_web/](https://zaapaa.github.io/meme_carver_web/)

## Overview

This is a single-file web application. It runs entirely in your browser. No server. No installation. Use the live demo above, or open `index.html` in a modern browser.

## Features

- **Seam-carving melt** — content-aware resize that shrinks the image while it keeps the important parts.
- **Input** — load any image (JPG, PNG, GIF, WebP, AVIF, BMP, TIFF). Animated GIFs work frame by frame.
- **Transparency** — toggle to keep transparent pixels instead of flattening to black. The carving algorithm avoids transparent areas first.
- **Animation controls** — minimum scale, frame count, linear or sine schedule, recursive mode, loop (boomerang), frame delay.
- **Resize output** — keep native resolution, scale, or set a target width and height.
- **Size-limited GIF card** — dedicated preview with its own controls:
  - Maximum file size in KB
  - Permission checkboxes: palette reduction, dithering, drop every 2nd/3rd/4th frame, resolution reduction
  - Optional: merge dropped frames' durations to keep total animation length
  - Live preview of the optimized result with file size and reduction percentage
  - Automatic re-fit when you change the limit or the checkboxes
- **Download formats** — GIF (with or without size limit), animated WebP (lossy, smooth alpha), animated PNG / APNG (lossless, full alpha), PNG frames ZIP.
- **Transparency** — toggle to keep transparent pixels. Transparent areas are removed first during carving.

## How to use

1. Open `index.html` in a browser (Chrome, Firefox, Edge, Safari) — or use the live demo.
2. Drop an image, click to browse, or paste (Ctrl+V). Or click **Load a sample image** / **Load a transparent sample**.
3. Adjust the settings on the left.
4. Press **Carve!**
5. The **Output** card shows the full-quality result.
6. The **Size-limited GIF** card appears below it when you set a limit. Adjust the checkboxes to see the effect in real time. The optimized preview plays below the controls.
7. Click **Download GIF** (full quality) or **Download optimized GIF** (size-limited). WebP / PNG / Frames ZIP buttons are also available.

## Requirements

- Modern browser with Web Workers, OffscreenCanvas, and `createImageBitmap` support (all current browsers).
- No internet connection required after the initial load.

## How the carving works

Seam carving removes low-energy pixel paths (seams) from the image.

1. **Energy map** — for each pixel, compute the color difference with its horizontal and vertical neighbors. High difference = high energy (important detail). Low difference = low energy (flat area).
2. **Find the cheapest seam** — dynamic programming finds a connected path of pixels from top to bottom (or left to right) with the lowest total energy.
3. **Remove the seam** — delete that path of pixels. The image shrinks by one pixel in that dimension.
4. **Repeat** — recompute energy and remove seams until the target scale is reached.

Transparent pixels (when the option is on) get zero energy, so seams cut through transparent areas first. The foreground subject stays intact.

## Attribution

Ported from the Python/Tkinter implementation by [zaapaa](https://github.com/zaapaa/meme_carver). Seam-carving, GIF codec, and ZIP writer reimplemented in vanilla JavaScript.
