# Ancient apps — webOS Homebrew Channel repository

Self-hosted [webOS Homebrew Channel](https://github.com/webosbrew/webos-homebrew-channel) package repository containing:

| App | Package ID | Source |
| --- | --- | --- |
| **AncientTwitchWebOSTV** | `com.tamulus.ancienttwitchwebostv` | [Tamulus47/SmartTwitchWebOSTV](https://github.com/Tamulus47/SmartTwitchWebOSTV) |
| **Kick TV** | `com.tamulus.ancientkickwebostv` | [Tamulus47/KickWebOSTV](https://github.com/Tamulus47/KickWebOSTV) |

The package format follows [webosbrew/apps-repo](https://github.com/webosbrew/apps-repo) conventions — see the
[Publishing guide](https://www.webosbrew.org/develop/guides/publishing/) for the field reference. This is a
personal repository, not affiliated with the official webosbrew repository.

## Add this repository on your TV

1. Open **Homebrew Channel** and go to **Settings** (gear icon).
2. Choose **Add repository** and enter exactly:

   ```
   https://raw.githubusercontent.com/Tamulus47/apps-repo/main/manifest.json
   ```

3. Go back to **Browse**. Both apps appear in the list, marked with
   `(from https://raw.githubusercontent.com/Tamulus47/apps-repo/main/manifest.json)`.
4. Open an app and choose **Install**. Homebrew Channel verifies the SHA-256 of the
   IPK before installing it.

One-click alternative, from a PC on the same network (needs `ares` / Developer Mode):

```sh
ares-launch org.webosbrew.hbchannel -p '{"launchMode":"addRepository","url":"https://raw.githubusercontent.com/Tamulus47/apps-repo/main/manifest.json"}'
```

## Updating an app

1. Build the new IPK in the app's own repo
   (`npm run webos:package` for AncientTwitchWebOSTV, `npm run package` for Kick TV).
2. Copy it into `apps/`, keeping the `<id>_<version>_all.ipk` naming.
3. Compute the hash: `certutil -hashfile apps\<file>.ipk SHA256` (Windows) or `sha256sum apps/<file>.ipk`.
4. Edit `manifest.json` — inside the app's entry update `manifest.version`, `manifest.ipkUrl`,
   `manifest.ipkHash.sha256` and `manifest.ipkSize` (the index embeds each manifest inline,
   so there is only one place to edit).
5. Commit and push. Raw URLs always serve `main`, so the TV picks the update up on the next
   Browse refresh — no release or extra step needed.

## Layout

- `manifest.json` — repository index; this is the URL you paste into Homebrew Channel.
  It embeds each app's manifest inline (the format the Homebrew Channel accepts directly).
- `apps/*.ipk` — installable packages.
- `apps/*.description.html` — rich store descriptions (`fullDescriptionUrl`).
- `icons/` — icons that are not hosted by the app's own repository.
