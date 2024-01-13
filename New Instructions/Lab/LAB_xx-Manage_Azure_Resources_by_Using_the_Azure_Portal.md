---
lab:
  title: 'Lab 03a: Mengelola sumber daya Azure dengan Menggunakan Portal Microsoft Azure'
  module: Administer Azure Resources
---

# Lab 03a - Kelola sumber daya Azure dengan Menggunakan Portal Azure
# Panduan lab siswa

## Skenario laboratorium

Anda perlu menjelajahi kemampuan administratif Azure dasar yang terkait dengan penyediaan sumber daya dan mengaturnya.  Anda ingin memahami metode yang paling umum di Azure untuk menata sumber daya. Anda juga ingin memahami cara memindahkan sumber daya. Terakhir, Anda ingin menguji melindungi sumber daya disk agar tidak dihapus secara tidak sengaja, sambil tetap memungkinkan untuk memodifikasi karakteristik dan ukuran performa.

**Catatan:** Tersedia **[simulasi lab interaktif](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%204)** yang memungkinkan Anda mengklik lab ini sesuai keinginan Anda. Anda mungkin menemukan sedikit perbedaan antara simulasi interaktif dan lab yang dihosting, tetapi konsep dan ide utama yang ditunjukkan sama. 

## Tujuan

Di lab ini, kami akan:

+ Tugas 1: Membuat grup sumber daya, membuat sumber daya, dan menyebarkan sumber daya ke grup sumber daya
+ Tugas 2: Memindahkan sumber daya antar grup sumber daya
+ Tugas 3: Menerapkan dan menguji kunci sumber daya

## Perkiraan waktu: 30 menit

## Diagram arsitektur

![Arsitektur diagram lab 03a](../media/az104-lab03a-architecture-diagram.png)

### Petunjuk

## Latihan 1

## Tugas 1: Membuat grup sumber daya dan menyebarkan sumber daya ke grup sumber daya

Dalam tugas ini, Anda akan menggunakan portal Azure untuk membuat disk terkelola. Dalam tugas berikutnya, kita akan memindahkan disk ini ke grup sumber daya lain. 

1. Masuk ke [**portal Azure**](http://portal.azure.com).

1. Di portal Azure, cari dan pilih **Disk**, klik **+ Buat** dan tentukan pengaturan berikut:

    |Pengaturan|Nilai|
    |---|---|
    |Langganan| nama langganan Azure Anda  |
    |Grup Sumber Daya| pilih **buat grup sumber daya baru** - `az104-rg3a`|
    |Nama disk| `az104-disk1` |
    |Wilayah| **(AS) AS Timur** |
    |Zona ketersediaan| **Tidak ada redundansi infrastruktur yang diperlukan** |
    |Jenis sumber| **Tidak** |

1. Pilih **Ubah ukuran** dan pilih **HDD** Standar dan **32 GiB**. Pilih **OK** saat selesai. 

    ![Cuplikan layar halaman buat disk.](../media/az104-lab03a-createdisk1.png)

1. Klik **Tinjau + Buat**, lalu klik **Buat**.

    >**Catatan**: Tunggu hingga disk dibuat. Ini seharusnya memakan waktu kurang dari satu menit.

## Tugas 2: Memindahkan sumber daya antar grup sumber daya 

Dalam tugas ini, kami akan memindahkan sumber daya disk yang Anda buat di tugas sebelumnya ke grup sumber daya baru. Sumber daya individual, seperti disk, hanya dapat ditempatkan dalam satu grup sumber daya. Seringkali, organisasi menerapkan kontrol akses berbasis peran, pemberian tag, atau Azure Policy di tingkat grup sumber daya. Anda mungkin perlu memindahkan sumber daya ke grup sumber daya yang berbeda jika sumber daya direalokasikan ke proyek lain, jika Anda ingin memanfaatkan konfigurasi grup sumber daya yang ada (misalnya, kontrol akses berbasis peran dan pemberian tag sudah dikonfigurasi), atau jika Anda berencana untuk menonaktifkan grup sumber daya asli.

1. Cari dan pilih **Grup sumber daya**. 

1. Pada bilah **Grup** sumber daya, klik entri yang mewakili **grup sumber daya az104-rg3a** yang Anda buat di tugas sebelumnya.

1. Dari bilah **Gambaran Umum** grup sumber daya, dalam daftar sumber daya grup sumber daya, pilih entri yang mewakili disk yang baru dibuat.

1. Gunakan elipsis bilah alat (...) untuk memilih **Pindahkan**, lalu di daftar drop-down, pilih **Pindahkan ke grup** sumber daya lain. Perhatikan pilihan Anda untuk pindah ke langganan atau wilayah lain. 

    ![Cuplikan layar untuk memindahkan sumber daya.](../media/az104-lab03a-moverg.png)

1. Di bawah kotak **teks Grup** sumber daya, klik **Buat baru** lalu ketik `az104-rg3move` di kotak teks. Pilih **OK** untuk membuat grup sumber daya baru.

1. Pada tab **Sumber Daya untuk dipindahkan** , verifikasi **Berhasil** adalah status validasi. Mungkin perlu waktu satu menit agar validasi selesai. Sementara itu, tinjau [daftar operasi pemindahan yang](https://learn.microsoft.com/azure/azure-resource-manager/management/move-support-resources) didukung. Pilih **Berikutnya** untuk melanjutkan. 

1. Pada tab Ulasan, centang kotak **Saya memahami bahwa alat dan skrip yang terkait dengan sumber daya yang dipindahkan tidak akan berfungsi sampai saya memperbaruinya untuk menggunakan ID sumber daya baru**, dan klik **Pindahkan**.

    >**Catatan**: Jangan tunggu hingga pemindahan selesai tetapi lanjutkan ke tugas berikutnya. Perpindahan ini mungkin memakan waktu sekitar 10 menit. Anda dapat menentukan bahwa operasi telah selesai dengan memantau entri log aktivitas sumber atau grup sumber daya target. Tinjau kembali langkah ini setelah Anda menyelesaikan tugas berikutnya.

## Tugas 3: Menerapkan kunci sumber daya

Dalam tugas ini, Anda akan menerapkan kunci sumber daya ke grup sumber daya Azure yang berisi sumber daya disk. Kunci sumber daya dapat mencegah semua jenis perubahan pada sumber daya, seperti ukuran disk. Atau, kunci sumber daya dapat mencegah penghapusan yang tidak disengaja. Kedua jenis kunci ini adalah *Hapus* kunci dan *kunci Baca-Saja* .

### Buat {i>disk<i} terkelola baru. 

1. Di portal Azure, cari dan pilih **Disk**, klik **+ Buat** dan tentukan pengaturan berikut:

    |Pengaturan|Nilai|
    |---|---|
    |Langganan| nama langganan yang Anda gunakan di lab ini |
    |Grup Sumber Daya| klik **buat grup sumber daya baru** dan beri nama `az104-rg3` |
    |Nama disk| `az104-disk2` |
    |Wilayah| nama wilayah Azure tempat Anda membuat grup sumber daya lain di lab ini |
    |Zona ketersediaan| **Tidak ada redundansi infrastruktur yang diperlukan** |
    |Jenis sumber| **Tidak** |

1. Pilih **Ubah ukuran** dan pilih **HDD** Standar dan **32 GiB**. Pilih **OK** saat selesai. 

    ![Cuplikan layar pengaturan jenis dan ukuran disk.](,./media/az104-lab03a-createdisk1.png)

1. Klik **Tinjau + Buat**, lalu klik **Buat**.

1. Tunggu hingga disk disebarkan, lalu pilih **Buka sumber daya**.

### Kunci grup sumber daya sehingga sumber daya tidak dapat didelekasikan. 

1. Dari bilah **Gambaran Umum** , pilih grup sumber daya Anda.

1. Di bagian **Pengaturan**, pilih **Kunci**, lalu pilih **+ Tambahkan**. Tentukan pengaturan berikut lalu pilih **OK**. 

    |Pengaturan|Nilai|
    |---|---|
    |Nama kunci| `az104-rglock` |
    |Jenis kunci| **Hapus** |

    ![Cuplikan layar kunci sumber daya.](../media/az104-lab03a-deletelock.png)

1. Dari bilah Gambaran Umum** grup **sumber daya, pilih disk terkelola, lalu pilih **Hapus** di toolbar. 

1. Pada halaman **Hapus sumber daya** , dalam kotak **teks Konfirmasi hapus** , ketik `delete` dan klik **Hapus**. Konfirmasi konfirmasi** Hapus Anda**. 

1. Anda akan melihat pesan kesalahan, memberi tahu Anda bahwa operasi penghapusan gagal.

    >**Catatan**: Ini diharapkan. Pesan menyatakan ada kunci pada grup sumber daya. 

    ![Cuplikan layar pesan kesalahan.](../media/az104-lab03a-deleteerror.png)

1. Pilih **sumber daya az104-disk2** . 

1. Di bagian **Pengaturan, klik **Ukuran + performa****. Ubah ke **SSD** Premium dan **64 GiB**. Klik **Simpan** untuk menerapkan perubahan.

1. Dari bilah **Gambaran Umum** , konfirmasikan bahwa Anda dapat mengubah ukuran disk. 

    >**Catatan**: Ini diharapkan. Kunci tingkat grup sumber daya hanya berlaku untuk menghapus operasi. Anda dapat mengedit pengaturan sumber daya. 

## Tinjau

Selamat! Anda telah berhasil membuat grup sumber daya dan sumber daya, memindahkan sumber daya ke grup lain, dan menerapkan kunci sumber daya.
