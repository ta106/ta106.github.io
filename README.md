# ta106.github.io

User-level GitHub Pages site for **Sufra** share links.

This repo MUST be public (GH Pages free tier requires public). Once Pages is enabled, content here is served at the root of `https://ta106.github.io/`.

## What's hosted here

| URL | File |
|---|---|
| `https://ta106.github.io/.well-known/apple-app-site-association` | `.well-known/apple-app-site-association` — iOS Universal Links manifest. Apple fetches this at the root of the domain (NOT a subpath), which is why we need a user-level Pages repo rather than a project repo. |
| `https://ta106.github.io/.well-known/assetlinks.json` | `.well-known/assetlinks.json` — Android App Links manifest. Placeholder fingerprint until Android signing is set up. |
| `https://ta106.github.io/s/{linkId}` | `s/index.html` — fallback page for share links. Single file reads the path; on iOS/Android tries `sufra://` first, falls back to App Store / Play Store. |

## Enable GitHub Pages

1. Push this repo to `github.com/ta106/ta106.github.io` (must be PUBLIC).
2. Repo Settings → Pages → Source: **Deploy from a branch**, branch `main`, folder `/` → Save.
3. Wait ~1 minute, then verify:
   ```
   curl -I https://ta106.github.io/.well-known/apple-app-site-association
   ```
   Expect `HTTP/2 200` and `Content-Type: application/json`.

## Wired into

- `ios/Sufra/Sufra.entitlements` → `applinks:ta106.github.io`
- `app/services/sharing.ts` → `SHARE_BASE_URL = 'https://ta106.github.io/s'`

## Placeholders to update before App Store submission

| Where | Placeholder | Replace with |
|---|---|---|
| `.well-known/apple-app-site-association` | `8Y5U2672GW.org.reactjs.native.example.Sufra` | Your `TEAMID.<bundle-id>` once the bundle ID is changed from the RN-template default |
| `.well-known/assetlinks.json` | `REPLACE_WITH_RELEASE_KEY_SHA256_FINGERPRINT` | `keytool -list -v -keystore <release.keystore> \| grep SHA256` |
| `s/index.html` | `idPLACEHOLDER` and `com.sufra.app` | Real App Store ID and Android package name when published |

## Validating

- AASA validator (must be public): https://branch.io/resources/aasa-validator/
- Universal-link debugging on iOS: paste a `https://ta106.github.io/s/{any-id}` URL into the Notes app, long-press it; the menu should say "Open in Sufra" once the app is installed and AASA is cached. iOS caches AASA via Apple's CDN — to force a re-fetch on a dev device, delete the app, reboot, then reinstall.
