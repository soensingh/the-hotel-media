# Announcing a release

These are the maintainer's steps for recording a new app version here once it's live on the App Store and Google Play. Commands assume the repo is `soensingh/the-hotel-media` and that you run them from this repository's folder with the GitHub CLI (`gh`) logged in.

> [!IMPORTANT]
> The apps ship only through the App Store and Google Play. GitHub Releases here carry **release notes only**. Never attach or commit builds (`.ipa`, `.apk`, `.aab`), source code, signing keys, Firebase config files or `.env` files to this repository.

## 1. Before you start

- [ ] The new version is approved and live on **both** stores, or note which one it's on so far.
- [ ] The version number matches the stores, e.g. `1.2.7` → tag `v1.2.7`.

## 2. Update the changelog

In `CHANGELOG.md`, rename `## [Unreleased]` to the new version with the release date, add a fresh empty `## [Unreleased]` above it, and update the comparison links at the bottom. Keep the wording the same as the store's "What's New" text, expanded where it helps.

```sh
git add CHANGELOG.md
git commit -m "Changelog: The Hotel Media 1.2.7"
git push
```

## 3. Publish the release notes

```sh
VERSION=1.2.7
NOTES="/tmp/the-hotel-media-$VERSION-notes.md"
cat > "$NOTES" <<EOF
## What's new

- (paste the bullets from CHANGELOG.md for $VERSION)

## Get it

- iPhone: https://apps.apple.com/in/app/the-hotel-media/id6670192036
- Android: https://play.google.com/store/apps/details?id=com.thehotelmedia.android

Full changelog: https://github.com/soensingh/the-hotel-media/blob/main/CHANGELOG.md
EOF
open -e "$NOTES"   # fill in the bullets, then save

gh release create "v$VERSION" \
  --repo soensingh/the-hotel-media \
  --title "The Hotel Media $VERSION" \
  --notes-file "$NOTES" \
  --latest
```

Optional: add `--discussion-category "Announcements"` to open an announcement discussion for the release too.

People who choose **Watch → Custom → Releases** on this repo get notified.

## 4. Afterwards

- [ ] Close any issues the release fixed, with a comment naming the version.
- [ ] Post the release in Discussions → Announcements (if you didn't use `--discussion-category`).

## Fixing a release entry

- Typo in the notes: `gh release edit vX.Y.Z --notes-file notes.md`.
- Wrong version: `gh release delete vX.Y.Z --cleanup-tag`, then create it again.
