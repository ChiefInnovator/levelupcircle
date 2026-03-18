# URL Sharing & Deep Linking Specification

## Overview

The Level-Up Circle Generator supports deep linking via compressed URL parameters. Users can share a URL that fully reconstructs their circle configuration, including all points, colors, settings, and display options.

## URL Format

```
https://chiefinnovator.github.io/levelupcircle/levelup-circle-generator.html?s={compressed_state}&fs={fullscreen}
```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `s` | Yes | lz-string compressed, URI-encoded state JSON |
| `fs` | No | `1` to open in fullscreen mode |

## Compression

State is compressed using [lz-string](https://github.com/pieroxy/lz-string) v1.5.0:

1. State object is serialized to a compact JSON format (short keys)
2. JSON string is compressed with `LZString.compressToEncodedURIComponent()`
3. Result is placed in the `s` query parameter

Decompression reverses the process with `LZString.decompressFromEncodedURIComponent()`.

## Compact State Schema

The URL state uses abbreviated keys to minimize URL length:

```json
{
  "t": "Level-Up Circle",
  "n": 8,
  "es": 3840,
  "th": "dark",
  "ps": 14,
  "lo": 35,
  "fs": 16,
  "tfs": 28,
  "gt": 1,
  "gp": 1,
  "gl": 1,
  "gc": 1,
  "gn": 1,
  "sc": 1,
  "p": [
    ["MILL5", "FF6B6B"],
    ["Card Game", "4ECDC4"]
  ]
}
```

### Key Mapping

| Compact Key | Full State Property | Type |
|-------------|-------------------|------|
| `t` | `title` | string |
| `n` | `numPoints` | number (1–36) |
| `es` | `exportSize` | number (1080, 2048, 3840) |
| `th` | `theme` | string (`"dark"` or `"light"`) |
| `ps` | `pointSize` | number (1–60) |
| `lo` | `labelOffset` | number (0–100) |
| `fs` | `labelFontSize` | number (8–32) |
| `tfs` | `titleFontSize` | number (16–48) |
| `gt` | `neonGlowTitle` | 0 or 1 |
| `gp` | `neonGlowPoints` | 0 or 1 |
| `gl` | `neonGlowLabels` | 0 or 1 |
| `gc` | `neonGlowCircle` | 0 or 1 |
| `gn` | `neonGlowConnections` | 0 or 1 |
| `sc` | `showConnections` | 0 or 1 |
| `p` | `points` | array of `[label, colorHexWithout#]` |

### Points Array

Each point is stored as a two-element array `[label, color]` where:
- `label` is the point's text label (string)
- `color` is the hex color without the `#` prefix (e.g., `"FF6B6B"`)

## Encoding Flow

```
encodeStateToURL()
├── Build compact object with short keys
├── Convert booleans to 0/1
├── Strip # from point colors
├── JSON.stringify()
├── LZString.compressToEncodedURIComponent()
├── Construct URL with ?s= parameter
└── Append &fs=1 if fullscreen is active
```

## Decoding Flow

```
decodeStateFromCompressed(str)
├── LZString.decompressFromEncodedURIComponent()
├── JSON.parse()
├── Map compact keys back to full property names
├── Convert 0/1 back to booleans
├── Re-add # prefix to point colors
├── Validate and apply to state
└── Sync UI controls with loaded state
```

## Legacy URL Support

The application maintains backward compatibility with an older URL format that used uncompressed JSON with different key names. The decoding logic detects and normalizes legacy property names (e.g., older glow property naming conventions) when loading from URL parameters.

## Share Functions

### Copy Link (`shareLink()`)

1. Calls `encodeStateToURL()` to generate the full URL
2. Uses `navigator.clipboard.writeText()` to copy to clipboard
3. Shows a success toast notification

### Twitter/X (`shareToTwitter()`)

1. Encodes the state URL
2. Constructs a Twitter intent URL with pre-filled text including the circle title
3. Opens in a new browser tab

### Facebook (`shareToFacebook()`)

1. Encodes the state URL
2. Constructs a Facebook share dialog URL
3. Opens in a new browser tab

## URL Length Considerations

- lz-string compression typically reduces URL length by 60–80%
- A circle with 8 points and default settings produces a URL of approximately 200–400 characters
- Maximum of 36 points with long labels may produce longer URLs, but still within browser URL limits (~2,000 characters for most browsers, up to ~8,000 for modern browsers)
