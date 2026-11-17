# Device profile: Acer AOHAPPY

## Confirmed running system

- Vendor/model reported by `ubus call system board`: Acer AOHAPPY
- CPU: Intel Atom N570 @ 1.66 GHz
- OpenWrt release: 25.12.5
- OpenWrt revision: r33051-f5dae5ece4
- Target: x86/64
- Package architecture: x86_64
- Kernel: 6.12.94
- Root filesystem: ext4
- Package manager: apk
- Firmware hostname: x86_64_Wrtx

## Boot and storage observations

- Running root device was identified as `/dev/sdb2`.
- Boot partition was mounted at `/boot` from `/dev/sdb1`.
- The USB disk uses an MBR/DOS partition table.
- `grub2-bios-setup` is in the explicit APK world list.
- These observations strongly suggest a legacy-BIOS image, but the actual boot mode should be confirmed before selecting a final image type.

## Build baseline

The first reproducibility build should target the exact running baseline:

- Release: `25.12.5`
- Target/subtarget: `x86/64`
- Generic x86/64 profile
- Kernel ABI observed on the device: `6.12.94-1-a7bc15f451f9652701ba04af9cfb0b95`

Do not mix kernel modules from another OpenWrt release or kernel ABI.

## Important limitations

This file records observed hardware and release facts. It is not a complete device configuration or a verified build configuration. Network settings, Wi-Fi credentials, private keys, Samba secrets, and other live configuration are intentionally excluded.

The explicit APK world list contains package names that still need validation against the official ImageBuilder package index. In particular, custom-looking LuCI themes must be checked for availability before they are included in an automated build.
