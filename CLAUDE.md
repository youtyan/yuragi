# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a "ゆらぎのグラフ" (Yuragi Graph) web application - an interactive canvas drawing tool designed for visualizing gender expression and identity over time. Users can draw lines on a graph with M (masculine) to F (feminine) on the X-axis and time periods 1-5 on the Y-axis.

## Technical Architecture

### Core Structure
- **Single-page application** - `index.html` contains all HTML, CSS, and JavaScript
- **Background image overlay** - Uses `Graph.png` (1617x1792) as the graph template
- **Canvas-based drawing** - Transparent canvas layer positioned over the background image
- **No build process** - Pure HTML/CSS/JavaScript, no dependencies or frameworks

### Key Components

1. **Drawing System**
   - Two-layer approach: background image (`Graph.png`) + transparent drawing canvas
   - Drawing area boundaries calculated as ratios relative to image size
   - Smooth line drawing using quadratic Bézier curves
   - Hand tremor correction with minimum distance threshold (3px)

2. **Coordinate System**
   - Drawing area: x: 140/1617, y: 340/1792, width: 1340/1617, height: 1250/1792
   - Coordinates are clamped to drawing boundaries for natural edge behavior
   - High DPI display support with devicePixelRatio scaling

3. **Features**
   - Two drawing colors: black (#333333) for "振る舞ってきた性" and pink (#ff6b9d) for "性自認"
   - Clear button to reset the canvas
   - Save functionality that composites the background and drawing layers
   - Responsive design with touch support
   - Debug mode (add #debug to URL) shows drawing boundaries in red

## Development Commands

```bash
# Run locally (no build needed)
python -m http.server 8000
# or
npx http-server

# Debug drawing area
# Open http://localhost:8000/#debug
```

## Important Implementation Details

- **Drawing area precision**: The drawing boundaries are precisely calibrated to start at the M-F axis line (y: 340/1792)
- **Smooth lines**: Uses a point collection system with quadratic curves for natural-looking lines
- **Performance**: Only redraws the last 3 points during active drawing for smooth real-time feedback
- **Save mechanism**: Creates a temporary canvas at original resolution, composites both layers, then triggers download

## Common Adjustments

1. **Drawing area boundaries**: Modify `drawingAreaRatio` object in JavaScript
2. **Hand tremor sensitivity**: Adjust `minDistance` variable (currently 3 pixels)
3. **Line thickness**: Change `ctx.lineWidth` value (currently 3)
4. **Colors**: Modify hex values in `setColor()` function calls