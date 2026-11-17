# Inventaris sistem OpenWrt x86-64 Wrtx

Sumber: transkrip diagnostik SSH yang diberikan pemilik proyek. Ini adalah snapshot saat perintah dijalankan, bukan pemeriksaan langsung terhadap router saat ini.

## Identitas sistem

| Item | Nilai |
|---|---|
| Sistem operasi | OpenWrt 25.12.5 |
| Revision | r33051-f5dae5ece4 |
| Arsitektur | x86_64 |
| Kernel | 6.12.94 |
| Package manager | APK |
| APK tools | 3.0.5 |
| BusyBox | 1.37.0 |
| Paket terpasang yang tercatat | 349 |

## Storage dan mount

- `/dev/root` dipasang pada `/` sebagai ext4, read-write.
- `/dev/sdb1` dipasang pada `/boot` sebagai ext4.
- `/dev/sda1` dipasang pada `/mnt/storage` sebagai ext4.
- Terdapat mount `proc`, `sysfs`, `tmpfs`, `devpts`, `debugfs`, dan `bpffs`.
- Device yang terlihat: `/dev/sda`, `/dev/sda1`, `/dev/sdb`, `/dev/sdb1`, dan `/dev/sdb2`.

Catatan: transkrip menampilkan `/dev/sdb1` dua kali pada output mount. Layout disk perlu diperiksa ulang sebelum prosedur backup/restore dibuat.

## Driver jaringan

| Interface | Driver | Versi | Bus |
|---|---|---|---|
| eth0 | atl1c | 6.12.94 | 0000:01:00.0 |
| eth1 | cdc_ether | 6.12.94 | usb-0000:00:1d.7-1 |

## Komponen penting yang terdeteksi

Daftar paket mencakup LuCI, firewall4/nftables, dnsmasq, PPP/PPPoE, SQM/CAKE, Samba4, ADB, curl, iperf3, nmap, speedtest-go, FFmpeg, ALSA, dan Python 3.

Modul kernel mencakup antara lain ath9k, atl1c, r8169, e1000/e1000e, igb/igc, ixgbe, bnx2, USB networking, storage, DRM/i915, audio, dan ZRAM.

## Temuan yang perlu ditindaklanjuti

1. Pembacaan `/etc/apk/repositories` gagal karena file tersebut tidak ditemukan. Lokasi/konfigurasi repository APK perlu diperiksa.
2. Ambil ekspor `/etc/config` yang telah disanitasi untuk merekam konfigurasi aktual.
3. Simpan manifest paket dan konfigurasi build firmware agar firmware custom dapat dibangun ulang.
4. Identifikasi disk dan partisi secara lengkap sebelum membuat image backup. Jangan menyimpulkan ukuran atau peran partisi hanya dari nama device pada snapshot.
5. Snapshot ini bukan backup yang dapat dipulihkan. Snapshot tidak memuat konfigurasi UCI lengkap, kredensial, private key, atau image firmware/disk.

## Privasi

Baris perintah login SSH dan label perangkat khusus tidak disertakan. Password dan private key tidak disalin.
