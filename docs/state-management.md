# State Management

## State Schema

The application state is a single JavaScript object:

```javascript
let state = {
    title: 'Level-Up Circle',        // Circle title text
    numPoints: 8,                    // Number of points (1–36)
    exportSize: 3840,                // Export resolution (1080, 2048, or 3840)
    theme: 'dark',                   // 'dark' or 'light'
    neonGlowTitle: true,             // Glow effect on title
    neonGlowPoints: true,            // Glow effect on point dots
    neonGlowLabels: true,            // Glow effect on label text
    neonGlowCircle: true,            // Glow effect on outer ring
    neonGlowConnections: true,       // Glow effect on connection lines
    showConnections: true,           // Show lines between all points
    pointSize: 14,                   // Point dot radius (1–60)
    labelOffset: 35,                 // Label distance from circle edge (0–100)
    labelFontSize: 16,               // Label text size (8–32)
    titleFontSize: 28,               // Title text size (16–48)
    points: [                        // Array of point objects
        {
            label: 'MILL5',          // Display text
            color: '#FF6B6B'         // Hex color with # prefix
        }
        // ... up to 36 points
    ]
};
```

## Persistence

### Auto-Save

Every state change triggers a debounced auto-save (500ms delay) to `localStorage`:

- Key: `levelUpCircleState`
- Value: `JSON.stringify(state)`
- Silent — no toast notification
- Debounced to avoid excessive writes during rapid changes (e.g., dragging a slider)

### Manual Save

The "Save State" button calls `saveState()`:

- Same localStorage key and format as auto-save
- Shows a success toast notification on completion
- Shows an error toast if storage is full or unavailable

### Load on Startup

On page load, state is restored in this priority order:

1. **URL parameters** — If `?s=` is present, decode and apply the compressed state
2. **localStorage** — If no URL params, load from `levelUpCircleState`
3. **Default example** — If neither exists, call `loadExample()` for the demo state

### Clear State

The "Clear State" button:

1. Removes `levelUpCircleState` from localStorage
2. Reloads the example/default state
3. Shows a success toast notification

## Default State

The `loadExample()` function sets:

- Title: `"Level-Up Circle"`
- 8 points with labels: MILL5, Card Game, Webinars, Casual Apps, Inventing Fire with AI, Rich Crane, Xmas Store, Purdue Course
- Colors generated with equal spacing, seed 42, saturation 0.85, lightness 0.50
- All glow effects enabled
- Connections shown
- Point size 14, label offset 35, label font 16, title font 28
- Dark theme, 3840 export size

## UI Synchronization

When state is loaded (from URL, localStorage, or example), the UI controls are synchronized:

- Text inputs receive `state.title`
- Dropdowns receive `state.theme` and `state.exportSize`
- Sliders receive `state.numPoints`, `state.pointSize`, `state.labelOffset`, `state.labelFontSize`, `state.titleFontSize`
- Checkboxes receive all glow toggles and `state.showConnections`
- Point list is rebuilt with `updatePointList()`
- Canvas is redrawn with `redraw()`

## State Flow

```
User interaction (input/click)
    ↓
Update state property
    ↓
├── redraw() → Canvas re-renders
├── debouncedAutoSave() → localStorage (500ms delay)
└── updatePointList() → Point list UI (if points changed)
```

## Backward Compatibility

When loading state from localStorage or URLs, the application normalizes older property names. This handles states saved with earlier versions of the application that may have used different naming conventions for glow properties.
