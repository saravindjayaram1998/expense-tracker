# Spendify

Personal & shared expense tracker. Category auto-detection, trip/group expense splitting, recurring expense reminders, and basic charts.

## Structure
- `www/` — the app itself (HTML/CSS/JS, offline-capable via service worker)
- `capacitor.config.json`, `package.json` — Capacitor project config, for wrapping as a native Android/iOS app
- `BUILD_GUIDE.md` — step-by-step instructions to turn this into an installable `.apk` (Android) and notes on iOS

## Quick start (just the web app)
Open `www/index.html` in a browser. No build step required. Data is stored locally in the browser.

## Building the native app
See `BUILD_GUIDE.md` for the full walkthrough, either the fast path (PWABuilder, no local tools needed) or the full native path (Capacitor + Android Studio, gives you real background notifications).

## Status
v2 prototype. Data storage is local-only for now (localStorage); cloud sync (Firebase) is a planned next step, see the PRD conversation history for the roadmap.
