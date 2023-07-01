---
lab:
  title: 'Lab 03b: Mengelola sumber daya Azure dengan Menggunakan Templat ARM'
  module: Administer Azure Resources
---

# Lab 03b - Mengelola sumber daya Azure dengan Menggunakan Template ARM
# Panduan lab siswa

## Skenario lab
Sekarang setelah Anda menjelajahi kemampuan administrasi Azure dasar yang terkait dengan penyediaan sumber daya dan mengaturnya berdasarkan grup sumber daya menggunakan portal Azure, Anda perlu melakukan tugas yang setara dengan menggunakan templat Azure Resource Manager.

**Catatan:** Tersedia **[simulasi lab interaktif](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%205)** yang memungkinkan Anda mengklik lab ini sesuai keinginan Anda. Anda mungkin menemukan sedikit perbedaan antara simulasi interaktif dan lab yang dihosting, tetapi konsep dan ide utama yang ditunjukkan sama. 

## Tujuan

Di lab ini Anda akan:

+ Tugas 1: Meninjau template ARM untuk penyebaran disk yang dikelola Azure
+ Tugas 2: Membuat disk terkelola Azure dengan menggunakan template ARM
+ Tugas 3: Meninjau penyebaran berbasis template ARM dari disk yang dikelola

## Perkiraan waktu: 20 menit

## Diagram arsitektur

![gambar](../media/lab03b.png)

### Petunjuk

## Latihan 1

## Tugas 1: Meninjau template ARM untuk penyebaran disk yang dikelola Azure

1. Masuk ke [**portal Microsoft Azure**](http://portal.azure.com).

1. Di portal Microsoft Azure, cari dan pilih **Grup sumber daya**. 

1. Dalam daftar grup sumber daya, klik **az104-03a-rg1**.

1. Pada bilah grup sumber daya **az104-03a-rg1**, di bagian **Pengaturan**, klik **Penyebaran**.

1. Pada bilah **az104-03a-rg1 - Penyebaran**, klik entri pertama dalam daftar penyebaran.

1. Pada bilah **Microsoft.ManagedDisk-* XXXXXXXXX* \| panel**, klik **Templat**.

    >**Catatan**: Tinjauan konten template dan perhatikan bahwa Anda memiliki opsi untuk **Mengunduh** ke komputer lokal, **Tambahkan ke pustaka**, atau **Sebarkan** lagi.

1. Klik **Unduh** dan simpan berkas terkompresi yang berisi template dan file parameter ke map **Unduhan** di komputer lab Anda.

1. Pada panel **Microsoft.ManagedDisk-* XXXXXXXXX* \| Template**, klik **Input**.

1. Perhatikan nilai parameter **lokasi**. Anda akan membutuhkannya di tugas berikutnya.

1. Ekstrak konten file yang diunduh ke dalam folder **Unduhan** di komputer lab Anda.

    >**Catatan**: File ini juga tersedia sebagai **\\Allfiles\\Labs\\03\\az104-03b-md-template.json** dan **\\Allfiles\\Labs\\ 03\\az104-03b-md-parameters.json**
    
1. Tutup semua jendela **File Explorer**.

## Tugas 2: Membuat disk terkelola Azure dengan menggunakan template ARM

1. Di portal Azure, cari **Menyebarkan templat kustom**.

1. Pada panel **Penyebaran Kustom**, klik **Buat template Anda sendiri di editor**.

1. Pada panel **Edit template**, klik **Muat file** dan unggah file **template.json** yang Anda unduh di tugas sebelumnya.

1. Di dalam panel editor, hapus baris berikut:

   ```json
   "sourceResourceId": {
       "type": "String"
   },
   ```

   ```json
   "hyperVGeneration": {
       "defaultValue": "V1",
       "type": "String"
   },      
   ```

    >**Catatan**: Parameter ini dihapus karena tidak berlaku untuk penyebaran saat ini. Secara khusus, parameter sourceResourceId, sourceUri, osType, dan hyperVGeneration berlaku untuk membuat disk Azure dari file VHD yang ada.

1. **Simpan** perubahan.

1. Kembali ke bilah **Penyebaran Kustom**, klik **Edit parameter**. 

1. Pada panel **Edit parameter**, klik **Muat file** dan unggah file **parameters.json** yang Anda unduh di tugas sebelumnya, dan **Simpan** perubahan.

1. Kembali ke panel **Penerapan khusus**, tentukan setelan berikut:

    | Pengaturan | Nilai |
    | --- |--- |
    | Langganan | *nama langganan Azure yang Anda gunakan di lab ini* |
    | Grup Sumber Daya | nama grup sumber daya **baru** **az104-03b-rg1** |
    | Wilayah | nama wilayah Azure yang tersedia dalam langganan yang Anda gunakan di lab ini |
    | Nama Disk | **az104-03b-disk1** |
    | Lokasi | nilai parameter lokasi yang Anda catat di tugas sebelumnya |
    | Sku | **Standar_LRS** |
    | Ukuran Disk Gb | **32** |
    | Buat Opsi | **empty** |
    | Jenis Set Enkripsi Disk | **EncryptionAtRestWithPlatformKey** |
    | Mode Autentikasi Akses Data | Tidak ada |
    | Azure Policy Akses Jaringan | **IzinkanSemua** |
    | Akses Jaringan Publik | Nonaktif |

1. Pilih **Ulas + buat**, lalu pilih **Buat**.

1. Verifikasi bahwa penerapan berhasil diselesaikan.

## Tugas 3: Meninjau penyebaran berbasis template ARM dari disk yang dikelola

1. Di portal Microsoft Azure, cari dan pilih **Grup sumber daya**. 

1. Dalam daftar grup sumber daya, klik **az104-03b-rg1**.

1. Pada bilah grup sumber daya **az104-03b-rg1**, di bagian **Pengaturan**, klik **Penyebaran**.

1. Dari panel **az104-03b-rg1 - Penyebaran**, klik entri pertama dalam daftar penyebaran dan tinjauan konten bilah **Input** dan **Template**.

## Membersihkan sumber daya

   >**Catatan**: Jangan hapus sumber daya yang Anda terapkan di lab ini. Anda akan merujuknya di lab berikutnya dari modul ini.

## Tinjau

Di lab ini, Anda telah:

- Meninjau templat ARM untuk penyebaran disk terkelola Azure
- Membuat disk terkelola Azure dengan menggunakan template ARM
- Meninjau penyebaran berbasis template ARM dari disk yang dikelola
