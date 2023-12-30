---
lab:
  title: 'Lab 03b: Mengelola sumber daya Azure dengan Menggunakan Templat ARM'
  module: Administer Azure Resources
---

# Lab 03b - Kelola sumber daya Azure dengan Menggunakan Template ARM
# Panduan lab siswa

## Skenario laboratorium
Tim Anda telah menjelajahi kemampuan administratif Azure dasar seperti menyediakan sumber daya dan mengaturnya berdasarkan grup sumber daya. Selanjutnya, tim Anda ingin melihat cara untuk mengotomatiskan dan menyederhanakan penyebaran. Organisasi sering terlihat otomatisasi untuk mengurangi overhead administratif, mengurangi kesalahan manusia atau meningkatkan konsistensi, dan sebagai cara untuk memungkinkan administrator mengerjakan tugas yang lebih kompleks atau kreatif.

**Catatan:** Tersedia **[simulasi lab interaktif](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%205)** yang memungkinkan Anda mengklik lab ini sesuai keinginan Anda. Anda mungkin menemukan sedikit perbedaan antara simulasi interaktif dan lab yang dihosting, tetapi konsep dan ide utama yang ditunjukkan sama. 

## Tujuan

Di lab ini Anda akan:

+ Tugas 1: Membuat templat ARM untuk penyebaran disk terkelola Azure
+ Tugas 2: Edit templat ARM lalu buat disk terkelola Azure dengan menggunakan templat
+ Tugas 3: Tinjau penyebaran berbasis templat ARM dari disk terkelola

## Perkiraan waktu: 30 menit

## Diagram arsitektur

![gambar](./media/az104-lab03b-architecture-diagram.png)

### Petunjuk

## Latihan 1

## Tugas 1: Membuat templat ARM untuk penyebaran disk terkelola Azure
Dalam tugas ini, Anda akan menggunakan portal Azure untuk menghasilkan templat ARM. Anda kemudian dapat mengunduh templat untuk digunakan dalam penyebaran mendatang. Organisasi yang berencana untuk menyebarkan ratusan atau ribuan disk dapat memanfaatkan satu atau beberapa templat untuk membantu mengotomatiskan penyebaran. Dalam tugas ini, kita menggunakan portal Azure. Anda juga dapat mendekompilasi templat ke Azure Bicep.

1. Masuk ke [**portal Azure**](http://portal.azure.com).

1. Di portal Azure, cari dan pilih `Disks`.

1. Pada halaman Disk, pilih **Buat**.

1. Pada halaman Buat disk terkelola, gunakan informasi berikut untuk membuat disk.
    
    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | Langganan yang Anda gunakan | 
    | Grup Sumber Daya | `az104-rg1` (Jika perlu, pilih **Buat baru**.)
    | Nama disk | `az104-disk1` | 
    | Wilayah | **US Timur** |
    | Zona ketersediaan | **Tidak ada redundansi infrastruktur yang diperlukan** | 
    | Jenis sumber | **Tidak** |
    | Ukuran | **32 Gb** | 
    | Performa | **HDD Standar** |

    ![gambar](./media/az104-lab03b-createdisk.png)

1. Klik **Tinjau + Buat** *sekali*. Jangan **** benar-benar menyebarkan sumber daya.

1. Pada tab Tinjau + buat, klik **Unduh templat untuk otomatisasi**.

1. Tinjau informasi yang ditampilkan pada templat, termasuk parameter dan sumber daya yang dibuat.

1. Klik **Unduh** dan simpan templat ke komputer Anda.

1. Ekstrak konten file yang diunduh ke **folder Unduhan** di komputer Anda.

    
1. Tutup semua jendela **File Explorer**.

1. Di portal Azure, batalkan penyebaran disk terkelola.

## Tugas 2: Edit templat ARM lalu buat disk terkelola Azure dengan menggunakan templat
Dalam tugas ini, Anda akan menggunakan templat ARM yang Anda buat untuk menyebarkan disk terkelola baru. Tugas ini menguraikan proses umum memiliki penyebaran berbasis templat sehingga Anda dapat dengan cepat dan mudah mengulangi penyebaran. Jika Anda perlu mengubah parameter atau dua, Anda dapat dengan mudah memodifikasi templat di masa mendatang.

1. Di portal Azure, cari dan pilih `Deploy a custom template`.

1. Pada panel **Penyebaran Kustom**, klik **Buat template Anda sendiri di penyunting**.

1. Pada panel **Edit template**, klik **Muat file** dan unggah file **template.json** yang Anda unduh di tugas sebelumnya.

1. Di dalam panel editor, hapus baris berikut:

   ```json
    "sourceResourceId": {
            "type": "string"
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

1. Kembali ke bilah **Penyebaran kustom**, klik **Edit parameter**. 

1. Pada panel **Edit parameter**, klik **Muat file** dan unggah file **parameters.json** yang Anda unduh di tugas sebelumnya, dan **Simpan** perubahan.

1. Kembali ke panel **Penerapan khusus**, tentukan setelan berikut:

    | Pengaturan | Nilai |
    | --- |--- |
    | Langganan | *nama langganan Azure yang Anda gunakan di lab ini* |
    | Grup Sumber Daya | `az104-rg1` (Jika perlu, pilih **Buat baru**)|
    | Wilayah | nama wilayah Azure apa pun yang tersedia dalam langganan yang Anda gunakan di lab ini, biasanya **US** Timur. |
    | Nama Disk | `az104-disk1` |
    | Lokasi | nilai parameter lokasi yang Anda catat di tugas sebelumnya |
    | SKU | **Standar_LRS** |
    | Ukuran Disk Gb | **32** |
    | Buat Opsi | **kosong** |
    | Jenis Set Enkripsi Disk | **EncryptionAtRestWithPlatformKey** |
    | Mode Autentikasi Akses Data | Tidak |
    | Azure Policy Akses Jaringan | **IzinkanSemua** |
    | Akses Jaringan Publik | Nonaktif |

    ![gambar](./media/az104-lab03b-customdeploy.png)

1. Pilih **Ulas + buat**, lalu pilih **Buat**.

1. Verifikasi bahwa penerapan berhasil diselesaikan.

## Tugas 3: Tinjau penyebaran berbasis templat ARM dari disk terkelola
Dalam tugas ini, Anda memverifikasi bahwa penyebaran telah berhasil diselesaikan. Semua penyebaran sebelumnya didokumentasikan dalam grup sumber daya tempat penyebaran ditargetkan. Tinjauan ini akan menampilkan detail sekeliling waktu dan lamanya penyebaran, yang dapat membantu saat pemecahan masalah. Sering kali merupakan praktik yang baik untuk meninjau beberapa penyebaran berbasis templat pertama untuk memastikan keberhasilan sebelum menggunakan templat untuk operasi skala besar.

1. Di portal Microsoft Azure, cari dan pilih **Grup sumber daya**. 

1. Dalam daftar grup sumber daya, klik **az104-rg1**.

1. Pada bilah **grup sumber daya az104-rg1**, di bagian **Pengaturan**, klik **Penyebaran**.

1. Dari bilah **az104-rg1 - Deployments** , klik entri pertama dalam daftar penyebaran dan tinjau konten **bilah Input** dan **Templat** .


## Tinjau

Selamat! Di lab ini, Anda telah menggunakan Portal Microsoft Azure untuk membuat templat ARM untuk disk terkelola lalu menggunakan templat untuk menyebarkan disk baru.