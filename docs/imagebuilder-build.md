# ImageBuilder build procedure

## Baseline

- OpenWrt: 25.12.5
- Target: x86/64
- ImageBuilder profile: `generic`
- Device boot mode observed: Legacy BIOS
- Root filesystem observed: ext4
- Build host: GitHub Actions, Ubuntu 24.04

The workflow downloads the official OpenWrt 25.12.5 x86/64 ImageBuilder archive and verifies its SHA-256 before extraction.

## Run

1. Open the repository's **Actions** tab.
2. Select **Build OpenWrt x86-64 Wrtx**.
3. Select **Run workflow** on branch `main`.
4. Wait for the job to finish.
5. Open the completed run and download artifact `OpenWrt-Wrtx-25.12.5-x86-64`.

A push changing the package manifest or workflow file also starts a build.

## Package selection policy

`configs/packages-imagebuilder.txt` is a curated starting list based on the router's APK world file. It is not a copy of every installed package.

- ImageBuilder owns the base system, kernel, and matching core libraries. Version-pinned `base-files`, `kernel`, and `libc` entries are not passed as package requests.
- Kernel modules are held back until the exact ImageBuilder kernel ABI and required hardware drivers are checked.
- Third-party or theme packages are held back until their exact package source and version are pinned.
- The historical installed-package inventory is documentation, not a build manifest.

If a package is unavailable in the release feeds, the build log will identify it. Remove or replace only that package after reviewing the log; do not silently drop broad groups of functionality.

## Output and safety

The artifact contains generated image files, available manifests, `SHA256SUMS`, and `BUILD-INFO.txt`. Artifacts expire after 30 days.

The workflow does not create a GitHub Release, write to the router, or flash any disk. Inspect the image type and checksum before any later restore. Test restoration on a spare disk first, never on the running router's boot disk.

## Next compatibility pass

Before calling this a faithful rebuild, validate:
1. The exact NIC, USB, storage, and other required kernel modules against the ImageBuilder's kernel ABI.
2. Availability and provenance of the Proton2025 and Footstrap themes.
3. The final image's boot mode, partition layout, and required services.
4. A successful boot and network test on a spare drive.
