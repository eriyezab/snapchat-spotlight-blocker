# Plan: Hide Spotlight Button via Content Script CSS

## Goal
Hide the Spotlight nav button in the left sidebar on `web.snapchat.com` so the UI gives no indication Spotlight exists, complementing the existing network-level blocking.

## DOM Structure (observed)
```html
<div class="OwWqx" title="View Spotlight Stories">
  <button type="button" class="cYbyK hw3Mb qRNqg">...</button>
  <div class="tAPSl"><span class="nonIntl">Spotlight</span>...</div>
</div>
```

The `title="View Spotlight Stories"` attribute is human-readable and was expected to be stable. CSS class names (`OwWqx`, etc.) are obfuscated and rotate.

## Approach
Add a content script (`content.js`) that injects a `<style>` tag hiding the Spotlight button. Registered in `manifest.json` with `run_at: "document_start"`.

## Implementation
- `extension/manifest.json` — added `content_scripts` entry matching `https://web.snapchat.com/*`, loading `content.js` at `document_start`
- `extension/content.js` (new) — injects `<style>` into `document.documentElement`

## Attempts & Failures

### Attempt 1
**Selector:** `div.OwWqx[title="View Spotlight Stories"]`
**Result:** Did not work. The CSS class `OwWqx` is obfuscated and had likely rotated since the plan was written.

### Attempt 2
**Selector:** `[title="View Spotlight Stories"]`
**Result:** Did not work. The `title` attribute was confirmed present in the live DOM via `document.querySelector('[title="View Spotlight Stories"]')` returning the correct element in the DevTools console. The CSS rule should have matched — root cause is unconfirmed. Suspected issues:
- The `<style>` tag may not actually be injected (head tag contents were not captured to confirm)
- The extension content script may not be executing on the page (no console errors captured)
- Snapchat may be using a Shadow DOM or iframe that isolates the nav from the main document

## Next Steps to Investigate
1. Confirm the `<style>` tag is present in the page DOM after load (check `<html>` children in DevTools Elements panel)
2. Add a `console.log` to `content.js` to confirm the script runs at all
3. Check if the nav renders inside a Shadow DOM root or a cross-origin iframe — if so, injected styles won't reach it
4. Consider using a `MutationObserver` in the content script to directly remove the element from the DOM instead of hiding via CSS
