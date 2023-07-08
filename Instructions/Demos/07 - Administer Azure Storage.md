---
demo:
  title: 'Demonstrasi 07: Mengelola Azure Storage'
  module: Administer Azure Storage
---


# 07 - Mengelola Azure Storage

## Mengonfigurasi Akun Penyimpanan

Dalam demonstrasi ini, kami akan membuat akun penyimpanan.

**Referensi**: [Membuat akun penyimpanan](https://docs.microsoft.com/azure/storage/common/storage-account-create?tabs=azure-portal)

1. Menggunakan portal Microsoft Azure.

1. Tinjau tujuan akun penyimpanan. 
   
1. Cari dan pilih **Akun Penyimpanan**. 
 
1. Buat akun penyimpanan dasar. 

    - Diskusikan persyaratan tentang penamaan akun penyimpanan dan kebutuhan nama menjadi unik di Azure. 

    - Tinjau berbagai jenis penyimpanan. Misalnya, tujuan umum v2. 

    - Tinjau pilihan tingkat akses. Misalnya, tingkat dingin dan panas. 

    - Tab dan pengaturan lain akan tercakup dalam demonstrasi lain. 

1. Buat akun penyimpanan dan tunggu hingga sumber daya disebarkan. 


## Mengonfigurasi Blob Storage

Dalam demonstrasi ini, kita akan menjelajahi penyimpanan blob.

**Catatan:** Langkah-langkah ini memerlukan akun penyimpanan.

**Referensi**: [Mulai cepat: Mengunggah, mengunduh, dan mencantumkan blob](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)

1. Navigasikan ke akun penyimpanan di portal Azure.

1. Tinjau tujuan penyimpanan blob. 

1. Membuat kontainer blob. Tinjau tingkat akses untuk kontainer. Misalnya, privat (tidak ada akses anonim). 

1. Unggah blob ke kontainer. Karena Anda memiliki waktu untuk meninjau pengaturan tingkat lanjut. Misalnya, jenis blob dan ukuran blob. 

## Mengonfigurasi Azure Files 

Dalam demonstrasi ini, kita akan bekerja dengan berbagi file dan snapshot.

**Catatan:** Langkah-langkah ini memerlukan akun penyimpanan.

**Referensi**: [Mulai cepat untuk mengelola berbagi file Azure](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-portal?tabs=azure-portal)

1. Tinjau tujuan berbagi file. 

1. Akses akun penyimpanan dan klik **File**.

1. Buat berbagi file. Tinjau kuota, unggah file, dan tambahkan direktori untuk mengatur informasi. 

1. Buat rekam jepret berbagi file. Tinjau kapan harus menggunakan rekam jepret dan perbedaannya dengan cadangan. Saat Anda memiliki waktu, unggah file, ambil rekam jepret, hapus file, dan pulihkan rekam jepret. 


## Mengonfigurasi Keamanan Penyimpanan

Dalam demonstrasi ini, kami akan membuat tanda tangan akses bersama.

**Catatan:** Demonstrasi ini memerlukan akun penyimpanan, dengan kontainer blob, dan file yang diunggah.

**Referensi**: [Membuat token SAS untuk kontainer penyimpanan](https://learn.microsoft.com/azure/applied-ai-services/form-recognizer/create-sas-tokens?source=recommendations&view=form-recog-3.0.0)

1. Pilih blob atau file yang ingin Anda amankan. 

1. Buat tanda tangan akses bersama (SAS). Tinjau izin, waktu mulai dan kedaluwarsa, dan protokol yang diizinkan.

1. Gunakan URL SAS untuk memastikan tampilan sumber daya. 


## Alat Penyimpanan (opsional)

Dalam demonstrasi ini, kami akan meninjau beberapa alat penyimpanan Azure umum. 

**Referensi**: [Mulai menggunakan Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows)

1. Instal Penjelajah Penyimpanan atau gunakan Browser Penyimpanan.

1. Tinjau cara menelusuri dan membuat sumber daya penyimpanan. Misalnya, tambahkan kontainer blob. 

**Referensi**: [Menyalin atau memindahkan data ke Azure Storage dengan menggunakan AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=/azure/storage/files/toc.json)

1. Diskusikan kapan harus menggunakan AzCopy. Lihat bantuan, **azcopy /?**.

1. Gulir ke bawah bagian **Sampel** . Saat Anda memiliki waktu, cobalah salah satu contohnya. 
    



