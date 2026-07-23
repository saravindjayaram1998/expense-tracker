# Spendify — turning this into a real APK (and iOS app)

You picked the free web-build path (PWABuilder), no Android Studio needed. Here's exactly what to do.

## What's in this package
- `www/` — the full app (HTML/CSS/JS), plus `manifest.json`, icons, and a service worker for offline use.
- `capacitor.config.json`, `package.json` — for the native-wrapped version (used if you later want Play Store / deeper native access).
- This guide.

## Path A: fastest, get an installable .apk today (PWABuilder)

1. Host the `www/` folder somewhere public and HTTPS. Easiest options, pick one:
   - Drag the `www` folder into [Netlify Drop](https://app.netlify.com/drop) — gives you a live HTTPS URL in seconds, free, no account strictly required.
   - Or push it to GitHub Pages / Vercel if you already use those.
2. Go to [pwabuilder.com](https://www.pwabuilder.com).
3. Paste your live URL, click **Start**. PWABuilder reads `manifest.json` automatically (already set up with the Spendify name, icons, and colors).
4. Click **Package for stores → Android**.
5. Fill in:
   - Package ID: `com.spendify.app`
   - App name: `Spendify`
   - Leave signing key options on "auto-generate" unless you already have a keystore.
6. Download the generated package. It gives you a **signed .apk** directly, install it on any Android phone (enable "Install unknown apps" for your browser/file manager first, Android will prompt you).

This gets you an installable Android app. It runs the same HTML/JS you've been testing, wrapped as a native shell. **Caveat**: PWABuilder's Android wrapper uses a Trusted Web Activity, which does not include the Capacitor Local Notifications plugin, so background reminders in that specific build will behave like the browser version (fires while the app/tab is active, catches up on reopen), not truly background. For the full native-notification version, use Path B.

## Path B: full native build (real background notifications, more setup)

This uses the Capacitor project already scaffolded here, needed if you want reminders to fire even when the app is fully closed.

1. Install Node.js if you don't have it (nodejs.org).
2. In this folder, run:
   ```
   npm install
   npx cap add android
   npx cap sync
   ```
3. Install [Android Studio](https://developer.android.com/studio) (free).
4. Run `npx cap open android` — opens the project in Android Studio.
5. In Android Studio: **Build → Generate Signed Bundle / APK → APK**. Create a new keystore (save it somewhere safe, you'll need it for future updates), build the release APK.
6. The .apk lands in `android/app/release/`. Copy it to your phone and install.

This is the version with real Capacitor Local Notifications wired in, so recurring reminders survive the app being closed (subject to the phone's battery optimization settings, see note below).

## iPhone: a separate build, not optional

APK cannot run on iOS, period. To get Spendify on iPhone:

- **Requires a Mac.** iOS apps can only be compiled on macOS with Xcode.
- **Requires an Apple Developer account** ($99/year) to install on a physical iPhone beyond a 7-day free provisioning window, or to publish to the App Store.
- Steps once you have both: `npx cap add ios`, then `npx cap open ios`, then build/run from Xcode onto your device or an archive for TestFlight/App Store.
- If you don't have a Mac, services like Codemagic or Ionic Appflow can build the iOS app in the cloud from this same Capacitor project for a fee, still needs your Apple Developer account.

There's no free shortcut here, Apple requires their toolchain and a paid developer account for real device installs.

## One-time step on the Android phone for reliable reminders

After installing the Path B build, Android may kill background timers to save battery. To make sure daily reminders are reliable:
- Go to **Settings → Apps → Spendify → Battery** → set to "Unrestricted" or disable battery optimization for the app.
- This is a per-phone-brand setting (Xiaomi/OnePlus/Samsung all phrase it slightly differently), Spendify's app info screen is where you start.

## Data note

Expenses, trips, budgets, and recurring items are stored locally on-device (localStorage inside the app's webview). Uninstalling the app clears this data. If you want data to survive reinstalls or sync across your Android and iPhone builds, that needs the Firebase step from the original PRD, a good next milestone once the native builds are working.
