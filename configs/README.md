# Configuration templates

Store sanitized, reusable configuration templates here.

Do not commit a live `/etc/config` export until it has been reviewed and all secrets, passwords, private keys, public IP details, and personal data have been removed.

Recommended future files:
- `.config` or a package-selection manifest for reproducible builds.
- Sanitized UCI configuration examples.
- A README describing which device and OpenWrt release each template targets.
