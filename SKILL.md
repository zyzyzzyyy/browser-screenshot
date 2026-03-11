---
name: browser-screenshot
description: |
  Capture browser pages and send to users. Use this skill when the user wants to take screenshots of webpages — including full-page captures, mobile viewport testing, or capturing after interactions. Trigger on phrases like "浏览器截图", "网页截图", "screenshot", "capture browser", "移动端截图", or when testing responsive designs.
---

# Browser Screenshot Skill 📸

Capture browser pages and send to users. Supports full-page, mobile viewports, and post-interaction screenshots.

## When to Use

- User wants a screenshot of a webpage
- Testing mobile responsive designs
- Need full-page captures of long pages
- Capturing after form interactions

## Workflow

### Step 1: Open Target URL

```json
{
  "action": "navigate",
  "target": "host",
  "url": "https://example.com"
}
```

### Step 2: Verify Page Loaded

```json
{
  "action": "tabs",
  "target": "host"
}
```

Confirm it's not `about:blank`.

### Step 3: Take Screenshot

**Standard screenshot:**
```json
{
  "action": "screenshot",
  "target": "host",
  "type": "png"
}
```

**Full page:**
```json
{
  "action": "screenshot",
  "target": "host",
  "type": "png",
  "fullPage": true
}
```

**Mobile viewport:**
```json
{
  "action": "screenshot",
  "target": "host",
  "type": "png",
  "width": 375,
  "height": 667
}
```

**Common viewports:**
| Device | Size |
|--------|------|
| iPhone SE/13 Mini | 375x667 |
| iPhone 14 Pro | 393x852 |
| iPad | 768x1024 |
| Android | 360x640 |

### Step 4: Send Screenshot

```json
{
  "action": "send",
  "media": "~/.openclaw/media/browser/xxx.png",
  "mimeType": "image/png"
}
```

## Common Patterns

### Full workflow
```json
// 1. Navigate
{"action": "navigate", "target": "host", "url": "https://example.com"}

// 2. Wait for load
{"action": "act", "request": {"kind": "wait", "timeMs": 2000}}

// 3. Screenshot
{"action": "screenshot", "target": "host", "type": "png", "fullPage": true}

// 4. Send
{"action": "send", "media": "~/.openclaw/media/browser/xxx.png", "mimeType": "image/png"}
```

### With interactions
```json
// 1. Navigate
{"action": "navigate", "target": "host", "url": "http://localhost:5173/"}

// 2. Fill form
{"action": "act", "request": {"kind": "fill", "ref": "e12", "text": "test"}}

// 3. Click button
{"action": "act", "request": {"kind": "click", "ref": "e34"}}

// 4. Wait
{"action": "act", "request": {"kind": "wait", "timeMs": 2000}}

// 5. Screenshot
{"action": "screenshot", "target": "host", "type": "png"}

// 6. Send
{"action": "send", "media": "~/.openclaw/media/browser/xxx.png", "mimeType": "image/png"}
```

## Error Handling

| Error | Cause | Fix |
|-------|-------|-----|
| Blank screenshot | `about:blank` | Navigate first |
| File not found | Wrong path | Check `ls ~/.openclaw/media/browser/` |
| MIME mismatch | Wrong type | PNG → `image/png` |
| Wrong page | Didn't verify | Use `tabs` to check |

## Tips

✅ Confirm not `about:blank` before screenshot
✅ Use `tabs` to verify current page
✅ Use absolute paths
✅ Match MIME type to file
✅ Use `fullPage: true` for long pages
✅ Use `width`/`height` for mobile testing

❌ Don't screenshot blank pages
❌ Don't use relative paths
❌ Don't skip navigation step

## Files

- Screenshots: `~/.openclaw/media/browser/`
- Filenames: UUID format (e.g., `412eeca8-xxx.png`)

## Dependencies

- OpenClaw Browser tool
- OpenClaw Message tool
- Configured browser environment

## Version

1.0.0 - Initial release
