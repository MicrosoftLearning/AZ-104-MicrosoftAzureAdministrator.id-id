---
lab:
  title: 'Lab 03a: Mengelola sumber daya Azure dengan Menggunakan Portal Microsoft Azure'
  module: Administer Azure Resources
---

# Lab 03a - Kelola sumber daya Azure dengan Menggunakan Portal Azure
# Panduan lab siswa

## Skenario laboratorium

Anda perlu menjelajahi kemampuan administrasi dasar Azure yang terkait dengan provisi dan pengelolaan sumber daya berdasarkan grup sumber daya, termasuk memindahkan sumber daya di antara grup sumber daya. Anda juga ingin menjelajahi opsi untuk melindungi sumber daya disk agar tidak terhapus secara tidak sengaja, sambil tetap memungkinkan untuk memodifikasi karakteristik dan ukuran kinerjanya.

**Catatan:** Tersedia **[simulasi lab interaktif](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%204)** yang memungkinkan Anda mengklik lab ini sesuai keinginan Anda. Anda mungkin menemukan sedikit perbedaan antara simulasi interaktif dan lab yang dihosting, tetapi konsep dan ide utama yang ditunjukkan sama. 

## Tujuan

Di lab ini, kami akan:

+ Tugas 1: Membuat grup sumber daya dan menyebarkan sumber daya ke grup sumber daya
+ Tugas 2: Memindahkan sumber daya antar grup sumber daya
+ Tugas 3: Menerapkan dan menguji kunci sumber daya

## Perkiraan waktu: 20 menit

## Diagram arsitektur

![gambar](../media/lab03a.png)

### Petunjuk

## Latihan 1

## Tugas 1: Membuat grup sumber daya dan menyebarkan sumber daya ke grup sumber daya

Dalam tugas ini, Anda akan menggunakan portal Azure untuk membuat grup sumber daya dan membuat disk di grup sumber daya.

1. Masuk ke [**portal Azure**](http://portal.azure.com).

1. Di portal Azure, cari dan pilih **Disk**, klik **+ Buat** dan tentukan pengaturan berikut:

    |Pengaturan|Nilai|
    |---|---|
    |Langganan| nama langganan Azure tempat Anda membuat grup sumber daya |
    |Grup Sumber Daya| nama grup sumber daya baru **az104-03a-rg1** |
    |Nama disk| **az104-03a-disk1** |
    |Wilayah| **(AS) AS Timur** |
    |Zona ketersediaan| **Tidak ada redundansi infrastruktur yang diperlukan** |
    |Jenis sumber| **Tidak ada** |

    >**Catatan**: Saat membuat sumber daya, Anda memiliki opsi untuk membuat grup sumber daya baru atau menggunakan yang sudah ada.

1. Ubah jenis dan ukuran disk masing-masing menjadi **HDD Standar** dan **32 GiB**.

1. Klik **Tinjau + Buat**, lalu klik **Buat**.

    >**Catatan**: Tunggu hingga disk dibuat. Ini seharusnya memakan waktu kurang dari satu menit.

## Tugas 2: Memindahkan sumber daya antar grup sumber daya 

Dalam tugas ini, kami akan memindahkan sumber daya disk yang Anda buat di tugas sebelumnya ke grup sumber daya baru. 

1. Cari dan pilih **Grup sumber daya**. 

1. Pada panel **Grup sumber daya**, klik entri yang mewakili grup sumber daya **az104-03a-rg1** yang Anda buat di tugas sebelumnya.

1. Dari bilah **Gambaran Umum** grup sumber daya, dalam daftar sumber daya grup sumber daya, pilih entri yang mewakili disk yang baru dibuat, klik **Pindahkan** di toolbar, dan di menu drop-down, pilih **Pindahkan ke grup sumber daya lain**.

    >**Catatan**: Metode ini memungkinkan Anda memindahkan beberapa sumber daya secara bersamaan. 

1. Di bawah kotak teks **Grup sumber daya**, klik **Buat baru** lalu ketik **az104-03a-rg2** di kotak teks. Pada tab Ulasan, centang kotak **Saya memahami bahwa alat dan skrip yang terkait dengan sumber daya yang dipindahkan tidak akan berfungsi sampai saya memperbaruinya untuk menggunakan ID sumber daya baru**, dan klik **Pindahkan**.

    >**Catatan**: Jangan tunggu hingga pemindahan selesai tetapi lanjutkan ke tugas berikutnya. Perpindahan ini mungkin memakan waktu sekitar 10 menit. Anda dapat menentukan bahwa operasi telah selesai dengan memantau entri log aktivitas sumber atau grup sumber daya target. Tinjau kembali langkah ini setelah Anda menyelesaikan tugas berikutnya.

## Tugas 3: Menerapkan kunci sumber daya

Dalam tugas ini, Anda akan menerapkan kunci sumber daya ke grup sumber daya Azure yang berisi sumber daya disk.

1. Di portal Azure, cari dan pilih **Disk**, klik **+ Buat** dan tentukan pengaturan berikut:

    |Pengaturan|Nilai|
    |---|---|
    |Langganan| nama langganan yang Anda gunakan di lab ini |
    |Grup Sumber Daya| klik **buat baru** grup sumber daya dan beri nama **az104-03a-rg3** |
    |Nama disk| **az104-03a-disk2** |
    |Wilayah| nama wilayah Azure tempat Anda membuat grup sumber daya lain di lab ini |
    |Zona ketersediaan| **Tidak ada redundansi infrastruktur yang diperlukan** |
    |Jenis sumber| **Tidak ada** |

1. Atur jenis dan ukuran disk masing-masing ke **HDD Standar** dan **32 GiB**.

1. Klik **Tinjau + Buat**, lalu klik **Buat**.

1. Klik **Buka sumber daya**.

1. Pada laman Gambaran Umum Disk, klik nama grup sumber daya, **az104-03a-rg3**.

1. Pada panel grup sumber daya **az104-03a-rg3**, klik **Kunci** lalu **+ Tambahkan** dan tentukan pengaturan berikut:

    |Pengaturan|Nilai|
    |---|---|
    |Nama kunci| **az104-03a-delete-lock** |
    |Jenis kunci| **Hapus** |
    
1. Klik **OK**    

1. Pada panel grup sumber daya **az104-03a-rg3**, klik **Gambaran Umum**, dalam daftar sumber daya grup sumber daya, pilih entri yang mewakili disk yang Anda buat sebelumnya dalam tugas ini, dan klik **Hapus** di toolbar. 

1. Saat diminta **Apakah Anda ingin menghapus semua sumber daya yang dipilih?**, di kotak teks **Konfirmasi hapus**, ketik **ya** dan klik **Hapus**.

1. Anda akan melihat pesan kesalahan, yang memberitahukan tentang operasi penghapusan yang gagal. 

    >**Catatan**: Seperti yang dinyatakan oleh pesan kesalahan, ini diharapkan karena kunci penghapusan diterapkan pada tingkat grup sumber daya.

1. Navigasikan kembali ke daftar sumber daya grup sumber daya **az104-03a-rg3** dan klik entri yang mewakili sumber daya **az104-03a-disk2**. 

1. Pada blade **az104-03a-disk2**, di bagian **Pengaturan**, klik **Ukuran + performa**, atur jenis dan ukuran disk ke **SSD Premium** dan **64 GiB**, dan klik **Simpan** untuk menerapkan perubahan. Verifikasi bahwa perubahan berhasil.

    >**Catatan**: Ini diharapkan, karena kunci tingkat grup sumber daya hanya berlaku untuk operasi penghapusan. 

## Membersihkan sumber daya

   >**Catatan**: Jangan hapus sumber daya yang Anda sebarkan di lab ini. Anda akan menggunakannya di lab berikutnya dari modul ini. Hapus hanya kunci sumber daya yang Anda buat di lab ini.

1. Navigasikan ke panel grup sumber daya **az104-03a-rg3**, tampilkan panel **Kunci**, dan lepaskan kunci **az104-03a-delete-lock** dengan mengeklik **Hapus** tautan di sisi kanan entri kunci **Hapus**.

## Tinjau

Di lab ini, Anda telah:

- Membuat grup sumber daya dan menyebarkan sumber daya ke grup sumber daya
- Memindahkan sumber daya antar kelompok sumber daya
- Kunci sumber daya yang diterapkan dan diuji
