---
lab:
  title: 'Lab 09a: Menerapkan Aplikasi Web'
  module: Administer PaaS Compute Options
---

# Lab 09a - Menerapkan Aplikasi Web


## Pengantar lab

Di lab ini, Anda mempelajari tentang aplikasi web Azure. Anda belajar mengonfigurasi aplikasi web untuk menampilkan aplikasi Halo Dunia di repositori GitHub eksternal. Anda belajar membuat slot penahapan dan bertukar dengan slot produksi. Anda juga mempelajari tentang penskalaan otomatis untuk mengakomodasi perubahan permintaan.

Lab ini memerlukan langganan Azure. Tipe langganan Anda dapat memengaruhi ketersediaan fitur di lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkah lab ini ditulis menggunakan US Timur.

## Perkiraan waktu: 20 menit

## Skenario lab

Organisasi Anda tertarik dengan aplikasi Web Azure untuk meng-hosting situs web perusahaan Anda. Situs web ini saat ini di-hosting di pusat data lokal. Situs web berjalan di server Windows menggunakan tumpukan runtime PHP. Perangkat keras mendekati akhir masa pakai dan akan segera perlu diganti. Organisasi Anda ingin menghindari biaya perangkat keras baru dengan menggunakan Azure untuk meng-hosting situs web. 

## Simulasi lab interaktif

Ada simulasi lab interaktif yang mungkin berguna bagi Anda untuk topik ini. Simulasi ini memungkinkan Anda mengeklik skenario serupa secara mandiri. Ada perbedaan antara simulasi interaktif dan lab ini, tetapi banyak konsep intinya sama. Langganan Azure tidak diperlukan.

+ [Membuat aplikasi web](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%202). Buat aplikasi web yang menjalankan kontainer Docker.
    
+ [Menerapkan aplikasi web Azure](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2013). Buat aplikasi web Azure, kelola penyebaran, dan skalakan aplikasi. 

## Diagram arsitektur

![Diagram tugas.](../media/az104-lab09a-architecture.png)

## Keterampilan pekerjaan

+ Tugas 1: Membuat dan mengonfigurasi penyewa aplikasi web Azure.
+ Tugas 2: Membuat dan mengonfigurasi slot penyebaran.
+ Tugas 3: Mengonfigurasikan pengaturan penyebaran aplikasi web.
+ Tugas 4: Menukar slot penyebaran.
+ Tugas 5: Mengonfigurasi dan menguji penskalaan otomatis aplikasi web Azure.

## Tugas 1: Membuat dan mengonfigurasi penyewa aplikasi web Azure

Dalam tugas ini, Anda membuat aplikasi web Azure. Azure App Services adalah solusi Platform as a Service (PAAS) untuk aplikasi web, aplikasi seluler, dan aplikasi berbasis web lainnya. Aplikasi web Azure adalah bagian dari Azure App Services yang meng-hosting sebagian besar lingkungan runtime, seperti PHP, Java, dan .NET. Paket layanan aplikasi yang Anda pilih menentukan komputasi, penyimpanan, dan fitur aplikasi web. 

1. Masuk ke **portal Azure** - `https://portal.azure.com`.

1. Cari dan pilih `App services`.

1. Pilih **+ Buat**, dari menu drop-down, **Aplikasi Web**. Perhatikan pilihan yang lain. 

1. Pada tab **Dasar-dasar** panel **Buat Aplikasi Web**, tentukan setelan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | ---|
    | Langganan | langganan Azure Anda |
    | Grup sumber daya | `az104-rg9` (Jika perlu, pilih **Buat baru**) |
    | Nama aplikasi web | nama unik global apa pun |
    | Terbitkan | **Kode** |
    | Tumpukan runtime | **PHP 8.2** |
    | Sistem operasi | **Linux** |
    | Wilayah | **US Timur** |
    | Paket harga | **Premium V3 P1V3** |
    | Redundansi zona | menerima default |

 1. Klik **Tinjau + buat.**, lalu klik **Buat**.

    >**Catatan**: Tunggu hingga Aplikasi Web dibuat sebelum Anda melanjutkan ke tugas berikutnya. Ini akan memakan waktu sekitar satu menit.
    
    >**Catatan**: Jika penyebaran gagal, ubah ke wilayah lain dan coba lagi. Misalnya, beralih ke **US Timur 2**. 

1. Setelah penyebaran, pilih **Buka sumber daya**.

## Tugas 2: Membuat dan mengonfigurasi slot penyebaran

Dalam tugas ini, Anda akan membuat slot penyebaran pentahapan. Slot penyebaran memungkinkan Anda melakukan pengujian sebelum menjadikan aplikasi Anda tersedia untuk publik (atau pengguna akhir Anda). Setelah melakukan pengujian, Anda dapat menukar slot dari pengembangan atau penahapan ke produksi. Banyak organisasi menggunakan slot untuk melakukan pengujian pra-produksi. Selain itu, banyak organisasi menjalankan beberapa slot untuk setiap aplikasi (misalnya, pengembangan, QA, pengujian, dan produksi).

1. Pada panel Aplikasi Web yang baru disebarkan, klik tautan **Domain default** untuk menampilkan halaman web default di tab browser baru.

1. Tutup tab browser baru dan, kembali ke portal Microsoft Azure, di bagian **Penyebaran** panel Aplikasi Web, klik **Slot penyebaran**.

    >**Catatan**: Aplikasi Web, pada titik ini, memiliki satu slot penerapan berlabel **PRODUCTION**.

1. Klik **Tambahkan slot**, dan tambahkan slot baru dengan setelan berikut:

    | Pengaturan | Nilai |
    | --- | ---|
    | Nama | `staging` |
    | Klon pengaturan dari | **Jangan klon pengaturan**|

1. Pilih **Tambahkan**.

1. Kembali ke penyebaran **Slot penyebaran** Aplikasi Web, klik entri yang mewakili slot penahapan yang baru dibuat.

    >**Catatan**: Ini akan membuka panel yang menampilkan properti slot pentahapan.

1. Tinjau panel slot pentahapan dan perhatikan bahwa URL-nya berbeda dari yang ditetapkan ke slot produksi.

## Tugas 3: Konfigurasikan pengaturan penyebaran Aplikasi Web

Dalam tugas ini, Anda akan mengonfigurasi pengaturan penyebaran Aplikasi Web. Pengaturan penyebaran memungkinkan penyebaran berkelanjutan. Hal ini memastikan agar layanan aplikasi memiliki versi aplikasi yang terbaru.

1. Di slot penahapan, pilih **Pusat Penyebaran** lalu pilih **Pengaturan**.

    >**Catatan:** Pastikan Anda berada di panel slot penahapan (bukan slot produksi).
    
1. Di daftar drop-down **Sumber**, pilih **Git Eksternal**. Perhatikan pilihan yang lain. 

1. Di bidang repositori, masukkan `https://github.com/Azure-Samples/php-docs-hello-world`

1. Di bidang cabang, masukkan `master`.

1. Pilih **Simpan**.

1. Dari slot penahapan, pilih **Gambaran Umum**.

1. Pilih tautan **Domain default**, lalu buka URL di tab baru. 

1. Verifikasikan bahwa slot penahapan menampilkan **Halo Dunia**.

>**Catatan:** Penerapan mungkin membutuhkan waktu satu menit. Pastikan untuk **Me-refresh** halaman aplikasi.

## Tugas 4: Menukar slot penyebaran

Dalam tugas ini, Anda akan menukar slot penahapan dengan slot produksi. Menukar slot memungkinkan Anda menggunakan kode yang telah Anda uji di slot penahapan Anda dan memindahkannya ke produksi. Portal Azure juga akan meminta Anda jika Anda perlu memindahkan pengaturan aplikasi lain yang telah Anda sesuaikan untuk slot tersebut. Menukar slot adalah tugas umum untuk tim aplikasi dan tim dukungan aplikasi, terutama yang menyebarkan pembaruan aplikasi rutin dan perbaikan bug.

1. Navigasikan kembali ke bilah **Slot penyebaran**, lalu pilih **Tukar**.

1. Tinjau pengaturan default dan klik **Mulai Pertukaran**.

1. Pada bilah **Gambaran Umum** Aplikasi Web, pilih tautan **Domain default** untuk menampilkan beranda situs web.

1. Verifikasikan halaman web produksi menampilkan **Halo Dunia!** halaman.

    >**Catatan:** Salin URL **Default domain** karena Anda akan memerlukannya untuk pengujian beban di tugas berikutnya. 

## Tugas 5: Mengonfigurasi dan menguji penskalaan otomatis Aplikasi Web Azure

Dalam tugas ini, Anda akan mengonfigurasi dan menguji penskalaan otomatis Aplikasi Web Azure. Penskalaan otomatis memungkinkan Anda mempertahankan aplikasi web Anda dalam performa optimal saat lalu lintas ke aplikasi web meningkat. Untuk menentukan kapan aplikasi harus diskalakan, Anda dapat memantau metrik seperti penggunaan CPU, memori, atau bandwidth.

1. Di bagian **Pengaturan**, pilih **Peluasan skala (paket App Service)**.

    >**Catatan:** Pastikan Anda mengerjakan slot produksi dan bukan slot penahapan.  

1. Dari bagian **Penskalaan**, pilih **Otomatis**. Perhatikan opsi **Berbasis Aturan**. Penskalaan berbasis aturan dapat dikonfigurasi untuk metrik aplikasi yang berbeda. 

1. Di bidang **Burst maksimum**, pilih **2**.

    ![Cuplikan layar halaman penskalaan otomatis.](../media/az104-lab09a-autoscale.png)

1. Pilih **Simpan**.

1. Pilih **Diagnosis dan selesaikan masalah** (panel kiri).

1. Dalam kotak **Uji Beban Aplikasi Anda**, pilih **Buat Pengujian Beban**.

    + Pilih **+ Buat** dan beri **nama** untuk pengujian beban Anda.  Nama harus unik.
    + Pilih **Ulas + buat**, lalu pilih **Buat**.

1. Tunggu hingga pengujian beban dibuat, lalu pilih **Buka sumber daya**.

1. Dari **Gambaran Umum** | **Tambahkan permintaan HTTP**, pilih **Buat**.

1. Pada tab **Uji paket** , klik **Tambahkan permintaan**. **Di bidang** URL, tempelkan di URL domain** Default Anda**. Pastikan ini diformat dengan benar dan dimulai dengan **https://**.

1. Pilih **Tinjau + buat** dan **Buat.**

    >**Catatan:** Mungkin butuh waktu beberapa menit untuk membuat pengujian. 

1. Tinjau hasil pengujian termasuk **Pengguna virtual**, **Waktu respons**, dan **Permintaan/detik**.

1. Pilih **Hentikan** untuk menyelesaikan eksekusi pengujian. Anda tidak perlu menunggu pengujian selesai. 

## Bersihkan sumber daya Anda

Jika Anda bekerja dengan **langganan Anda sendiri** luangkan waktu sebentar untuk menghapus sumber daya lab. Hal ini akan memastikan sumber daya dikosongkan dan biaya diminimalkan. Cara termudah untuk menghapus sumber daya lab adalah dengan menghapus grup sumber daya lab. 

+ Di portal Microsoft Azure, pilih grup sumber daya, pilih **Hapus grup sumber daya**, **Masukkan nama grup sumber daya**, lalu klik **Hapus**.
+ Menggunakan Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Menggunakan CLI, `az group delete --name resourceGroupName`.

## Perluas pemelajaran Anda dengan Copilot
Copilot dapat membantu Anda mempelajari cara menggunakan alat pembuatan skrip Azure. Copilot juga dapat membantu di area yang tidak tercakup dalam lab atau ketika Anda memerlukan informasi lebih lanjut. Buka browser Edge dan pilih Copilot (kanan atas) atau navigasi *copilot.microsoft.com*. Luangkan beberapa menit untuk mencoba perintah ini.

+ Ringkas langkah-langkah untuk membuat dan mengonfigurasi aplikasi web Azure.
+ Apa saja cara untuk menskalakan Aplikasi Web Azure?

## Pelajari lebih lanjut dengan pelatihan mandiri

+ [Lakukan penahapan penyebaran aplikasi web untuk pengujian dan pemutaran kembali dengan menggunakan slot penyebaran App Service](https://learn.microsoft.com/training/modules/stage-deploy-app-service-deployment-slots/). Gunakan slot penyebaran untuk menyederhanakan penyebaran dan mengembalikan aplikasi web di Azure App Service.
+ [Skalakan aplikasi web App Service untuk memenuhi permintaan secara efisien dengan meningkatkan dan meluaskan skala App Service](https://learn.microsoft.com/training/modules/app-service-scale-up-scale-out/). Tanggapi periode peningkatan aktivitas dengan meningkatkan sumber daya yang tersedia secara bertahap, lalu membebaskan sumber daya ini saat aktivitas turun.

## Poin penting

Selamat atas penyelesaian lab ini. Berikut adalah kesimpulan utama dari lab ini. 

+ Azure App Services memungkinkan Anda membuat, menyebarkan, dan menskalakan aplikasi web dengan cepat.
+ App Service mencakup dukungan untuk banyak lingkungan pengembang, termasuk ASP.NET, Java, PHP, dan Python.
+ Slot penyebaran memungkinkan Anda membuat lingkungan terpisah untuk menyebarkan dan menguji aplikasi web Anda.
+ Anda dapat menskalakan aplikasi web secara manual atau otomatis untuk menangani permintaan tambahan.
+ Tersedia berbagai alat diagnostik dan pengujian. 
