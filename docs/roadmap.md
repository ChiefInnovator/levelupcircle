# Roadmap & Enhancement Plan

UI/UX improvement plan for the Level-Up Circle Generator. Items marked with [x] are implemented; [ ] items are planned.

## Completed

### 1. Mobile-Responsive Layout

- [x] Add `@media (max-width: 900px)` query to stack controls above preview
- [x] Add `@media (max-width: 600px)` query for smaller screens
- [x] Adjust `.controls` max-height and border styling for mobile
- [x] Make `.button-grid` responsive on small screens (2-column)
- [x] Add mobile toggle button to show/hide controls
- [x] Add scroll indicator for mobile controls

### 2. Collapsible Sections

- [x] Use `<details>/<summary>` HTML elements for collapsible UI
- [x] Wrap Settings, Glow Effects, Actions, Point Configuration in sections
- [x] Add toggle icons and smooth transitions

### 3. Toast Notification System

- [x] Create `.toast` CSS styles with slide-in animation
- [x] Implement `showToast(message, type)` function with success/error/info types
- [x] Replace `alert()` in `saveState()` with toast
- [x] Replace `alert()` in `clearState()` with toast
- [x] Add toast for export success/failure
- [x] Add toast for share link copy

### 4. Social Sharing & Deep Linking

- [x] URL sharing with compressed state (lz-string)
- [x] Copy Share Link button
- [x] Share to Twitter/X
- [x] Share to Facebook
- [x] Floating share bar UI

### 5. Ask ChatGPT Integration

- [x] Add ChatGPT button with OpenAI logo SVG
- [x] Generate personalized strategy prompt from circle configuration
- [x] Open ChatGPT in new browser tab

### 6. Fullscreen Mode

- [x] Toggle fullscreen to hide controls
- [x] Preserve fullscreen state in shared URLs (`&fs=1`)
- [x] Fullscreen button in share bar

### 7. Auto-Save

- [x] Debounced auto-save to localStorage on every state change
- [x] Silent operation (no toast for auto-save)

## Planned

### 8. Drag-and-Drop Point Reordering

- [ ] Add `draggable="true"` to `.point-item` elements
- [ ] Implement `handleDragStart()` function
- [ ] Implement `handleDragOver()` function
- [ ] Implement `handleDrop()` function
- [ ] Implement `handleDragEnd()` function
- [ ] Add visual drag handle to point items

### 9. Keyboard Shortcuts & Undo/Redo

- [ ] Add keydown event listener for shortcuts
- [ ] Implement `Cmd/Ctrl+S` for save
- [ ] Implement `Cmd/Ctrl+E` for export
- [ ] Implement `Cmd/Ctrl+R` for random colors
- [ ] Create history array and `pushHistory()` function
- [ ] Implement `undo()` and `redo()` functions
- [ ] Add `Cmd/Ctrl+Z` and `Cmd/Ctrl+Shift+Z` handlers
- [ ] Create `syncUIFromState()` helper

### 10. Accessibility Improvements

- [ ] Add `role="form"` to `.controls` container
- [ ] Add `aria-labelledby` attributes to inputs
- [ ] Add skip link to canvas at top of controls
- [ ] Add `:focus-visible` CSS styles
- [ ] Add `aria-label` to point configuration inputs

### 11. Future Ideas

- [ ] Drag-and-drop manual reordering or angular offset per point
- [ ] Selective connection modes (adjacent, strategic, custom)
- [ ] Import/export JSON configuration files
- [ ] SVG export for print / vector workflows
- [ ] Open Graph image generation for social previews
- [ ] Preset templates (career, fitness, business, etc.)
- [ ] Animation preview
- [ ] Alternative AI services (Claude, Gemini)
- [ ] Custom prompt templates for ChatGPT
- [ ] Inline AI responses within the app (API integration)

## File to Modify

- `levelup-circle-generator.html`
