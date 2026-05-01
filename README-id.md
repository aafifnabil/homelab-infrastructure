# Merancang Private Cloud yang Teroptimasi untuk Ekosistem Sekolah/Kampus

*Baca dalam bahasa lain: [English](README.md), [Bahasa Indonesia](README-id.md).*

## 1. Ringkasan Eksekutif
Merancang dan mengelola infrastruktur *Private Cloud* yang efisien menggunakan Proxmox VE sebagai tulang punggung layanan digital institusi pendidikan. Sistem ini dioptimalkan untuk menangani beban kerja intensif, terbukti sukses melayani **400+ pengguna serentak** pada platform CBT tanpa kendala performa, dengan fokus pada stabilitas layanan dan efisiensi sumber daya *hardware*.

## 2. Arsitektur Sistem & Manajemen Sumber Daya

![Dashboard Proxmox VE](proxmox-dashboard.png)
*Dashboard Proxmox VE: Manajemen terpusat untuk seluruh kontainer LXC dan VM QEMU.*

Infrastruktur ini didesain dengan pendekatan isolasi layanan untuk menjaga stabilitas dan memaksimalkan penggunaan server fisik:

* **Routing Inti:** MikroTik CHR v7 (VM) sebagai *gateway* jaringan utama, menangani distribusi VLAN dan manajemen *bandwidth* klien.
* **Keamanan & Akses:** `cloudflared` untuk jalur akses eksternal yang aman dan `adguard` sebagai pelindung DNS tingkat lanjut.
* **Layanan Produksi:** Klaster aplikasi ujian `moodle` & `garuda-cbt` yang berjalan di atas LXC untuk mencapai latensi rendah.
* **Ruang Kerja Digital:** `nextcloud` & `onlyoffice` sebagai solusi kolaborasi dokumen internal mandiri.
* **Observabilitas:** `uptimekuma` untuk pemantauan kesehatan seluruh layanan secara *real-time*.

## 3. Strategi Resiliensi & Pemeliharaan
Meskipun berjalan pada lingkungan *single-node*, keamanan data dan keberlangsungan layanan tetap menjadi prioritas utama melalui:
* **Pemulihan Bencana Otomatis:** Implementasi jadwal *backup* rutin ke penyimpanan eksternal untuk menjamin keamanan data jika terjadi kegagalan sistem.
* **Optimasi Lalu Lintas:** Penggunaan `nginx-ncsibypass` kustom untuk menjaga stabilitas koneksi perangkat klien di dalam jaringan lokal.
* **Peringatan Proaktif:** Integrasi pemantauan dengan notifikasi Telegram untuk respon cepat terhadap anomali layanan.

## 4. Sorotan Engineering
* **Efisiensi Sumber Daya Maksimal:** Penggunaan intensif LXC (Linux Containers) untuk menjalankan aplikasi berat, menghasilkan penggunaan CPU dan RAM yang sangat rendah dibandingkan VM konvensional.
* **Presisi Jaringan:** Implementasi *Router-on-a-Stick* di dalam hypervisor untuk pemisahan lalu lintas yang rapi dan aman antar segmen jaringan.
