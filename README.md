# talrubin.github.io

Static hosting for the [Yaniv](https://github.com/talrubin/Yaniv) card game's share links. Generated
— don't hand-edit; the sources live in the Yaniv repo under `web/`.

| Path | Source | Why it must be here |
|---|---|---|
| `/.well-known/assetlinks.json` | `web/host-root/` (built by **Yaniv ▸ Build ▸ Write assetlinks.json**) | Android fetches the Digital Asset Links statement from the **host root** and nowhere else. It names the app's package and signing-certificate SHA-256; without it the app's `autoVerify` intent-filter silently fails and a shared link opens in Chrome with the app never offered. |
| `/Yaniv/r/index.html` | `web/link/index.html` | The page a share link points at, for anyone who doesn't have the app — or isn't on Android. The room code rides in the URL **fragment**, which browsers never send to a server, so no room code or password ever reaches these access logs. |
| `/.nojekyll` | — | GitHub Pages runs Jekyll by default and **Jekyll skips directories whose names start with a dot**. Without this marker `/.well-known/` is never published at all, and verification fails with no diagnostic anywhere. |

## Why the landing page is here and not in the Yaniv repo

`talrubin/Yaniv` is private, and GitHub Pages on a private repo needs a paid plan. A *user* site
serves the whole domain, so `/Yaniv/r/` works from here with no code change — `RoomLink.Host` and
`RoomLink.Path` already point at exactly this.

**If `talrubin/Yaniv` ever enables its own Pages site, it takes over `/Yaniv/*` and shadows this
copy.** Move the landing page there at that point, or the two will disagree.

## Checking it

```sh
curl -sS https://talrubin.github.io/.well-known/assetlinks.json
curl -sSI https://talrubin.github.io/Yaniv/r/ | head -1
adb shell pm get-app-links com.talrubin.yaniv          # → verified
```
