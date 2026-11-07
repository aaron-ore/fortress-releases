# Fortress desktop updates

This repository distributes Fortress POS Windows installers and update metadata. Application source, merchant records, employee credentials, and Supabase secrets do not belong here.

No desktop release is published yet. Update delivery starts with the first signed NSIS installer that includes the updater.

## Merchant experience

1. Fortress checks for new stable releases in the background.
2. A manager chooses Download update in Settings.
3. Fortress verifies the Windows publisher signature.
4. After finishing or holding the basket, resolving payments, and closing the shift, the manager chooses Restart and update.
5. Fortress verifies local database backups and runs the update installer in place. Merchants do not uninstall the old version.

Ordinary app shutdown does not install pending updates. Cashiers cannot initiate updates. Portable builds do not support this updater.

## Preparing a release

Use the private/local Fortress source project and its `release:prepare` command with a higher stable version and Windows signing credentials. The release configuration pins this GitHub repository and the certificate publisher, requires signing, and generates the NSIS installer, `latest.yml`, and blockmap.

Use `release:stage` to upload only those generated release artifacts as a draft. Review the draft and publish when the release has passed a signed upgrade test on a separate Windows test machine. Do not upload source archives or credentials to this repository.

The existing 0.1.1 preview installer has no updater. Its first transition requires running the new signed installer over the existing installation. Later versions can be installed through Settings without uninstalling Fortress.

Keep the application ID, product name, installation scope, and local data directory stable. Test database migrations with a copy of existing merchant data. Backups are recovery aids; this implementation does not promise automatic rollback after a bad application release.

References: [Electron Builder v26 auto update](https://www.electron.build/v26/docs/features/auto-update/), [Windows signature verification](https://www.electron.build/v26/docs/win/).
