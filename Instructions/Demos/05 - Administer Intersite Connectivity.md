---
demo:
  title: 'Demonstrasi 05: Mengelola Konektivitas AntarSitus'
  module: Administer Intersite Connectivity
---

# 05 - Mengelola Konektivitas AntarSitus

## Mengonfigurasi Peering VNet

**Catatan:** Untuk demonstrasi ini, Anda akan memerlukan dua jaringan virtual.

**Referensi**: [Menyambungkan jaringan virtual dengan peering VNet - tutorial](https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal)

**Konfigurasi peering VNet di jaringan virtual pertama**

1. Di  **portal Azure**, pilih jaringan virtual pertama. Tinjau nilai peering. 

1. Di bawah **Pengaturan**, pilih **Peering** dan **+ Tambahkan** peering baru.

1. Konfigurasikan peering jaringan virtual kedua. Gunakan ikon informasi untuk meninjau pengaturan yang berbeda. 

1. Saat peering selesai, tinjau **status Peering**. 

**Konfirmasi peering VNet di jaringan virtual kedua**

1. Di  **portal Azure**, pilih jaringan virtual kedua

1. Di bawah **Pengaturan**, pilih **Peering**.

1. Perhatikan bahwa peering telah dibuat secara otomatis.  Perhatikan bahwa **** Status   **Peering**Tersambung.

1. Di lab, siswa akan membuat peering dan menguji koneksi antara komputer virtual. 

## Mengonfigurasi Perutean Jaringan dan Titik Akhir

Dalam demonstrasi ini, kita akan mempelajari cara membuat tabel rute, menentukan rute kustom, dan mengaitkan rute dengan subnet.

**Catatan:** Demonstrasi ini memerlukan jaringan virtual dengan setidaknya satu subnet.

**Referensi**: [Merutekan lalu lintas jaringan - tutorial - portal Azure](https://learn.microsoft.com/azure/virtual-network/tutorial-create-route-table-portal#create-a-route-table)

**Membuat tabel perutean**

1. Karena Anda memiliki waktu meninjau diagram tutorial. Jelaskan mengapa Anda perlu membuat rute yang ditentukan pengguna. 

1. Akses portal Microsoft Azure.

1. Cari dan pilih **Tabel rute**. Diskusikan kapan **rute gateway penyebaran** harus digunakan. 

1. Buat tabel perutean, jelaskan pengaturan yang tidak biasa. 

1. Tunggu hingga tabel perutean baru diterapkan.

**Menambahkan rute**

1.  Pilih tabel perutean baru Anda, lalu pilih **Rute**.

1.  Buat **rute** baru. Diskusikan berbagai **jenis hop** yang tersedia. 

1.  Buat rute baru dan tunggu hingga sumber daya disebarkan.
 
**Mengaitkan tabel rute ke subnet**

1.  Navigasi ke subnet yang ingin Anda kaitkan dengan tabel perutean.

1.  Pilih **Tabel rute** dan pilih tabel perutean baru Anda. 

1.  **Menyimpan** perubahan Anda.

