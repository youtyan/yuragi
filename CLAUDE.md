# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

"ゆらぎのグラフ" (Yuragi Graph) - A web application for visualizing gender expression over time. Users draw lines on a graph with M (masculine) to F (feminine) on X-axis and time periods on Y-axis to represent their gender identity journey.

## Architecture

### Single-File Application
- **`index.html`** - Complete application in one file (HTML, CSS, JavaScript)
- **`Graph.png`** - Background graph image (1617x1792px) embedded as base64
- **No build process** - Static HTML file, works anywhere without server

### Core Technical Decisions
1. **Base64 embedded image** - Avoids CORS issues, enables offline use
2. **Canvas overlay** - Transparent drawing layer over background image
3. **Responsive scaling** - Uses coordinate ratios for device-independent rendering
4. **State management** - Simple JavaScript variables for modes and drawing state

## Key Implementation Details

### Drawing System
```javascript
// Drawing area boundaries (ratios relative to image)
drawingAreaRatio = {
    x: 140/1617,      // Start at M-F axis line
    y: 340/1792,      // Start at first time marker
    width: 1340/1617,
    height: 1250/1792
}

// Drawing modes
- drawingEnabled: true when color button is active
- currentColor: '#333333' (black) or '#ff6b9d' (pink) or null
- selectMode: for selecting existing lines
- eraseMode: for deleting lines
```

### Critical Behaviors
1. **Color buttons toggle** - Click active button to deactivate drawing
2. **Drawing from outside** - Can start drag outside drawing area
3. **Coordinate clamping** - Lines snap to drawing boundaries
4. **Touch/mouse unified** - Same behavior on all devices

### Button State Logic
- Only one mode active at a time (draw/select/erase)
- Color buttons show active state with golden border and checkmark
- Drawing only possible when a color is selected

## Development

```bash
# Serve locally
python -m http.server 8000
# Access at http://localhost:8000

# Debug mode (shows drawing boundaries)
# http://localhost:8000/#debug
```

## Common Modifications

### Adjust Drawing Area
```javascript
// In drawingAreaRatio object
x: 140/1617,     // Horizontal start position
y: 340/1792,     // Vertical start position
```

### Change Line Appearance
```javascript
ctx.lineWidth = 3;        // Line thickness
minDistance = 3;          // Hand tremor threshold (pixels)
```

### Modify Colors
```javascript
setColor('#333333', event)  // Black button
setColor('#ff6b9d', event)  // Pink button
```

## Testing Checklist
- [ ] Color button toggle functionality
- [ ] Drawing from outside boundaries
- [ ] Touch/mouse compatibility
- [ ] Save image with background
- [ ] Responsive layout on mobile/desktop
- [ ] Selection and erasing modes
- [ ] Line smoothing function