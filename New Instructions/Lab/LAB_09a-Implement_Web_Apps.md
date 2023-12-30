---
lab:
  title: 'Lab 09a: Menerapkan Web Apps'
  module: Administer PaaS Compute Options
---

# Lab 09a - Menerapkan Aplikasi Web


## Pengenalan lab

Di lab ini, Anda mempelajari tentang aplikasi web Azure. Anda belajar mengonfigurasi aplikasi web untuk menampilkan aplikasi Halo Dunia di repositori GitHub eksternal. Anda belajar membuat slot penahapan dan bertukar dengan slot produksi. Anda juga mempelajari tentang autoscaling untuk mengakomodasi perubahan permintaan.

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur di lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkahnya ditulis menggunakan US Timur.

## Perkiraan waktu: 30 menit

## Skenario laboratorium

Organisasi Anda tertarik dengan aplikasi Web Azure untuk menghosting situs web organisasi Anda. Situs web saat ini dihosting di pusat data lokal perusahaan. Situs web berjalan di server Windows menggunakan tumpukan runtime PHP. Perangkat keras mendekati akhir masa pakai dan akan segera membutuhkan penggantian. Organisasi Anda ingin menyelesaikan pengujian untuk memfasilitasi perpindahan ke Azure sebelum tanggal akhir masa pakai.

## Simulasi lab interaktif

Ada simulasi lab interaktif yang mungkin berguna bagi Anda untuk topik ini. Simulasi ini memungkinkan Anda mengklik skenario serupa dengan kecepatan Anda sendiri. Ada perbedaan antara simulasi interaktif dan lab ini, tetapi banyak konsep intinya sama. Langganan Azure tidak diperlukan.

+ [Membuat aplikasi](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%202) web. Buat aplikasi web yang menjalankan kontainer Docker.
    
+ [Menerapkan aplikasi](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2013) web Azure. Buat aplikasi web Azure, kelola penyebaran, dan skalakan aplikasi. 

## Diagram arsitektur

![Diagram tugas.](../media/az104-lab09a-architecture-diagram.png)

## Tugas

+ Tugas 1: Membuat aplikasi web Azure
+ Tugas 2: Membuat slot penyebaran penahapan
+ Tugas 3: Mengonfigurasi pengaturan penyebaran aplikasi web
+ Tugas 4: Menukar slot penahapan
+ Tugas 5: Mengonfigurasi dan menguji penskalaan otomatis aplikasi web Azure

## Tugas 1: Membuat aplikasi web Azure

Dalam tugas ini, Anda akan membuat aplikasi web Azure. Azure menawarkan Azure App Services, yang merupakan solusi Platform As a Service (PAAS) untuk aplikasi web, seluler, dan berbasis web lainnya. Azure Web Apps, satu jenis penawaran Azure App Services, dapat digunakan untuk menjalankan situs web untuk sebagian besar lingkungan runtime, seperti PHP, Java, .NET, dan banyak lagi. Jika Anda memerlukan dukungan untuk lebih dari satu lingkungan runtime, Anda dapat menggunakan App Services dengan kontainer Docker. SKU yang Anda pilih menentukan jumlah komputasi, penyimpanan, dan fitur yang Anda terima dengan aplikasi web.

1. Masuk ke **portal Azure** - `https://portal.azure.com`.

1. Cari dan pilih `App services`.

1. Pilih **+ Buat**, dari menu drop-down, **Aplikasi web**. Perhatikan pilihan lainnya. 

1. Pada tab **Dasar-dasar** panel **Buat Aplikasi Web**, tentukan setelan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | ---|
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | `az104-rg9` (Jika perlu, pilih **Buat baru**) |
    | Nama aplikasi web | nama unik global apa pun |
    | Terbitkan | **Kode** |
    | Tumpukan runtime | **PHP 8.2** |
    | Sistem operasi | **Linux** |
    | Wilayah | **US Timur** |
    | Paket harga | menerima default |
    | Redundansi zona | menerima default |

 1. Klik **Tinjau + buat**, lalu **Buat**.

    >**Catatan**: Tunggu hingga aplikasi web dibuat sebelum Anda melanjutkan ke tugas berikutnya. Ini akan memakan waktu sekitar satu menit.

1. Pada panel penyebaran, klik **Buka sumber daya**.

## Tugas 2: Membuat slot penyebaran penahapan

Dalam tugas ini, Anda akan membuat slot penyebaran pentahapan. Slot penyebaran adalah fitur paket App Service tertentu yang memungkinkan Anda melakukan pengujian sebelum membuat aplikasi Anda tersedia untuk publik (atau pengguna akhir Anda). Setelah melakukan pengujian, Anda dapat menukar slot dari pengembangan atau penahapan ke produksi. Banyak organisasi menggunakan slot untuk melakukan pengujian pra-produksi. Selain itu, banyak organisasi menjalankan beberapa slot untuk setiap aplikasi (misalnya, pengembangan, QA, pengujian, dan produksi).

1. Pada bilah aplikasi web yang baru disebarkan, klik **tautan Domain** default untuk menampilkan halaman web default di tab browser baru.

1. Tutup tab browser baru dan, kembali ke portal Microsoft Azure, di bagian **Penyebaran** panel aplikasi web, klik **Slot penyebaran**.

    >**Catatan**: Aplikasi web, pada titik ini, memiliki satu slot penyebaran berlabel **PRODUKSI**.

1. Klik **+ Tambahkan slot**, dan tambahkan slot baru dengan setelan berikut:

    | Pengaturan | Nilai |
    | --- | ---|
    | Nama | `staging` |
    | Klon pengaturan dari | **Jangan klon pengaturan**|

1. Pilih **Tambahkan**.

1. Kembali ke penyebaran **Slot penyebaran** aplikasi web, klik entri yang mewakili slot pentahapan yang baru dibuat.

    >**Catatan**: Ini akan membuka bilah yang menampilkan properti slot penahapan.

1. Tinjau panel slot pentahapan dan perhatikan bahwa URL-nya berbeda dari yang ditetapkan ke slot produksi.

## Tugas 3: Mengonfigurasi pengaturan penyebaran aplikasi web

Dalam tugas ini, Anda akan mengonfigurasi pengaturan penyebaran aplikasi web. App Services dapat dikonfigurasi dengan pengaturan penyebaran untuk memungkinkan penyebaran berkelanjutan dari repositori pilihan Anda, atau dengan menggunakan kredensial FTPS dan otomatisasi lainnya. Ini memastikan bahwa layanan aplikasi memiliki versi terbaru aplikasi yang berjalan.

1. Pada bilah slot penyebaran penahapan, di bagian **Pengaturan**, pilih **Konfigurasi**, lalu pilih **Pengaturan** umum.

    >**Catatan:** Pastikan Anda berada di bilah slot penahapan (bukan slot produksi).
    
1. Pada tab **Pengaturan, di **daftar drop-down Sumber**, pilih **Git**** Eksternal.

1. Di bidang repositori, masukkan `https://github.com/Azure-Samples/php-docs-hello-world`

1. Di bidang cabang, masukkan `master`.

1. Pilih **Simpan**.

1. Dari slot penahapan, pilih **Gambaran Umum**.

1. **Pilih tautan Domain** default, dan buka URL di tab baru. 

1. Verifikasi bahwa slot penahapan menampilkan **Halo Dunia**.

## Tugas 4: Menukar slot penahapan

Dalam tugas ini, Anda akan menukar slot penahapan dengan slot produksi. Menukar slot memungkinkan Anda menggunakan kode yang telah Anda uji di slot penahapan Anda, dan memindahkannya ke produksi. portal Azure juga akan meminta Anda jika Anda perlu memindahkan pengaturan aplikasi lain yang telah Anda sesuaikan untuk slot. Menukar slot adalah tugas umum untuk tim aplikasi dan tim dukungan aplikasi, terutama yang menyebarkan pembaruan aplikasi rutin dan perbaikan bug.

1. Navigasi kembali ke panel yang menampilkan slot produksi aplikasi web.

1. Di bagian **Penyebaran**, klik **Slot penyebaran**, lalu klik ikon toolbar **Tukar**.

1. Pada panel **Tukar**, tinjau pengaturan default dan klik **Tukar**.

1. Klik **Gambaran Umum** pada bilah slot produksi aplikasi web lalu klik **tautan Domain** default untuk menampilkan halaman beranda situs web di tab browser baru.

    >**Catatan:** Salin URL** domain **Default, Anda akan memerlukannya untuk pengujian beban di tugas berikutnya. 

1. Pastikan halaman web default telah diganti dengan **Halo Dunia!** halaman.

## Tugas 5: Mengonfigurasi dan menguji penskalaan otomatis aplikasi web Azure

Dalam tugas ini, Anda akan mengonfigurasi penskalaan otomatis aplikasi web Azure. Penskalaan otomatis memungkinkan Anda mempertahankan performa optimal untuk aplikasi web Anda saat lalu lintas ke aplikasi web meningkat.  Untuk sebagian besar aplikasi, Anda mungkin tahu metrik tertentu di aplikasi yang harus menyebabkannya menskalakan. Ini bisa berupa penggunaan CPU, memori, atau bandwidth.

1. Pada panel yang menampilkan slot produksi aplikasi web, di bagian **Pengaturan**, klik **Peluasan skala (paket App Service)**.

1. Dari bagian **Penskalakan** , pilih **Otomatis**.

1. **Di bidang Ledakkan** maksimum, pilih **2**.

    ![Cuplikan layar halaman skala otomatis.](../media/az104-lab09a-autoscale.png)

1. Pilih **Simpan**.

    >**Catatan**: Di lingkungan produksi, organisasi sering memilih **Berbasis** Aturan dan mengonfigurasi aturan sekeliling metrik tertentu atau komponen Application Insights yang memicu penskalakan otomatis.

1. Pada bilah yang menampilkan slot produksi aplikasi web, pilih **Diagnosis dan selesaikan masalah**.

1. Dalam kotak **Uji Beban Aplikasi** Anda, pilih **Buat Uji** Beban.

    + Pilih **+ Buat** dan beri nama** uji **beban Anda.  Nama harus unik.
    + Pilih **Ulas + buat**, lalu pilih **Buat**.

1. Tunggu hingga pengujian beban dibuat, lalu pilih **Buka sumber daya**.

1. Dari **Gambaran Umum**** | Tambahkan permintaan** HTTP, pilih **Buat**.

1. **Untuk URL** Pengujian, tempelkan di URL domain** Default Anda**. Pastikan ini diformat dengan benar dan dimulai dengan **https://**.

1. Pilih **Tinjau + buat** dan **Buat.**

    >**Catatan:** Mungkin perlu waktu beberapa menit untuk membuat pengujian. 

1. Tinjau hasil pengujian termasuk **Pengguna virtual**, **Waktu** respons, dan **Permintaan/detik**.

1. Pilih **Hentikan** untuk menyelesaikan eksekusi pengujian.
   
## Tinjau titik utama lab

Selamat atas penyelesaian lab. Berikut adalah takeaway utama untuk lab ini. 

+ Azure App Services memungkinkan Anda membuat, menyebarkan, dan menskalakan aplikasi web dengan cepat.
+ App Service mencakup dukungan untuk banyak lingkungan pengembang termasuk ASP.NET, Java, PHP, dan Python.
+ Slot penyebaran memungkinkan Anda membuat lingkungan terpisah untuk menyebarkan dan menguji aplikasi web Anda.
+ Anda dapat menskalakan aplikasi web secara manual atau otomatis untuk menangani permintaan tambahan. 

## Membersihkan sumber daya Anda

Jika Anda bekerja dengan langganan Anda sendiri membutuhkan waktu satu menit untuk menghapus sumber daya lab. Ini akan memastikan sumber daya dibebankan dan biaya diminimalkan. Cara term mudah untuk menghapus sumber daya lab adalah dengan menghapus grup sumber daya lab. 

+ Di portal Azure, pilih grup sumber daya, pilih **Hapus grup** sumber daya, **Masukkan nama** grup sumber daya, lalu klik **Hapus**.

+ Menggunakan Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.

+ Menggunakan CLI, `az group delete --name resourceGroupName`.
    
