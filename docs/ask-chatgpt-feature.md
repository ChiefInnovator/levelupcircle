# Ask ChatGPT Feature

## Overview

The "Ask ChatGPT" feature allows users to get personalized strategic advice about their Level-Up Circle. After configuring their circle with focus areas, users can click the ChatGPT button to open a new browser tab with a pre-constructed prompt that includes their specific goals and configuration.

## User Experience

1. User configures their Level-Up Circle with a title and focus area labels
2. User clicks the green ChatGPT icon in the share buttons bar
3. A new browser tab opens with ChatGPT pre-loaded with a personalized strategy prompt
4. ChatGPT provides tailored advice for creating synergy between focus areas

## Button Design

| Property | Value |
|----------|-------|
| Icon | OpenAI/ChatGPT logo |
| Color | `#10a37f` (OpenAI brand green) |
| Hover State | Lighten to `#12b886` |
| Size | 36x36 pixels |
| Position | After Facebook button, before Fullscreen toggle |

## Prompt Template

The prompt is designed to provide comprehensive, actionable strategic advice:

```
I have created a Level-Up Circle called "{TITLE}" with these {COUNT} focus areas: {POINT_LABELS}.

Analyze how these areas can create synergy and compound growth. Provide a strategic action plan that:
1. Identifies the strongest connections between these focus areas
2. Suggests daily/weekly habits that reinforce multiple areas simultaneously
3. Recommends which area to prioritize first for maximum flywheel effect
4. Highlights potential conflicts or trade-offs to watch for
5. Provides specific next actions for each focus area
```

## Dynamic Values

| Placeholder | Source | Description |
|-------------|--------|-------------|
| `{TITLE}` | `state.title` | The circle's title (e.g., "Level-Up Circle") |
| `{COUNT}` | `state.points.length` | Number of focus areas |
| `{POINT_LABELS}` | `state.points` | Comma-separated list of all point labels |

## Technical Implementation

### URL Construction

```
https://chat.openai.com/?q={URL_ENCODED_PROMPT}
```

### Value Extraction Logic

```javascript
function askChatGPT() {
    const title = state.title;
    const count = state.points.length;
    const labels = state.points.map(p => p.label).join(', ');
    
    const prompt = `I have created a Level-Up Circle called "${title}" with these ${count} focus areas: ${labels}.

Analyze how these areas can create synergy and compound growth. Provide a strategic action plan that:
1. Identifies the strongest connections between these focus areas
2. Suggests daily/weekly habits that reinforce multiple areas simultaneously
3. Recommends which area to prioritize first for maximum flywheel effect
4. Highlights potential conflicts or trade-offs to watch for
5. Provides specific next actions for each focus area`;

    const chatGptUrl = `https://chat.openai.com/?q=${encodeURIComponent(prompt)}`;
    window.open(chatGptUrl, '_blank');
}
```

### URL Encoding

The prompt is encoded using JavaScript's `encodeURIComponent()` function to ensure all special characters are properly escaped for URL transmission.

### Browser Behavior

```javascript
window.open(chatGptUrl, '_blank');
```

- Opens in a new browser tab
- Does not affect the current Level-Up Circle page
- User can return to their circle after reviewing ChatGPT's response

## Example

### Input

- **Title:** "2025 Goals"
- **Focus Areas:** MILL5, Card Game, Webinars, Casual Apps, Inventing Fire with AI, Rich Crane, Xmas Store, Purdue Course

### Generated Prompt

```
I have created a Level-Up Circle called "2025 Goals" with these 8 focus areas: MILL5, Card Game, Webinars, Casual Apps, Inventing Fire with AI, Rich Crane, Xmas Store, Purdue Course.

Analyze how these areas can create synergy and compound growth. Provide a strategic action plan that:
1. Identifies the strongest connections between these focus areas
2. Suggests daily/weekly habits that reinforce multiple areas simultaneously
3. Recommends which area to prioritize first for maximum flywheel effect
4. Highlights potential conflicts or trade-offs to watch for
5. Provides specific next actions for each focus area
```

### Generated URL

```
https://chat.openai.com/?q=I%20have%20created%20a%20Level-Up%20Circle%20called%20%222025%20Goals%22%20with%20these%208%20focus%20areas%3A%20MILL5%2C%20Card%20Game%2C%20Webinars%2C%20Casual%20Apps%2C%20Inventing%20Fire%20with%20AI%2C%20Rich%20Crane%2C%20Xmas%20Store%2C%20Purdue%20Course.%0A%0AAnalyze%20how%20these%20areas%20can%20create%20synergy%20and%20compound%20growth...
```

## Files to Modify

| File | Changes |
|------|---------|
| `levelup-circle-generator.html` | Add ChatGPT button with OpenAI logo SVG icon, add CSS styling, add `askChatGPT()` function |

## Dependencies

- Requires an active internet connection
- User must have access to ChatGPT (free or paid account)
- No API keys or authentication required from this application

## Future Enhancements

Potential improvements for future versions:

1. **Alternative AI Services** - Support for Claude, Gemini, or other AI assistants
2. **Custom Prompts** - Let users modify the prompt template
3. **Prompt Presets** - Different prompt styles (strategic, creative, analytical)
4. **Include Visual Context** - Mention theme, colors, or connection patterns
5. **Inline Responses** - Display AI responses within the app (would require API integration)

## Related Features

- **Copy Share Link** - Copy a shareable URL with circle configuration
- **Share to Facebook** - Post circle to Facebook
- **Share to X/Twitter** - Post circle to X (Twitter)
- **Fullscreen Mode** - View circle in presentation mode
