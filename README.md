# Old is Gold — Vinyl / Playlist Player (Web + Android APK)

A single-file HTML music player, now packaged so GitHub can build you an
Android **APK** automatically — you don't need Android Studio installed.

## Project structure

```
old-is-gold-player/
├─ index.html              ← standalone web version (open directly in a browser)
├─ www/
│  └─ index.html            ← same file, used as the Android app's content
├─ config.xml               ← Cordova app config (name, permissions, allowed domains)
├─ package.json
├─ .gitignore
└─ .github/
   └─ workflows/
      └─ build-apk.yml      ← GitHub Actions workflow that builds the APK
```

> **Important:** when you push to GitHub, commit the *contents* of this
> folder directly into your repo — don't wrap it in an extra folder.
> `config.xml` should sit at the repo root (e.g.
> `github.com/you/your-repo/blob/main/config.xml`), not nested inside
> `github.com/you/your-repo/blob/main/old-is-gold-player/config.xml`.
> If `git add .` is run from *inside* this folder (as in the steps below),
> you'll get this right automatically. The build workflow will also
> auto-detect `config.xml` even if it does end up nested, so a build
> won't fail outright either way — but a flat layout keeps things simple.

## Getting an APK from GitHub (no Android Studio needed)

1. **Push this folder to a new GitHub repo:**
   ```bash
   cd old-is-gold-player
   git init
   git add .
   git commit -m "Old is Gold player + APK build workflow"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<your-repo>.git
   git push -u origin main
   ```

2. **Let the workflow run.** As soon as you push to `main`, GitHub Actions
   automatically starts building the APK. Go to your repo → **Actions**
   tab → you'll see a run called "Build Android APK" in progress (takes
   roughly 3–6 minutes).

3. **Download the APK:**
   - Click into that completed workflow run.
   - Scroll down to the **Artifacts** section at the bottom of the run
     page.
   - Click **old-is-gold-apk** to download a zip — inside is
     `app-debug.apk`.

4. **Install it on your phone:**
   - Copy the `.apk` to your Android phone (or open the download link on
     the phone's browser directly).
   - Tap it to install. Android will ask you to allow "install from
     unknown sources" the first time — that's expected for a debug build
     not published on the Play Store.

Re-running: every time you push a new commit to `main`, a fresh APK is
built automatically. You can also trigger a build manually from the
**Actions** tab using **Run workflow** (this repo's workflow has
`workflow_dispatch` enabled).

## Important notes

- This is a **debug** APK — fine for installing on your own device and
  testing, but it's not signed for the Play Store. Publishing to the Play
  Store needs a release signing key, which is a separate step.
- The app is a WebView wrapper around `www/index.html`. It needs an
  active internet connection on the phone to load the YouTube player,
  same as the web version.
- Only one video source is used throughout (`sCIiVN0zIGE`); the playlist
  entries are just different start-time offsets into that same video.

## Run the web version locally (unchanged)

Just open `index.html` in a browser — no server or build step needed for
that version.

## Deploy the web version on GitHub Pages (unchanged)

1. Settings → Pages → Source: Deploy from a branch → `main`, `/ (root)`.
2. GitHub gives you a live URL:
   `https://<your-username>.github.io/<your-repo>/`
