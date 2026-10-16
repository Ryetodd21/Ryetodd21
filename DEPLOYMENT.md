# Deployment Guide

How to ship this profile to your GitHub at **github.com/Ryetodd21**.

---

## 1. Push the files to your profile repo

Your special repo `Ryetodd21/Ryetodd21` is what GitHub uses as the profile README.

```bash
# from the project folder
cd ryetodd21-profile

git init -b main
git add .
git commit -m "feat: ui/ux pro max profile redesign"
git remote add origin https://github.com/Ryetodd21/Ryetodd21.git
git push -u origin main --force
```

If the repo already has content, `--force` is fine here because this is a *profile* repo — the README is its only product.

---

## 2. File map (what goes where)

| File                        | Purpose                                          |
| --------------------------- | ------------------------------------------------ |
| `README.md`                 | The actual GitHub profile content                |
| `banner.svg`         | Animated SVG banner shown at the top of the README |
| `index.html`                | Standalone interactive preview (open locally)    |
| `DEPLOYMENT.md`             | This file                                        |

> The README references `assets/banner.svg` via raw.githubusercontent — **keep the folder name lowercase**.

---

## 3. Optional: enable the contribution snake

The snake widget is referenced in the README but only works if you set up a GitHub Action.

1. Make sure the repo has a folder `.github/workflows/` (create it if not).
2. Add a file `.github/workflows/snake.yml`:

   ```yaml
   name: Generate snake
   on:
     schedule:
       - cron: "0 0 * * *"
     workflow_dispatch:
   jobs:
     generate:
       runs-on: ubuntu-latest
       permissions:
         contents: write
       steps:
         - uses: actions/checkout@v4
         - uses: Platane/snk@v3
           with:
             github_user_name: Ryetodd21
             outputs: |
               ./github-contribution-grid-snake.svg
               ./github-contribution-grid-snake-dark.svg
         - uses: actions/upload-pages-artifact@v3
           with:
             path: .
         - id: deployment
           uses: actions/deploy-pages@v4
   ```

3. Enable **Settings → Pages → Build and deployment → GitHub Actions**.

After the workflow runs once, the snake SVG is published at the path the README references.

---

## 4. Optional: enable the 3D profile card

1. Make a folder `profile-3d-contrib/` at the repo root.
2. Add a GitHub Action (`.github/workflows/profile-3d.yml`):

   ```yaml
   name: 3D Profile
   on:
     schedule:
       - cron: "0 0 * * *"
     workflow_dispatch:
   jobs:
     build:
       runs-on: ubuntu-latest
       permissions:
         contents: write
       steps:
         - uses: actions/checkout@v4
         - uses: yyx990803/profile-3d-contrib@main
           with:
             username: Ryetodd21
           env:
             GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
         - run: |
             mkdir -p profile-3d-contrib
             mv *.svg profile-3d-contrib/ 2>/dev/null || true
         - uses: stefanzweifel/git-auto-commit-action@v5
           with:
             commit_message: "chore: refresh 3D profile"
   ```

---

## 5. The widgets I wired in

| Widget                | Source                                                  | Needs setup? |
| --------------------- | ------------------------------------------------------- | ------------ |
| Banner (SVG)          | local `assets/banner.svg`                               | ✅ already included |
| Typing animation      | [readme-typing-svg](https://readme-typing-svg.demolab.com) | ❌ no         |
| Profile views         | [komarev/ghpvc](https://komarev.com/ghpvc)              | ❌ no         |
| Followers / stars     | [shields.io](https://shields.io)                        | ❌ no         |
| Tech icons            | [skillicons.dev](https://skillicons.dev)                | ❌ no         |
| GitHub stats          | [github-readme-stats](https://github.com/anuraghazra/github-readme-stats) | ❌ no (uses default API) |
| Streak stats          | [github-readme-streak-stats](https://github.com/DenverCoder1/github-readme-streak-stats) | ❌ no (uses default API) |
| Trophies              | [github-profile-trophy](https://github.com/ryo-ma/github-profile-trophy) | ❌ no         |
| Snake                 | [Platane/snk](https://github.com/Platane/snk)          | ✅ needs Action |
| 3D profile            | [yyx990803/profile-3d-contrib](https://github.com/yyx990803/profile-3d-contrib) | ✅ needs Action |
| Quote                 | [quotes-github-readme](https://github.com/PiyushSuthar/github-quotes) | ❌ no         |
| Footer wave           | [capsule-render](https://github.com/kyechan99/capsule-render) | ❌ no         |

All third-party services are rate-limited — they may rate-cap anonymous users. For higher rate limits, deploy `github-readme-stats` and `streak-stats` yourself with your own Vercel token.

---

## 6. Preview the design locally

Just open `index.html` in your browser:

```bash
# from the project folder
start index.html   # Windows
# or
open index.html    # macOS
```

The HTML preview has extra goodies the GitHub README can't show:

- 🚀 Boot-sequence loading screen (`hary.sys v1.0.0` with status lines + progress bar)
- 🖱️  Custom cursor with lerp ring + hover state (desktop only)
- 🧲  Magnetic side nav (8 sections, mouse-attracted)
- 📱  **App Drawer** — 18 Android 13+ Material You–style 3D icons (squircle gradient + glossy highlight + tilt 3D + ripple on click)
- 🃏  3D tilt card with holographic gradient and orbit ring
- 📊  Animated activity bars (fill on scroll)
- 🏃  Marquee fun-facts track
- 🌊  Animated 3D grid background + particle canvas with mouse repulsion
- 🌗  Dark / light theme toggle (persists in localStorage)
- 👁️  Reveal-on-scroll animations
- ✨  Ripple click effect on every interactive element
- 📈  Scroll progress bar

> **Caveat:** the App Drawer icons load from `cdn.jsdelivr.net` (simple-icons). Offline preview still works — the 3D cards stay visible, only the inner logo `<img>` is blank. No other feature depends on the network.

---

## 7. Customise

- **Change the colors** → search & replace `#00ffaa` (primary), `#ff006e` (secondary), `#00f5d4` (tertiary).
- **Change the typing lines** → edit the `lines` query param in the `readme-typing-svg` URL.
- **Change the skill icons** → edit the `skillicons.dev` `?i=` list.
- **Add a real Spotify / WakaTime widget** → drop the token into the widget's URL when you have one set up.

---

Done. Refresh your GitHub profile and the new look should be live within a minute.
