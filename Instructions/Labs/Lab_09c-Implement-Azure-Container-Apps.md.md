---
lab:
  title: 'Lab 09c: Menerapkan Azure Container Apps'
  module: Administer PaaS Compute Options
---

# Lab 09c: Menerapkan Azure Container Apps
# Panduan lab siswa

## Skenario lab
Azure Container Apps memungkinkan Anda menjalankan layanan mikro dan aplikasi dalam kontainer pada platform tanpa server. Dengan Container Apps, Anda bisa mendapatkan manfaat menjalankan kontainer tanpa repot mengonfigurasi infrastruktur cloud dan orkestrator kontainer yang kompleks secara manual.

## Tujuan

Di lab ini, kita akan:
- Tugas 1: Membuat aplikasi dan lingkungan kontainer
- Tugas 2: Menyebarkan aplikasi kontainer
- Tugas 3: Menguji dan memverifikasi penyebaran aplikasi kontainer

Mulailah dengan masuk ke [portal Microsoft Azure](https://portal.azure.com).

## Perkiraan waktu: 20 menit

## Tugas 1: Membuat aplikasi dan lingkungan kontainer

Untuk membuat aplikasi kontainer Anda, mulai di halaman beranda portal Microsoft Azure.

1. Cari `Container Apps` di bilah pencarian atas.
1. Pilih **Aplikasi Kontainer** di hasil pencarian.
1. Pilih tombol **Buat**.

### Tab Dasar

Pada tab *Dasar*, lakukan hal berikut.

1. Masukkan nilai berikut ini di bagian *Detail Proyek*.

    | Pengaturan | Tindakan |
    |---|---|
    | Langganan | Pilih langganan Azure Anda. |
    | Grup sumber daya | Pilih **Buat baru** dan masukkan `az104-09c-rg1`. |
    | Nama aplikasi kontainer |  Masukkan `my-container-app`. |

#### Membuat lingkungan

Selanjutnya, buat lingkungan untuk aplikasi kontainer Anda.

1. Pilih subnet yang sesuai.

    | Pengaturan | Nilai |
    |--|--|
    | Wilayah | **Pilihanmu**. |

1. Di bidang *Buat lingkungan Container Apps*, pilih tautan **Buat baru**.
1. Di halaman *Buat Lingkungan Container Apps* pada tab *Dasar-dasar*, masukkan nilai berikut:

    | Pengaturan | Nilai |
    |--|--|
    | Nama lingkungan | Masukkan `my-environment`. |
    | Redundansi zona | Pilih **Dinonaktifkan** |

1. Pilih tab **Pemantauan** untuk membuat ruang kerja Analitik Log.
1. Pilih tautan **Buat baru** di bidang *ruang kerja Analitik Log* dan masukkan nilai berikut.

    | Pengaturan | Nilai |
    |--|--|
    | Nama | Masukkan `my-container-apps-logs` |
  
    Bidang *Lokasi* telah diisi sebelumnya dengan wilayah Anda untuk Anda.

1. Pilih **OK** lalu **Buat**. 

1. Klik **Berikutnya: Kontainer**.

1. Centang kotak di samping **Gunakan gambar mulai cepat**.

1. Pilih tombol **Tinjau + buat** di bagian bawah halaman. Langkah ini mungkin memakan waktu beberapa menit. 

    Pengaturan di Aplikasi Kontainer diverifikasi. Jika tidak ada kesalahan yang ditemukan, tombol *Buat* diaktifkan.  

    Jika ada kesalahan, tab apa pun yang berisi kesalahan ditandai dengan titik merah.  Buka tab yang sesuai. Bidang yang berisi kesalahan akan disorot dengan warna merah.  Setelah semua kesalahan diperbaiki, pilih **Tinjau dan buat** lagi.

1. Pilih **Buat**.

    Halaman dengan pesan *Penyebaran sedang berlangsung* ditampilkan.  Setelah penyebaran berhasil diselesaikan, Anda akan melihat pesan *Penyebaran Anda selesai*.
   
## Tugas 2: Menguji dan memverifikasi penyebaran aplikasi kontainer

1. Pilih **Buka sumber daya** untuk melihat aplikasi kontainer baru Anda.

1. Pilih tautan di samping *URL Aplikasi* untuk melihat aplikasi Anda.

1. Verifikasi bahwa Anda menerima pesan *aplikasi Azure Container Apps Anda adalah live**.

## Membersihkan sumber daya

Jika Anda tidak akan terus menggunakan aplikasi ini, Anda dapat menghapus instans Aplikasi Kontainer Azure dan semua layanan terkait dengan menghapus grup sumber daya.

1. Pilih grup sumber daya **my-container-apps** dari bagian *Gambaran Umum*.
1. Pilih tombol **Hapus grup sumber daya** di bagian atas *Gambaran Umum* grup sumber daya.
1. Masukkan nama grup sumber daya dan konfirmasikan bahwa Anda ingin menghapus aplikasi. 
1. Pilih **Hapus**.
1. Proses untuk menghapus grup sumber daya mungkin memerlukan waktu beberapa menit untuk diselesaikan.
