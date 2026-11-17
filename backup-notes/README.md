# Backup and recovery notes

This directory documents backup inventory and safe recovery procedures. It is not intended for storing large disk images in Git.

Before creating a full-disk image:
1. Identify the source disk by model, size, and partition layout.
2. Identify the destination separately.
3. Confirm the destination has enough free space.
4. Ensure the destination is not the source disk.
5. Record the command, date, image size, and checksum.
6. Verify the image before relying on it.

Never run a restore or overwrite command until source and destination devices have been independently verified.
