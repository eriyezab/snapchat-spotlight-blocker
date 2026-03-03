# Snapchat Spotlight Blocker — Chrome Extension

## Goal
Build a minimal Manifest V3 Chrome extension that blocks all network requests to Snapchat's Spotlight API and hides the Spotlight UI tab on web.snapchat.com.

## File Structure
```
snapchat-spotlight-blocker/
├── manifest.json
└── rules.json
```

## Files to Create

### 1. `manifest.json`
- Manifest version: 3
- Permissions: `declarativeNetRequest`, `declarativeNetRequestFeedback`
- Host permissions: `https://web.snapchat.com/*`
- Register `rules.json` as a declarative net request ruleset with id `"ruleset_1"`, enabled by default


### 2. `rules.json`
Block all of the following URL patterns using resource types `xmlhttprequest`, `fetch`, `main_frame`, `sub_frame`:
- `web.snapchat.com/context/spotlight`
- `web.snapchat.com/spotlight` (catch navigation to the spotlight route directly)

Rules:
```json
[
  {
    "id": 1,
    "priority": 1,
    "action": { "type": "block" },
    "condition": {
      "urlFilter": "web.snapchat.com/context/spotlight",
      "resourceTypes": ["xmlhttprequest", "fetch", "main_frame", "sub_frame"]
    }
  },
  {
    "id": 2,
    "priority": 1,
    "action": { "type": "block" },
    "condition": {
      "urlFilter": "web.snapchat.com/spotlight",
      "resourceTypes": ["xmlhttprequest", "fetch", "main_frame", "sub_frame"]
    }
  }
]
```


## Acceptance Criteria
- [ ] Network tab in DevTools shows requests to `web.snapchat.com/context/spotlight` are blocked (status: `(blocked:extension)`)
- [ ] All other Snapchat functionality (chat, stories, camera) is unaffected
- [ ] Extension loads without errors in `chrome://extensions`

## How to Load the Extension
1. Open `chrome://extensions`
2. Enable **Developer mode** (toggle, top-right)
3. Click **Load unpacked**
4. Select the `snapchat-spotlight-blocker/` directory

## Notes for Claude Code
- Do not add any background service worker — `declarativeNetRequest` works without one
- Do not request any permissions beyond what is listed above
