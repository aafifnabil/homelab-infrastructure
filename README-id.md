---
title: "afn-lab's Homelab Infra"
---

# Merancang Private Cloud Hibrida Tingkat Lanjut untuk Ekosistem Pendidikan

*Baca dalam bahasa lain: [English](README.md), [Bahasa Indonesia](README-id.md).*

## 1. Ringkasan Eksekutif
Merancang, mendeploy, dan mengelola infrastruktur *Private Cloud* yang sangat teroptimasi menggunakan Proxmox VE. Proyek ini berfungsi sebagai tulang punggung digital institusi pendidikan, direkayasa untuk memberikan kapabilitas hibrida (Jaringan Online & Lokal). Sistem ini dengan mulus menangani **400+ pengguna serentak** selama ujian CBT massal, sekaligus menyediakan manajemen jaringan tingkat lanjut, penyaringan konten, dan lingkungan pengembangan terintegrasi untuk aplikasi kustom.

## 2. Arsitektur Sistem & Layanan Inti

![Dashboard Proxmox VE](proxmox-dashboard.png)
*Dashboard Proxmox VE: Manajemen terpusat untuk kontainer LXC, VM QEMU, dan lingkungan Docker.*

Infrastruktur ini disegmentasi secara ketat untuk memastikan performa, keamanan, dan efisiensi sumber daya:

* **Routing Inti Tingkat Lanjut (MikroTik CHR):** Otak jaringan yang dikonfigurasi dengan fitur kelas *enterprise* termasuk **Load Balancing**, **Multi-WAN Failover**, dan **VRRP** (Virtual Router Redundancy Protocol) untuk memastikan tidak ada *downtime* pada *gateway* jaringan.
* **Mesin CBT Hibrida (Online & Offline):** Sistem ujian adaptif yang berjalan di atas LXC ringan. Sistem ini menyediakan akses global melalui Zero Trust Tunnel `cloudflared`, sekaligus mendukung ujian jaringan lokal yang terisolasi menggunakan konfigurasi kustom **`nginx-ncsibypass`** untuk menjaga koneksi klien Windows tetap stabil tanpa akses internet.
* **Manajemen DNS & Penyaringan Konten:** Penerapan `adguard` terpusat yang bertindak sebagai *DNS sinkhole* untuk memblokir iklan, *malware*, dan menyaring *website* yang tidak pantas, memastikan lingkungan digital yang aman bagi siswa.
* **Ruang Kerja Digital Mandiri:** Implementasi `nextcloud` yang berfungsi sebagai *file server* privat dan aman untuk administrasi dan kolaborasi data sekolah.
* **Pengembangan Aplikasi & Kontainerisasi:** Lingkungan **Docker** khusus yang digunakan untuk mengembangkan, menguji, dan meng- *host* **ANSimpleLibrary**—aplikasi manajemen perpustakaan buatan sendiri, membuktikan kapabilitas *Dev/Ops* secara menyeluruh.

## 3. Strategi Resiliensi & Pemeliharaan
Untuk memaksimalkan *uptime* dan integritas data pada *hypervisor* tunggal, saya menerapkan kerangka resiliensi yang ketat:
* **Pemulihan Bencana Otomatis:** *Backup* data terjadwal ke penyimpanan eksternal untuk menjamin *Recovery Time Objective* (RTO) yang cepat.
* **Optimasi Lalu Lintas:** Penyesuaian antrean *bandwidth* (Queue) dan aturan *routing* di MikroTik untuk mencegah kemacetan saat *login* ujian massal.
* **Peringatan Proaktif:** Integrasi `uptimekuma` untuk observabilitas *real-time*, mengirimkan notifikasi anomali instan langsung ke Telegram.

## 4. Sorotan Engineering
* **Kompetensi Full-Stack:** Berhasil menjembatani operasi Infrastruktur (Proxmox, MikroTik, Jaringan) dengan *deployment* Perangkat Lunak (meng- *containerize* aplikasi kustom menggunakan Docker).
* **Efisiensi Sumber Daya Maksimal:** Penggunaan intensif Linux Containers (LXC) dan Docker dibandingkan VM tradisional, memangkas beban CPU dan RAM secara drastis sambil memaksimalkan *throughput*.
