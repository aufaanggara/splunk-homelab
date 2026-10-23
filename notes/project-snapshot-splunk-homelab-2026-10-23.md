## 🗂️ Project Snapshot — Splunk Home Lab

> Deskripsi project, siap dipakai sebagai context di LinkedIn, CV, resume, atau web portofolio.

**Nama Project:** Splunk Home Lab (SIEM Log Collection & Detection)
**Periode:** Agustus 2026 — masih berjalan (sempat jeda sekitar 2,5 bulan)
**Kategori:** Blue Team / SIEM / Infrastructure
**Stack/Tools:** Splunk Enterprise 10.4.2, Splunk Universal Forwarder 10.4.2, VirtualBox, Ubuntu Server 24.04 LTS, Windows (endpoint), Kali Linux 2025.4, OpenSSH, Netplan, Wazuh agent (dari lab sebelumnya, berjalan berdampingan di endpoint)

**Deskripsi:**
Membangun lab SIEM berbasis Splunk Enterprise dari nol di VirtualBox untuk belajar pengumpulan log dengan Universal Forwarder, analisis dengan SPL, serta pembuatan dashboard dan alert dari simulasi serangan (Kali Linux). Endpoint yang sama dengan lab Wazuh sebelumnya dipakai ulang agar log yang sama bisa dibandingkan di dua SIEM. Seluruh proses didokumentasikan bertahap dengan screenshot untuk dijadikan writeup.

**Yang sudah dikerjakan:**
- Merancang arsitektur lab: Splunk server terpisah dari Wazuh, endpoint dan Kali dipakai ulang
- Menyusun struktur repo `splunk-homelab` dan konvensi dokumentasi screenshot per tahap
- Membangun VM `splunk-server` (Ubuntu Server 24.04, 8 GB RAM, 4 CPU, 50 GB dinamis) dengan dua adapter (NAT dan Host-only `192.168.56.0/24`)
- Mendiagnosis dan memperbaiki adapter Host-only yang tidak dapat IP (interface tidak terdaftar di netplan setelah unattended install)
- Mengganti mirror apt yang menolak koneksi (`id.archive.ubuntu.com` ke `archive.ubuntu.com`) dan mengaktifkan OpenSSH
- Menginstal Splunk Enterprise 10.4.2 via `.deb`, membuat akun admin, mengakses web UI, dan menjalankan `enable boot-start`
- Mengaktifkan receiving port 9997 dan memverifikasinya dengan `netstat` dan `ss`
- Menginstal Splunk Universal Forwarder 10.4.2 di `endpoint-linux-waz-plunk` (berjalan sebagai service systemd dengan user `splunkfwd`)

**Angka & bukti konkret:**
- 4 VM dalam topologi (1 Splunk server, 2 endpoint, 1 attacker); 2 VM dikonfigurasi baru/diubah di sesi ini (splunk-server dan endpoint Linux)
- 1 Splunk Enterprise + 1 Universal Forwarder terpasang dan berjalan
- 6 masalah teknis didiagnosis dan diselesaikan (soft lockup saat instalasi, adapter Host-only tanpa IP, mirror apt, hostname, boot-start, kredensial Enterprise vs UF tertukar)
- Sekitar 22 screenshot terdokumentasi dengan penamaan konsisten
- UF memakai sekitar 78 MB RAM (pembanding: Splunk server dialokasikan 8 GB)
- Belum ada angka alert, rule, atau skenario serangan karena tahap tersebut belum dijalankan

**5 Screenshot Terpenting:**

| No | Nama File (dengan path folder) | Apa yang ditunjukkan |
|---|---|---|
| 1 | `docs/setup/01-splunk-server/14-splunk-dashboard-login-success` | Dashboard Splunk Enterprise berhasil diakses dan login dari host |
| 2 | `docs/setup/01-splunk-server/16-verify-port-9997-listening` | Port 9997 listening oleh `splunkd`, siap menerima data dari forwarder |
| 3 | `docs/setup/03-forwarder-ubuntu/04-uf-systemd-active-enabled` | Universal Forwarder aktif dan enabled sebagai service systemd |
| 4 | `docs/setup/00-network/05-netplan-fix-dan-verifikasi-ip` | Perbaikan netplan dan interface Host-only mendapat IP `192.168.56.108` |
| 5 | `docs/setup/00-network/06-ping-endpoint-linux-wazuh` | Konektivitas Host-only antara Splunk server dan endpoint Linux terverifikasi |

---

## 📋 Project Progress Checklist — Splunk Home Lab

**Terakhir diupdate:** 23 Oktober 2026

### Persiapan & Dokumentasi

| Status | Item | Catatan |
|---|---|---|
| ✅ | Struktur folder `splunk-homelab` | Screenshot `docs/00-struktur-folder` |
| ✅ | Keputusan arsitektur (Splunk terpisah dari Wazuh, reuse endpoint dan Kali) | Disepakati di awal |
| ✅ | Konvensi screenshot (tanpa `.png`, sebar per tahap, urutan 2 digit) | Dipakai konsisten |
| ✅ | Draft template proyek (24 poin) | Bagian hasil masih berstatus rencana |

### Tahap 1: Jaringan

| Status | Item | Catatan |
|---|---|---|
| ✅ | Putuskan memakai network lama (NAT + Host-only `192.168.56.0/24`) | |
| ✅ | Dokumentasi adapter Kali, Ubuntu, Windows | Screenshot 00 sampai 02 |
| ✅ | Troubleshoot `enp0s8` DOWN, tambah ke netplan (DHCP) | Screenshot 03 sampai 05, IP `.108` |
| ✅ | Ping dari splunk-server ke endpoint Linux (`.106`) dan Windows (`.109`) | Screenshot 06 dan 07 |

### Tahap 2: VM Splunk Server

| Status | Item | Catatan |
|---|---|---|
| ✅ | Create VM (8 GB RAM, 4 CPU, 50 GB dinamis, Ubuntu 24.04, unattended) | Screenshot 00 sampai 03 |
| ✅ | Install OS sampai login pertama | Sempat soft lockup, pulih sendiri |
| ✅ | Ganti hostname jadi `splunk-server` | |
| ✅ | Ganti mirror apt dan install OpenSSH | Mirror `id.archive` diganti |
| ✅ | SSH dari Windows host berhasil | |

### Tahap 3: Splunk Enterprise

| Status | Item | Catatan |
|---|---|---|
| ✅ | Download dan install Splunk Enterprise 10.4.2 (`.deb`) | |
| ✅ | Start pertama dan buat akun admin (`--run-as-root`) | |
| ✅ | Login web UI `https://192.168.56.108:8000` | Screenshot 13 dan 14 |
| ✅ | `enable boot-start` | Output sukses, belum terbukti saat reboot |
| ✅ | Aktifkan receiving port 9997 | Terverifikasi `netstat` dan `ss` |

### Tahap 4: Universal Forwarder

| Status | Item | Catatan |
|---|---|---|
| ✅ | Rename VM dan hostname Linux jadi `endpoint-linux-waz-plunk` | |
| ✅ | Install UF di endpoint Linux, running dan enabled (systemd) | Screenshot `03-forwarder-ubuntu` 00 sampai 04 |
| 🔄 | `add forward-server` ke `192.168.56.108:9997` | Command sudah diberikan, hasil belum dilaporkan |
| ⬜ | `inputs.conf` (`auth.log`, `syslog`) | |
| ⬜ | Install UF di endpoint Windows (`endpoint-windows-waz-plunk`) | Rename VM sudah direncanakan |
| ⬜ | Konfigurasi forwarder Windows (Event Log) | |
| ⬜ | Sysmon (opsional) | |

### Tahap 5 sampai 9: Verifikasi, Serangan, Analisis, Deteksi, Writeup

| Status | Item | Catatan |
|---|---|---|
| ⬜ | Verifikasi data masuk di Splunk (`index=main`) | |
| ⬜ | Verifikasi Kali bisa menjangkau target | |
| ⬜ | Simulasi serangan (brute force SSH, port scan, brute force Windows) | |
| ⬜ | Analisis dengan SPL | |
| ⬜ | Dashboard | |
| ⬜ | Alert / correlation search | Perlu cek lisensi setelah trial habis |
| ⬜ | Runbook dan catatan kasus | |
| ⬜ | Writeup `report.md` dan README | |
| ⬜ | Rencana lanjutan (komparasi Wazuh vs Splunk, notifikasi, SOAR, user non-root) | |

### Pengecekan Setelah Jeda

| Status | Item | Catatan |
|---|---|---|
| ⬜ | Nyalakan VM, cek IP (`ip a`) | DHCP bisa berubah |
| ⬜ | Cek `splunk status` dan boot-start | |
| ⬜ | Cek status lisensi trial (Settings > Licensing) | Trial 60 hari kemungkinan sudah lewat |

---

### 📊 Summary

| Metric | Jumlah |
|---|---|
| ✅ Selesai | 20 item |
| 🔄 Sedang berjalan | 1 item |
| ⬜ Belum dimulai | 16 item |
| **Total** | **37 item** |
| **Progress** | **±54%** |

**Terakhir selesai di:** Verifikasi port 9997 listening di splunk-server (screenshot `01-splunk-server/16-verify-port-9997-listening`), setelah UF di endpoint Linux terinstal dan berjalan.
**Selanjutnya:** Nyalakan VM, cek IP dan `splunk status`, lalu jalankan `add forward-server 192.168.56.108:9997` di `endpoint-linux-waz-plunk`.
