---
lab:
  title: 09b - Menerapkan Azure Container Instances
  module: Administer Serverless Computing
---

# Lab 09b - Menerapkan Azure Container Instances
# Panduan lab siswa

## Skenario lab

Contoso ingin menemukan platform baru untuk beban kerja virtualnya. Anda mengidentifikasi sejumlah gambar kontainer yang dapat dimanfaatkan untuk mencapai tujuan ini. Karena Anda ingin meminimalkan manajemen kontainer, Anda berencana untuk mengevaluasi penggunaan Azure Container Instances untuk penyebaran gambar Docker.

**Catatan:** Tersedia **[simulasi lab interaktif](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2014)** yang memungkinkan Anda mengklik lab ini sesuai keinginan Anda. Anda mungkin menemukan sedikit perbedaan antara simulasi interaktif dan lab yang dihosting, tetapi konsep dan ide utama yang ditunjukkan sama. 

## Tujuan

Di lab ini Anda akan:

- Tugas 1: Terapkan gambar Docker dengan menggunakan Azure Container Instance
- Tugas 2: Tinjauan fungsionalitas Instans Kontainer Azure

## Perkiraan waktu: 20 menit

## Diagram arsitektur

![gambar](../media/lab09b.png)

## Petunjuk

### Latihan 1

#### Tugas 1: Terapkan gambar Docker dengan menggunakan Azure Container Instance

Dalam tugas ini, Anda akan membuat instans kontainer baru untuk aplikasi web.

1. Masuk ke [portal Microsoft Azure](https://portal.azure.com).

1. Di portal Azure, jelajahi **Instans kontainer** lalu, pada bilah **Instans kontainer**, klik **+ Buat**.

1. Pada tab **Dasar** pada bilah **Buat instans penampung**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | ---- | ---- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | nama grup sumber daya baru **az104-09b-rg1** |
    | Nama kontainer | **az104-9b-c1** |
    | Wilayah | nama wilayah tempat Anda dapat menyediakan instans kontainer Azure |
    | Sumber Gambar | **Gambar memulai cepat** |
    | Gambar | **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)** |

1. Klik **Berikutnya: Jaringan >** dan, pada tab **Jaringan** bilah **Buat instans penampung**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Label nama DNS | nama host DNS yang valid dan unik secara global |

    >**Catatan**: Kontainer Anda akan dapat dijangkau publik di dns-name-label.region.azurecontainer.io. Jika Anda menerima pesan error **Label nama DNS tidak tersedia**, tentukan nilai yang berbeda.

1. Klik **Berikutnya: Lanjutan >** , tinjauan pengaturan pada tab **Lanjutan** pada bilah **Buat instans penampung** tanpa membuat perubahan apa pun, klik **Tinjauan + Buat**, pastikan validasi lulus dan klik **Buat**.

    >**Catatan**: Tunggu hingga penyebaran selesai. Ini akan memakan waktu sekitar 3 menit.

    >**Catatan**: Sambil menunggu, Anda mungkin tertarik untuk melihat [kode di balik contoh aplikasi](https://github.com/Azure-Samples/aci-helloworld). Untuk melihatnya, jelajahi \\folder aplikasi.

#### Tugas 2: Tinjauan fungsionalitas Instans Kontainer Azure

Dalam tugas ini, Anda akan meninjau penyebaran instans kontainer.

1. Pada bilah penyebaran, klik tautan **Buka sumber daya**.

1. Pada bilah **Ringkasan** instans penampung, verifikasi bahwa **Status** dilaporkan sebagai **Berjalan**.

1. Salin nilai instans kontainer **FQDN**, buka tab browser baru, dan navigasikan ke URL yang sesuai.

1. Pastikan halaman **Selamat datang di Azure Container Instance** ditampilkan.

1. Tutup tab browser baru, kembali ke portal Azure, di bagian **Pengaturan** bilah instans kontainer, klik **Kontainer**, lalu klik **Log**.

1. Verifikasi bahwa Anda melihat entri log yang mewakili permintaan HTTP GET yang dihasilkan dengan menampilkan aplikasi di browser.

#### Membersihkan sumber daya

>**Catatan**: Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak digunakan akan memastikan bahwa Anda tidak akan melihat biaya yang tidak diharapkan.

>**Catatan**:  Jangan khawatir jika sumber daya lab tidak dapat segera dihapus. Terkadang sumber daya memiliki ketergantungan dan membutuhkan waktu lama untuk dihapus. Ini adalah tugas Administrator yang umum untuk memantau penggunaan sumber daya, jadi tinjauan sumber daya Anda secara berkala di Portal untuk melihat bagaimana pembersihannya. 

1. Di portal Microsoft Azure, buka sesi **PowerShell** dalam panel **Cloud Shell**.

    >**Catatan**: penyimpanan Cloud Shell harus dibuat agar perintah ini berfungsi. 

1. Buat daftar semua grup sumber daya yang dibuat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-09b*'
   ```

1. Hapus semua grup sumber daya yang Anda buat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-09b*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Catatan**: Perintah dijalankan secara asinkron (sebagaimana yang ditentukan oleh parameter -AsJob), jadi saat Anda akan dapat menjalankan perintah PowerShell lain langsung setelahnya dalam sesi PowerShell yang sama, proses ini akan memakan waktu beberapa menit sebelum grup sumber daya benar-benar dihapus.

#### Tinjau

Di lab ini, Anda telah:

- Menyebarkan gambar Docker dengan menggunakan Azure Container Instance
- Meninjau fungsionalitas Instans Kontainer Azure
