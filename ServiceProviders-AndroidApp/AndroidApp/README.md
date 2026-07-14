# Service Providers — Android App

This is a ready-to-build **Android Studio project** that wraps your hosted
website (`https://service-providers.infinityfreeapp.com/index.php`) as a
native-feeling Android app.

## Why a wrapper app, and not a full rewrite?

Your project is a **PHP + MySQL** web app. PHP only runs on a server — it
can't run inside an Android app. Since your site is already hosted online,
the fast, reliable way to turn it into an app is to load it inside a
native Android `WebView`. That's what this project does, plus the extras
a plain browser tab wouldn't give you:

- Full-screen, no browser address bar — looks and feels like a real app
- Splash-free instant load, pull-to-refresh, and a proper offline screen
- Back button navigates your site's history before exiting the app
- File uploads work (e.g. profile photo / booking proof forms)
- Location permission handling (for `map.html`)
- `tel:`, `mailto:`, `whatsapp:`, `upi:` links open the right app on the
  phone instead of failing inside the WebView (useful for your payment
  and WhatsApp/SMS features)
- Downloads (e.g. receipts) save via the system Download Manager
- App icon generated from your own `favicon-image.png`

## Option A — Get an APK with zero installs (GitHub Actions)

This project includes `.github/workflows/build-apk.yml`, which makes
GitHub's own servers build the APK for you automatically — you don't
need Android Studio or anything installed on your PC.

1. Create a free account at https://github.com if you don't have one.
2. Create a new repository (public or private, either works).
3. Upload this entire `AndroidApp` folder to that repository (drag-and-drop
   works fine on github.com, or use `git push` if you're comfortable with git).
4. Go to the repo's **Actions** tab. A workflow called "Build APK" will
   run automatically (takes ~3-5 minutes).
5. When it finishes, click into that workflow run → scroll to
   **Artifacts** → download **ServiceProviders-debug-apk**. Unzip it and
   you'll have `app-debug.apk` — install it on any Android phone
   (you'll need to allow "install from unknown sources" once, since it's
   not from the Play Store).

This gives you a real, installable APK without touching Android Studio.

## Option B — Build it yourself in Android Studio

1. Install **Android Studio** (free): https://developer.android.com/studio
2. Open Android Studio → **Open** → select this `AndroidApp` folder.
3. Let it sync (first sync downloads Gradle + dependencies — needs internet).
   If it asks about the Gradle wrapper, click **OK / Use Gradle from Android
   Studio** and let it proceed.
4. Once synced, click the green **Run ▶** button with a phone/emulator
   connected — or go to **Build → Generate Signed Bundle / APK** to produce
   a `.apk` you can share or upload to the Play Store.

That's it — no coding required to get a working app.

## The one line you can change

Open `app/src/main/res/values/strings.xml` and edit:

```xml
<string name="base_url">https://service-providers.infinityfreeapp.com/index.php</string>
```

This is the only thing you need to touch if your site's URL ever changes.

## Notes about your PHP site itself

A few things worth checking on the server side so the app behaves well:

- Make sure the site works correctly over **HTTPS** (InfinityFree gives you
  a free SSL cert — turn it on in the control panel if you haven't).
- Any `header('Location: ...')` redirects, session cookies, and file
  upload forms will all work fine inside the WebView since it's a real
  browser engine under the hood.
- Since InfinityFree is a free host, expect occasional slow loads or
  downtime — this is a hosting limitation, not something the app can fix.

## Want a true native app instead?

If down the line you want real native screens (faster, works offline,
push notifications, no dependency on the PHP pages being reachable), the
path is: turn your PHP files into a JSON API (most of them already read
from MySQL and could return `json_encode(...)` instead of HTML), then
build native Kotlin screens that call that API. That's a much bigger
project than this wrapper — happy to help you start on it whenever you're
ready, one screen/endpoint at a time.
