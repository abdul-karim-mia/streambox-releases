# StreamBox Releases

Public releases-only repo for the StreamBox Android app (app source stays private).

- `version.json` — update manifest the app checks at startup and from Settings → App Updates
- GitHub Releases — host the signed release APKs

## Release checklist

1. Bump `versionCode` (+1) and `versionName` in the app repo's `app/build.gradle.kts`.
2. Build: `.\gradlew.bat assembleRelease`
3. Compute APK checksum: `Get-FileHash app\build\outputs\apk\release\app-release.apk -Algorithm SHA256`
4. Tag the app repo: `git tag v<versionName> && git push --tags`
5. Create a GitHub Release in this repo named `v<versionName>`, upload `app-release.apk`.
6. Update `version.json`:
   - `versionCode`, `versionName`
   - `apkUrl` = the new release asset URL
   - `sha256` = hash from step 3 (lowercase hex)
   - `changelog` = short user-facing notes
   - `minVersionCode` unchanged unless a forced update is needed
7. Commit and push `version.json`.

## Manifest fields

| Field | Meaning |
|---|---|
| `versionCode` | Integer, must be > app's `BuildConfig.VERSION_CODE` to be offered |
| `versionName` | Display name for the update prompt |
| `apkUrl` | Direct APK download URL (release asset) |
| `sha256` | Expected APK hash; mismatch aborts install |
| `changelog` | Shown in the in-app update dialog |
| `minVersionCode` | Apps below this get a "must update" (non-dismissable) prompt |
