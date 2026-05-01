---
title: "afn-lab's Homelab Infra"
---

# Merancang Arsitektur Private Cloud & Jaringan Tingkat Lanjut untuk Ekosistem Kampus

*Baca dalam bahasa lain: [English](README.md), [Bahasa Indonesia](README-id.md).*

## 1. Ringkasan Eksekutif
Merancang, mendeploy, dan mengelola infrastruktur *Private Cloud* dan Jaringan Tingkat Lanjut secara komprehensif menggunakan Proxmox VE. Dibangun untuk institusi pendidikan, sistem ini secara mulus menangani **400+ pengguna serentak** pada platform CBT. Arsitektur ini menggabungkan rekayasa jaringan yang tangguh, lingkungan akses *hybrid*, dan ruang *DevOps* khusus untuk memberikan performa tinggi, keamanan, dan kapabilitas *zero-downtime*.

## 2. Arsitektur Inti & Layanan

![Dashboard Proxmox VE](proxmox-dashboard.png)
*Dashboard Proxmox VE: Manajemen terpusat untuk kontainer LXC, QEMU VM, dan lingkungan Docker.*

Infrastruktur ini disegregasi menjadi beberapa lapisan operasional untuk memastikan keamanan absolut dan efisiensi sumber daya:

* **Routing Inti & Gateway Lanjutan (MikroTik CHR v7):**
  * Dikonfigurasi dengan **Load Balancing** dan **Dual-WAN Failover** untuk menjamin konektivitas tanpa gangguan.
  * Mengimplementasikan **VRRP (Virtual Router Redundancy Protocol)** untuk redundansi *gateway* pada lapisan jaringan.
* **Keamanan Perusahaan & Filter DNS:**
  * Menggunakan `adguard` sebagai *DNS sinkhole* untuk pemblokiran iklan di seluruh jaringan, filter situs web berbahaya, dan manajemen akses konten bagi siswa.
* **Ekosistem E-Learning & CBT Hybrid (`moodle` & `garuda-cbt`):**
  * **Mode Online:** Akses jarak jauh yang aman dialirkan melalui *Zero Trust Tunnels* (`cloudflared`).
  * **Mode Ujian Offline Ketat:** Penguncian jaringan lokal untuk mencegah kecurangan. Menggunakan rekayasa **`nginx-ncsibypass`** kustom untuk "mengelabui" perangkat Windows agar mengira ada koneksi internet (mencegah *error captive portal*), padahal akses WAN diputus secara fisik. Siswa dapat ujian tanpa bisa *browsing* internet.
* **Pusat Kerja Digital Mandiri:**
  * Menyebarkan `nextcloud` sebagai *file server* internal dan pusat kolaborasi dokumen, menjamin kedaulatan data kampus sepenuhnya.
* **DevOps & Sandbox Pengembangan Aplikasi:**
  * Menyediakan lingkungan **Docker** khusus untuk pengujian dan penerapan aplikasi buatan sendiri, secara spesifik menjalankan sistem manajemen perpustakaan mandiri, **ANSimpleLibrary**.

## 3. Strategi Resiliensi & Optimasi
* **Pemulihan Bencana Otomatis:** Penjadwalan *backup snapshot* ke penyimpanan eksternal untuk memastikan waktu pemulihan (RTO) yang sangat cepat.
* **Maksimalisasi Sumber Daya:** Penggunaan strategis LXC untuk layanan produksi dan Docker untuk pengembangan, secara drastis menekan beban CPU/RAM dibandingkan penggunaan VM tradisional.
* **Observabilitas:** Pemantauan kesehatan sistem terpusat secara *real-time* dan peringatan dini proaktif melalui `uptimekuma` yang terintegrasi dengan bot Telegram.
