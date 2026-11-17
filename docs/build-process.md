# Reproducible firmware build process

This is the project workflow outline. Device-specific commands will be added after the hardware and OpenWrt release are confirmed.

1. Confirm the x86-64 hardware, boot mode, storage layout, and network interfaces.
2. Select the exact OpenWrt release and source revision.
3. Define the target, subtarget, profile, and package set.
4. Keep build configuration and package manifests under version control.
5. Build in a clean, documented environment.
6. Record image names, sizes, and SHA-256 checksums.
7. Test boot, storage, Ethernet, wireless (if present), and recovery.
8. Publish a build record and release artifacts.

Do not flash an image until its target and installation method have been verified.
