---
lab:
  title: 'Lab 09b: Menerapkan Kontainer Azure'
  module: Administer PaaS Compute Options
---

# Lab 09b - Menerapkan Kontainer Azure

## Pengenalan lab

Di lab ini, Anda mempelajari cara menerapkan Azure Container Instances dan Azure Container Apps. Anda belajar menyebarkan Azure Container Instance untuk menampilkan aplikasi Halo Dunia.
Anda belajar menyebarkan Aplikasi Kontainer Azure default. 

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur di lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkahnya ditulis menggunakan US Timur.

## Perkiraan waktu: 30 menit

## Skenario laboratorium

Organisasi Anda memiliki aplikasi web yang berjalan pada komputer virtual di pusat data lokal Anda. Organisasi ingin memindahkan semua aplikasi ke cloud tetapi tidak ingin memiliki sejumlah besar server untuk dikelola. Anda memutuskan untuk mengevaluasi Azure Container Instances dan Docker. Selain itu, Anda ingin menyebarkan dan menguji aplikasi kontainer Azure.

## Simulasi lab interaktif

Ada simulasi lab interaktif yang mungkin berguna bagi Anda untuk topik ini. Simulasi ini memungkinkan Anda mengklik skenario serupa dengan kecepatan Anda sendiri. Ada perbedaan antara simulasi interaktif dan lab ini, tetapi banyak konsep intinya sama. Langganan Azure tidak diperlukan.

+ [Menyebarkan Azure Container Instances](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%203). Membuat, mengonfigurasi, dan menyebarkan kontainer Docker dengan Azure Container Instances. 
+ [Menerapkan Azure Container Instances](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2014).  Sebarkan gambar Docker menggunakan Azure Container Instances. 

## Tugas

- Tugas 1: Menyebarkan Azure Container Instance menggunakan gambar Docker
- Tugas 2: Tinjau fungsionalitas Azure Container Instance
- Tugas 3: Membuat Aplikasi dan lingkungan Kontainer Azure
- Tugas 4: Menyebarkan dan menguji aplikasi kontainer

## Tugas 1 dan 2: Diagram Arsitektur Azure Container Instances

![Diagram tugas.](../media/az104-lab09baci-architecture-diagram.png)

## Tugas 1: Menyebarkan Azure Container Instance menggunakan gambar Docker

Dalam tugas ini, Anda akan membuat instans kontainer baru untuk aplikasi web. Docker adalah platform yang menyediakan kemampuan untuk mengemas dan menjalankan aplikasi di lingkungan terisolasi yang disebut kontainer. Azure Container Instances menyediakan lingkungan komputasi untuk gambar kontainer.

1. Masuk ke **portal Azure** - `https://portal.azure.com`.

1. Di portal Azure, cari dan pilih `Container instances` lalu, pada bilah **Instans** kontainer, klik **+ Buat**.

1. Pada tab **Dasar-dasar** pada bilah **Buat instans penampung**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | ---- | ---- |
    | Langganan | nama langganan Azure Anda |
    | Grup sumber daya | `az104-rg9` (Jika perlu, pilih **Buat baru**) |
    | Nama kontainer | `az104-c1` |
    | Wilayah | **US** Timur (atau wilayah yang tersedia di dekat Anda)|
    | Sumber Gambar | **Gambar memulai cepat** |
    | Gambar | **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)** |

1. Klik **Berikutnya: Jaringan >** dan, pada tab **Jaringan** dari bilah **Buat instans** kontainer, tentukan pengaturan berikut (biarkan orang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Label nama DNS | nama host DNS yang valid dan unik secara global |

    >**Catatan**: Kontainer Anda akan dapat dijangkau secara publik pada dns-name-label.region.azurecontainer.io. Jika Anda menerima pesan galat **DNS name label not available**, tentukan nilai yang berbeda.

1. Klik **Berikutnya: >** tingkat lanjut, tinjau pengaturan pada tab **Tingkat Lanjut** dari bilah **Buat instans** kontainer tanpa membuat perubahan apa pun.

 1. Klik **Tinjau + Buat**, pastikan validasi lulus lalu pilih **Buat**.

    >**Catatan**: Tunggu hingga penyebaran selesai. Ini akan memakan waktu 2-3 menit.

    >**Catatan**: Saat menunggu, Anda mungkin tertarik untuk melihat [kode di belakang aplikasi](https://github.com/Azure-Samples/aci-helloworld) sampel. Untuk melihatnya, jelajahi \\folder aplikasi.

## Tugas 2: Tinjau fungsionalitas Azure Container Instance

Dalam tugas ini, Anda akan meninjau penyebaran instans kontainer. Secara default, Azure Container Instance akan dapat diakses melalui port 80. Setelah instans disebarkan, Anda dapat menavigasi ke kontainer menggunakan nama DNS yang Anda berikan di tugas sebelumnya.

1. Pada bilah penyebaran, klik tautan **Buka sumber daya**.

1. Pada bilah **Ringkasan** instans penampung, verifikasi bahwa **Status** dilaporkan sebagai **Berjalan**.

1. Salin nilai instans kontainer **FQDN**, buka tab browser baru, dan navigasikan ke URL yang sesuai.

     ![Cuplikan layar halaman gambaran umum ACI di portal.](../media/az104-lab09b-aci-overview.png)

1. Pastikan halaman **Selamat datang di Azure Container Instance** ditampilkan.

1. Tutup tab browser baru, kembali ke portal Azure, di bagian **Pengaturan** bilah instans kontainer, klik **Kontainer**, lalu klik **Log**.

1. Verifikasi bahwa Anda melihat entri log yang mewakili permintaan HTTP GET yang dihasilkan dengan menampilkan aplikasi di browser.

## Tugas 3 dan 4: Diagram Arsitektur Azure Container Apps

![Diagram tugas.](../media/az104-lab09baca-architecture-diagram.png)

## Tugas 3: Membuat aplikasi dan lingkungan kontainer

Azure Container Apps mengambil konsep kluster Kubernetes terkelola selangkah lebih jauh dan mengelola lingkungan kluster serta menyediakan layanan terkelola lainnya di atas kluster. Tidak seperti kluster Azure Kubernetes, di mana Anda masih harus mengelola kluster, instans Azure Container Apps menghapus beberapa kompleksitas untuk menyiapkan kluster Kubernetes.

1. Dari portal Azure, cari dan pilih `Container Apps`.

1. Dari **Aplikasi** Kontainer, pilih **Buat**.

1. Gunakan informasi berikut untuk mengisi detail pada tab **Dasar** , lalu pilih **Berikutnya: Kontainer >**.

    | Pengaturan | Tindakan |
    |---|---|
    | Langganan | Pilih langganan Azure Anda |
    | Grup sumber daya | `az104-rg9` |
    | Nama aplikasi kontainer |  `my-app` |
    | Wilayah    | **US** Timur (Atau wilayah yang tersedia di dekat Anda) |
    | Lingkungan Aplikasi Kontainer | Biarkan default |

1. Pastikan bahwa **Gunakan gambar** mulai cepat diaktifkan dan gambar mulai cepat diatur ke **kontainer** Halo dunia sederhana.

1. Pilih tombol **Tinjau + buat** di bagian bawah halaman. 

    >**Catatan**: Jika ada kesalahan, tab apa pun yang berisi kesalahan ditandai dengan titik merah.  Buka tab yang sesuai. Bidang yang berisi kesalahan akan disorot dengan warna merah.  Setelah semua kesalahan diperbaiki, pilih **Tinjau dan buat** lagi.

1. Pilih **Buat**.

    >**Catatan:** Halaman dengan pesan *Penyebaran sedang berlangsung* ditampilkan.  Setelah penyebaran berhasil diselesaikan, Anda akan melihat pesan *Penyebaran Anda selesai*.
 
## Tugas 4: Menguji dan memverifikasi penyebaran aplikasi kontainer

Secara default, aplikasi kontainer Azure yang Anda buat akan menerima lalu lintas pada port 80 menggunakan sampel Halo Dunia aplikasi. Azure Container Apps akan memberikan nama DNS untuk aplikasi tersebut. Salin dan navigasikan ke URL ini untuk memastikan bahwa aplikasi aktif dan berjalan.

1. Pilih **Buka sumber daya** untuk melihat aplikasi kontainer baru Anda.

1. Pilih tautan di samping *URL Aplikasi* untuk melihat aplikasi Anda.

    ![Cuplikan layar halaman gambaran umum ACA di portal.](../media/az104-lab09b-aca-overview.png)

1. Verifikasi bahwa Anda menerima **aplikasi Azure Container Apps Anda adalah pesan langsung** .

## Tinjau titik utama lab

Selamat atas penyelesaian lab. Berikut adalah takeaway utama untuk lab ini. 

+ Azure Container Instances (ACI) adalah layanan yang memungkinkan Anda menyebarkan kontainer di cloud publik Microsoft Azure. ACI tidak mengharuskan Anda untuk menyediakan atau mengelola infrastruktur yang mendasar. Layanan ini mendukung kontainer Linux dan kontainer Windows.
+ Azure Container Apps (ACA) adalah platform tanpa server yang memungkinkan Anda mempertahankan lebih sedikit infrastruktur dan menghemat biaya saat menjalankan aplikasi kontainer. Alih-alih mengkhawatirkan konfigurasi server, orkestrasi kontainer, dan detail penyebaran, Container Apps menyediakan semua sumber daya server terbaru yang diperlukan untuk menjaga aplikasi Anda tetap stabil dan aman.
+ Beban kerja pada ACI biasanya dimulai dan dihentikan oleh beberapa jenis proses atau pemicu dan biasanya berumur pendek. Beban kerja di ACA biasanya merupakan proses yang berjalan lama seperti aplikasi Web.

## Membersihkan sumber daya Anda

Jika Anda bekerja dengan langganan Anda sendiri membutuhkan waktu satu menit untuk menghapus sumber daya lab. Ini akan memastikan sumber daya dibebankan dan biaya diminimalkan. Cara term mudah untuk menghapus sumber daya lab adalah dengan menghapus grup sumber daya lab. 

+ Di portal Azure, pilih grup sumber daya, pilih **Hapus grup** sumber daya, **Masukkan nama** grup sumber daya, lalu klik **Hapus**.

+ Menggunakan Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.

+ Menggunakan CLI, `az group delete --name resourceGroupName`.
