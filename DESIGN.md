---
name: Level-Up Circle Generator
description: Dark, tactile tool for building glowing radial goal diagrams.
colors:
  neon-green: "#39FF14"
  signal-pink: "#f5576c"
  void-black: "#000000"
  night-ink: "#0a0a0a"
  header-graphite: "#111111"
  panel-graphite: "#1a1a1a"
  control-graphite: "#2a2a2a"
  control-hover: "#3a3a3a"
  hairline: "#333333"
  control-edge: "#6e6e6e"
  text-primary: "#e0e0e0"
  text-label: "#b0b0b0"
  text-muted: "#888888"
typography:
  title:
    fontFamily: "-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif"
    fontSize: "20px"
    fontWeight: 700
  section:
    fontFamily: "-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif"
    fontSize: "16px"
    fontWeight: 700
    letterSpacing: "1px"
  body:
    fontFamily: "-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif"
    fontSize: "14px"
    fontWeight: 400
  label:
    fontFamily: "-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif"
    fontSize: "14px"
    fontWeight: 500
  diagram-label:
    fontFamily: "'Segoe UI', Roboto, Helvetica, Arial, sans-serif"
    fontSize: "16px"
    fontWeight: 700
rounded:
  sm: "4px"
  md: "6px"
  lg: "8px"
spacing:
  xs: "4px"
  sm: "8px"
  md: "12px"
  lg: "16px"
  xl: "20px"
components:
  button-primary:
    backgroundColor: "{colors.control-graphite}"
    textColor: "{colors.text-primary}"
    rounded: "{rounded.md}"
    padding: "10px 16px"
  button-secondary:
    backgroundColor: "{colors.control-graphite}"
    textColor: "{colors.text-primary}"
    rounded: "{rounded.md}"
    padding: "10px 16px"
  button-secondary-hover:
    backgroundColor: "{colors.control-hover}"
  input-field:
    backgroundColor: "{colors.control-graphite}"
    textColor: "#ffffff"
    rounded: "{rounded.md}"
    padding: "8px 12px"
  point-row:
    backgroundColor: "{colors.control-graphite}"
    rounded: "{rounded.md}"
    padding: "8px"
  panel:
    backgroundColor: "{colors.panel-graphite}"
    padding: "20px"
---

# Design System: Level-Up Circle Generator

## Overview

**Creative North Star: "The Neon Orbit"**

The glowing circle is the only star in the room. The interface is a dark control deck around it: graphite panels on near-black, quiet labels, and controls that feel physical. Light belongs to the diagram; the chrome borrows it only to answer the user (a focused field, a hovered button, a success toast) and always in the same neon green.

Controls are tactile and playful (owner, 2026-11-02). Buttons, toggles, and point rows should respond to hover and press with a visible lift or glow, and the point list reads like a row of physical chips, each carrying its own color swatch. Playfulness lives in response and motion, never in extra ornament or extra hues.

The exported image follows the theme selector (dark or light); the app chrome is always dark.

**Key Characteristics:**
- Near-black stage, graphite panels, one neon accent.
- The diagram's per-point colors are the only rainbow on screen.
- Tactile feedback: hover lift, press, glow on focus.
- Three-column deck: settings left, circle center, points right; panels can be hidden.

## Colors

One loud accent on a graphite stage; the diagram supplies all other color.

### Primary
- **Neon Green** (#39FF14): The single interface accent: focus rings, focus borders, success toasts, copy-link button, add-point hover. Echoes the diagram's glow.

### Secondary
- **Signal Pink** (#f5576c): Destructive and error states only: delete-point hover, error toasts.

### Neutral
- **Void Black** (#000000): The preview stage and the dark-theme export background.
- **Night Ink** (#0a0a0a): Page background.
- **Header Graphite** (#111111): Site header bar.
- **Panel Graphite** (#1a1a1a): Settings and points panels.
- **Control Graphite** (#2a2a2a): Inputs, point rows, secondary buttons.
- **Control Hover** (#3a3a3a): Hovered secondary buttons.
- **Hairline** (#333333): Panel borders and dividers.
- **Control Edge** (#6e6e6e): Input and button borders (≥3:1 against panels, WCAG 1.4.11).
- **Text Primary** (#e0e0e0), **Text Label** (#b0b0b0), **Text Muted** (#888888): Body text, form labels, point indices.

### Retired
- The violet (#667eea → #764ba2) and pink (#f093fb → #f5576c) gradients were removed on 2026-11-02. Export is now a graphite button with a Neon Green border that fills Neon Green on hover and focus.

### Named Rules
**The One Voice Rule.** Neon Green is the only interface accent. No second accent hue in the chrome.

**The Diagram Owns the Rainbow Rule.** Multi-hue color appears only in the circle and in point swatches, never in chrome.

## Typography

**Body Font:** system UI stack (-apple-system, Segoe UI, Roboto, Helvetica Neue, Arial)
**Diagram Font:** 'Segoe UI', Roboto, Helvetica, Arial at weight 700, drawn on canvas

**Character:** Plain system type keeps the chrome invisible; the diagram's bold labels carry the voice.

### Hierarchy
- **Title** (700, 20px; 16px under 900px): App name in the header.
- **Section** (700, 16px, uppercase, 1px letter-spacing, #aaa): Panel headings.
- **Body** (400, 14px): Inputs and buttons.
- **Label** (500, 14px, #b0b0b0): Form labels; 12px under 600px.
- **Diagram label** (700, 8–32px authored, default 16): Point labels on canvas. **Diagram title** (700, 16–48px authored, default 28).

### Named Rules
**The 16px Input Rule.** Text inputs on phones must be at least 16px, or iOS zooms on focus.

## Layout

- Fixed 56px header; below it a CSS grid of 320px | 1fr | 320px filling the viewport.
- The panel toggle cycles both / left only / right only / none by collapsing columns to 0.
- At 900px and below, columns stack with the circle first: preview (min 50vh), then settings, then points; panels grow to their content and the page scrolls as one.
- At 600px and below, padding drops to 12px and the action buttons form a 2-column grid.
- Spacing steps: 4, 8, 12, 16, 20px.

## Elevation & Depth

Flat tonal layering: depth comes from stepping graphite values (night ink → header → panel → control), not from shadows. Shadows appear only as hover feedback.

### Shadow Vocabulary
- **Hover lift** (`box-shadow: 0 3px 10px rgba(255,255,255,0.1)`): secondary button hover.
- **Toast** (`box-shadow: 0 4px 12px rgba(0,0,0,0.4)`): floating toasts.
- **Neon glow** (`box-shadow: 0 0 5px rgba(57,255,20,0.3)`): neon-accented focus and copy-link hover.

### Named Rules
**The Flat-at-Rest Rule.** Surfaces are flat until touched; elevation is a response, not a state.

## Shapes

Gently rounded rectangles (6px) for every control, 4px for small chips and icons, 8px for toasts. Circular forms are reserved for the diagram and the share buttons.

## Components

### Buttons
- **Shape:** gently rounded (6px).
- **Primary / Secondary:** Control Graphite fill, Control Edge border, Text Primary label, full width in panels.
- **Hover / Focus:** lift to Control Hover with the hover-lift shadow; keyboard focus shows a 2px Neon Green outline offset 2px.
- **Destructive:** Signal Pink on hover only (delete point).

### Inputs / Fields
- **Style:** Control Graphite fill, 1px Control Edge border, 6px radius, white text.
- **Focus:** border turns Neon Green and a 2px Neon Green focus-visible outline appears.

### Navigation
- **Header:** text links (Articles, About) left in #ccc, 15px/600, hover to white with soft glow; round icon share buttons right.

### Point Row (signature)
- Four-column chip: zero-padded index, label input, color swatch, delete. Control Graphite fill, 6px radius, 8px padding. The swatch is the row's personality; the row lifts on hover.

### Toast
- Panel Graphite card, 4px left border in Neon Green (success) or Signal Pink (error), 8px radius, slides in from the right.

## Do's and Don'ts

### Do:
- **Do** use Neon Green (#39FF14) for every interface highlight: focus, success, primary hover.
- **Do** give every interactive element a hover response and a visible 2px focus outline.
- **Do** keep chrome in graphite steps (#111 / #1a1a1a / #2a2a2a) so the circle stays the brightest object.

### Don't:
- **Don't** add gradients to chrome.
- **Don't** introduce a second accent hue in the interface.
- **Don't** use text inputs under 16px on phones.
