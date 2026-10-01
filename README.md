# BuildBook — downloads & update manifest

This is the public download mirror for **BuildBook**, the bill-of-quantities
take-off app for South African construction.

The app's source repository is private. That is deliberate — it is proprietary
and the licence does not grant the right to copy or compile it. But it also
means two things break for people who just want to install the app:

- GitHub answers **404** to anonymous requests for a private repository, so the
  release assets are not publicly downloadable.
- The in-app update check cannot read the private repository's API either, which
  is why it used to fail silently and tell you the app was up to date.

This repository exists to fix both. It holds no source code — only the built
APK and a small manifest.

## Install

Download the latest APK:

```
https://github.com/Davedes83/BuildBook-Updates/releases/latest
```

Then allow "Install unknown apps" for your browser the first time.

## Update manifest

```
https://raw.githubusercontent.com/Davedes83/BuildBook-Updates/main/latest.json
```

The app reads this on launch to tell you when a newer version exists. It is a
static file in the default branch, served without credentials, which is why it
works while the main repository does not.

```json
{
  "version": "0.14.22",
  "versionCode": 54,
  "publishedAt": "2026-10-01T00:00:00Z",
  "apk": "BuildBook-v0.14.22-release.apk",
  "sizeBytes": 1657467,
  "sha256": "…",
  "url": "https://github.com/Davedes83/BuildBook-Updates/releases/download/v0.14.22/BuildBook-v0.14.22-release.apk"
}
```

## Verifying a download

Every release notes the APK's SHA-256, and `latest.json` carries the same value.
To check a file you downloaded:

```
sha256sum BuildBook-v0.14.22-release.apk
```

Windows:

```
certutil -hashfile BuildBook-v0.14.22-release.apk SHA256
```

Compare it with `latest.json`. It should match exactly.

The APK's **signing certificate** is published in the app repository's README.
You can check it with:

```
apksigner verify --print-certs BuildBook.apk
```

The certificate proves which key the APK was signed with. It does not and cannot
give anyone the ability to sign anything — that requires the private key, which
is not and never will be in a public repository.

## How this stays up to date

Nothing here is edited by hand. The main repository's CI publishes the APK to
its own release, then mirrors it here: same tag, APK attached, and
`latest.json` rewritten with the new version, size and SHA-256. If a build ever
fails before this step, this repository keeps pointing at the last version that
did publish — so it fails closed, never forward.

## Licence

BuildBook is proprietary software by David Desousa (Davedes83). This
repository distributes the built application only; it grants no rights to the
source code. See the in-app licence agreement.
