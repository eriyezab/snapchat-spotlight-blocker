# Snapchat Spotlight Blocker

A Chrome extension that blocks Snapchat Spotlight so you can use Snapchat on your laptop without being enticed to scroll.

Spotlight API requests are blocked at the network level, so the feed never loads. The extension targets `web.snapchat.com` only — it has no effect on the mobile app.

https://github.com/user-attachments/assets/4eef9dd9-3dc6-40f3-9239-92857c4359cf

## What it does

- Blocks all network requests to Snapchat's Spotlight API (`/context/spotlight`, `/batch_stories`, etc.)
- Leaves all other Snapchat functionality untouched (chat, stories, camera)

## Installation

This extension is not on the Chrome Web Store. You load it manually:

1. Clone or download this repository
   ```
   git clone https://github.com/eriyeza/snapchat-spotlight-blocker.git
   ```
2. Open Chrome and go to `chrome://extensions`
3. Enable **Developer mode** (toggle in the top-right corner)
4. Click **Load unpacked**
5. Select the `extension/` folder inside this repository
6. Visit `web.snapchat.com` — Spotlight requests will be blocked immediately

To update after pulling new changes, go back to `chrome://extensions` and click the reload icon on the extension card.

## How it works

The extension uses Chrome's `declarativeNetRequest` API (Manifest V3) to block requests by URL pattern — no background service worker needed.

## License

MIT
