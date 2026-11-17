# OpenWrt x86-64 Wrtx

Project for building, maintaining, documenting, and restoring a custom OpenWrt x86-64 system.

## Goals

1. Build reproducible custom OpenWrt firmware images.
2. Keep device configuration, package selections, and automation scripts under version control.
3. Document safe backup and restore procedures.
4. Track firmware builds and releases without committing large disk images to Git.

## Repository structure

- `configs/` - configuration templates and package selections. Never commit passwords, private keys, tokens, or live secrets.
- `docs/` - build, installation, maintenance, backup, and recovery documentation.
- `scripts/` - reviewed automation scripts.
- `backup-notes/` - backup inventory, procedures, and restore checklists. Actual backup images should be stored separately.
- `firmware/` - build manifests, checksums, and release notes. Large firmware images belong in an appropriate release or external storage.

## Safety rules

- Do not store credentials, Wi-Fi passwords, private keys, API tokens, or unredacted configuration exports in this repository.
- Verify target disk names before running destructive commands such as `dd`, formatting, or partitioning.
- Keep at least one tested backup before changing partitions or flashing firmware.
- Record the exact OpenWrt version, target, packages, and build configuration for every release.

## Build status

The initial GitHub Actions ImageBuilder workflow targets OpenWrt 25.12.5, x86/64, generic profile, and the observed Legacy BIOS/ext4 device baseline. See [`docs/imagebuilder-build.md`](docs/imagebuilder-build.md) for the run procedure and safety checks. The initial package selection is in [`configs/packages-imagebuilder.txt`](configs/packages-imagebuilder.txt).

The first build is a baseline, not yet a byte-for-byte reproduction. Kernel modules and third-party themes remain under compatibility review. Do not flash an artifact until its image type, checksum, boot behavior, and required services have been tested on a spare drive.
