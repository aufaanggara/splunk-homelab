# Splunk Home Lab

Lab SIEM berbasis Splunk Enterprise di VirtualBox untuk belajar pengumpulan log, analisis, dan deteksi serangan.

---

## Deskripsi

Project ini membangun lab SIEM dari nol dengan Splunk Enterprise sebagai indexer dan search head, serta Universal Forwarder di endpoint Linux dan Windows. Tujuannya mempelajari alur log collection, analisis dengan SPL, pembuatan dashboard dan alert, lalu menguji deteksi dengan simulasi serangan dari Kali Linux. Endpoint yang sama dengan lab Wazuh sebelumnya dipakai ulang agar log yang sama bisa dibandingkan di dua SIEM.

## Tech Stack & Tools

- Splunk Enterprise 10.4.2 — indexer dan search head (trial 60 hari)
- Splunk Universal Forwarder 10.4.2 — agen ringan pengirim log dari endpoint
- VirtualBox — virtualisasi (NAT + Host-only `192.168.56.0/24`)
- Ubuntu Server 24.04 LTS — OS Splunk server dan endpoint Linux
- Windows — endpoint Windows
- Kali Linux 2025.4 — mesin attacker untuk simulasi serangan
- Wazuh agent — dari lab sebelumnya, berjalan berdampingan di endpoint
- OpenSSH — akses remote dari host Windows

## Topologi

| VM | IP Host-only | Peran |
|---|---|---|
| `splunk-server` | `192.168.56.108` | Splunk Enterprise (web 8000, receiving 9997) |
| `endpoint-linux-waz-plunk` | `192.168.56.106` | Endpoint Linux (Wazuh agent + Splunk UF) |
| `endpoint-windows-waz-plunk` | `192.168.56.109` | Endpoint Windows (Wazuh agent + Splunk UF, planned) |
| Kali | `192.168.56.102` | Attacker |

## Struktur Folder

```
splunk-homelab/
├── infra/
│   ├── network-diagram.md
│   └── vm-specs.md
├── docs/
│   ├── setup/
│   │   ├── 00-network/
│   │   ├── 01-splunk-server/
│   │   ├── 02-forwarder-windows/
│   │   └── 03-forwarder-ubuntu/
│   ├── attack-simulation/
│   └── detection-dashboard/
├── configs/
│   ├── splunk-server/
│   ├── forwarders/
│   └── kali/
└── writeup/
    └── report.md
```

## Progress

| Fase | Status |
|---|---|
| Persiapan & dokumentasi | ✅ Selesai |
| Tahap 1: Jaringan | ✅ Selesai |
| Tahap 2: VM Splunk server | ✅ Selesai |
| Tahap 3: Splunk Enterprise | ✅ Selesai |
| Tahap 4: Universal Forwarder | 🔄 Berjalan (Linux terinstal, konfigurasi kirim log dan Windows belum) |
| Verifikasi data masuk | ⬜ Belum |
| Simulasi serangan | ⬜ Belum |
| Analisis SPL | ⬜ Belum |
| Dashboard & alert | ⬜ Belum |
| Writeup | ⬜ Belum |

**Progress keseluruhan: ±54%**

## Dokumentasi

Screenshot per tahap ada di folder `docs/`. Writeup lengkap akan tersedia di folder `writeup/` (`writeup/report.md`).

## Disclaimer

Project ini dibuat untuk tujuan pembelajaran dan portofolio. Seluruh simulasi serangan dijalankan hanya di lingkungan lab terisolasi milik sendiri (VM Host-only) dan tidak ditujukan ke sistem pihak lain.
