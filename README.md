# Pray Times - Over-The-Air (OTA) Updates Repository

This repository hosts the encrypted application bundles (`app.bin`) and revision metadata (`version.json`) for the Pray Times Android application.

## How OTA Works
1. When the app launches, `VaultManager` checks `https://raw.githubusercontent.com/mohannad-alayash/pray-times-ota/main/version.json`.
2. If `versionCode` is greater than the currently active version, the new encrypted `app.bin` is downloaded over HTTPS.
3. The binary's SHA-256 integrity hash is verified against `version.json`.
4. Upon next launch or live reload, `VaultManager` decrypts the new vault directly into RAM using the AES-256 key protected by the native C++ NDK module (`native-lib.so`).
5. Plaintext code and assets are never written to disk or device storage.

## Files
- `version.json`: Current active version, SHA-256 hash, and download URL.
- `app.bin`: AES-256-CBC encrypted application payload.

## Pushing an Update
After running `npm run package:ota`, simply run:
```bash
git add version.json app.bin
git commit -m "OTA update to new version"
git push origin main
```
