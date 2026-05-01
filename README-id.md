# Membangun Private Cloud Ketersediaan Tinggi untuk Ekosistem School/Kampus

*Baca dalam bahasa lain: [English](README.md), [Bahasa Indonesia](README-id.md).*

## 1. Ringkasan Eksekutif
Merancang, men-deploy, dan saat ini mengelola infrastruktur *private cloud* yang tangguh menggunakan Proxmox VE. Lingkungan ini berfungsi sebagai tulang punggung digital untuk institusi pendidikan dengan lalu lintas tinggi, menangani Ujian Berbasis Komputer (CBT) tanpa hambatan untuk 400+ pengguna serentak, penyimpanan *cloud* internal, dan perutean jaringan yang aman.

## 2. Arsitektur & Teknologi

![Proxmox VE Dashboard](proxmox-dashboard.png)

Berdasarkan topologi lingkungan di atas, infrastruktur dipisahkan ke dalam kontainer LXC dan VM QEMU yang dioptimalkan untuk memaksimalkan sumber daya *bare-metal*:

* **Hypervisor:** Proxmox VE 8.x
* **Core Routing (QEMU VM):** MikroTik CHR v7 berfungsi sebagai gerbang jaringan utama, menangani VLAN dan *load balancing*.
* **Observability & Monitoring (LXC):** `uptimekuma` untuk pelacakan kesehatan layanan dan latensi jaringan secara *real-time*.
* **Zero Trust & Security (LXC):** `cloudflared` untuk *tunneling* eksternal yang aman dan `adguard` untuk penyaringan DNS dan pemblokiran iklan di seluruh jaringan.
* **Web Server & Production Apps (LXC):** * `moodle` & `garuda-cbt`: Platform ujian berkonkurensi tinggi.
    * `nextcloud` & `onlyoffice`: Suite kolaborasi dokumen *on-premise* yang di-*host* secara mandiri.
* **Network Hacks & Optimization:** `nginx-ncsibypass` direkayasa khusus untuk memanipulasi Indikator Status Konektivitas Jaringan milik Microsoft, memaksa perangkat klien untuk tetap terhubung ke jaringan luring lokal selama ujian berlangsung.

## 3. Sorotan Teknis Utama
* **Efisiensi Sumber Daya:** Memaksimalkan penggunaan LXC (*Linux Containers*) dibandingkan VM berat untuk *deployment* aplikasi, menghasilkan penggunaan CPU yang sangat rendah dan jejak RAM yang minimal.
* **Segmentasi Jaringan:** Menjaga jaringan tetap terpisah secara logis dengan menempatkan *router* inti (MikroTik CHR) di dalam *hypervisor*, menciptakan lingkungan "Router on a Stick" sejati di dalam Proxmox.
