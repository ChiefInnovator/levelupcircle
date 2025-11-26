# Level-Up Circle UI Enhancement Plan

Implement UI/UX improvements for the Level-Up Circle Generator.

## Steps

### 1. Mobile-Responsive Layout
- [ ] Add `@media (max-width: 900px)` query to stack controls above preview
- [ ] Add `@media (max-width: 600px)` query for smaller screens
- [ ] Adjust `.controls` max-height and border styling for mobile
- [ ] Make `.button-grid` single-column on small screens

### 2. Collapsible Sections
- [ ] Create `.section-header` and `.section-content` CSS classes
- [ ] Wrap Settings, Glow Effects, Actions, Point Configuration in sections
- [ ] Add toggle icons and collapse/expand animations
- [ ] Persist collapsed state in localStorage

### 3. Toast Notification System
- [ ] Create `.toast` CSS styles with slide-in animation
- [ ] Implement `showToast(message, type)` function
- [ ] Replace `alert()` in `saveState()` with toast
- [ ] Replace `alert()` in `clearState()` with toast
- [ ] Add toast for export success/failure

### 4. Drag-and-Drop Point Reordering
- [ ] Add `draggable="true"` to `.point-item` elements
- [ ] Implement `handleDragStart()` function
- [ ] Implement `handleDragOver()` function
- [ ] Implement `handleDrop()` function
- [ ] Implement `handleDragEnd()` function
- [ ] Add visual drag handle (⋮⋮) to point items

### 5. Keyboard Shortcuts & Undo/Redo
- [ ] Add keydown event listener for shortcuts
- [ ] Implement `Cmd/Ctrl+S` for save
- [ ] Implement `Cmd/Ctrl+E` for export
- [ ] Implement `Cmd/Ctrl+R` for random colors
- [ ] Create history array and `pushHistory()` function
- [ ] Implement `undo()` and `redo()` functions
- [ ] Add `Cmd/Ctrl+Z` and `Cmd/Ctrl+Shift+Z` handlers
- [ ] Create `syncUIFromState()` helper

### 6. Accessibility Improvements
- [ ] Add `role="form"` to `.controls` container
- [ ] Add `aria-labelledby` attributes to inputs
- [ ] Add skip link to canvas at top of controls
- [ ] Add `:focus-visible` CSS styles
- [ ] Add `aria-label` to point configuration inputs

## Future Enhancements

- [ ] SVG export option
- [ ] URL sharing (encode state in query params)
- [ ] Dark/Light theme toggle
- [ ] Preset templates
- [ ] Animation preview

## File to Modify

- `levelup-circle-generator.html`
