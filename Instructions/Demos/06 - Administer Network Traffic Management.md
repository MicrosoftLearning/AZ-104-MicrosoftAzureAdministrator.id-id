---
demo:
  title: 'Demonstrasi 06: Mengelola Manajemen Lalu Lintas Jaringan'
  module: Administer Network Traffic Management
---


# 06 - Mengelola Manajemen Lalu Lintas Jaringan

## Mengonfigurasi Perutean Jaringan dan Titik Akhir

Dalam demonstrasi ini, kita akan mempelajari cara membuat tabel rute, menentukan rute kustom, dan mengaitkan rute dengan subnet.

**Catatan:** Demonstrasi ini memerlukan jaringan virtual dengan setidaknya satu subnet.

**Referensi**: [Merutekan lalu lintas jaringan - tutorial - portal Azure](https://learn.microsoft.com/azure/virtual-network/tutorial-create-route-table-portal#create-a-route-table)

**Membuat tabel perutean**

1. Karena Anda memiliki waktu untuk meninjau diagram tutorial. Jelaskan mengapa Anda perlu membuat rute yang ditentukan pengguna. 

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

1.  Pilih **Tabel rute* dan pilih tabel perutean baru Anda. 

1.  **Menyimpan** perubahan Anda.

