---
lab:
  title: 'Lab 09c: Menerapkan Azure Container Apps'
  module: Administer PaaS Compute Options
---

# Lab 09c - Menerapkan Azure Container Apps

## Pengantar lab

Di lab ini, Anda akan mempelajari cara menerapkan dan menyebarkan Azure Container Apps.

Lab ini memerlukan langganan Azure. Tipe langganan Anda dapat memengaruhi ketersediaan fitur di lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkah ditulis menggunakan **US Timur**.

## Perkiraan waktu: 15 menit

## Skenario lab

Organisasi Anda memiliki aplikasi web yang berjalan pada mesin virtual di pusat data lokal Anda. Organisasi ingin memindahkan semua aplikasi ke cloud tetapi tidak ingin mengelola jumlah server yang besar. Anda memutuskan untuk mengevaluasi Azure Container Apps.

## Simulasi lab interaktif

Tidak ada simulasi lab interaktif untuk topik ini. 

## Diagram arsitektur

![Diagram tugas.](../media/az104-lab09b-aca-architecture.png)

## Keterampilan pekerjaan

- Tugas 1: Membuat dan mengonfigurasi Aplikasi Kontainer dan lingkungan Azure.
- Tugas 2: Menguji dan memverifikasi penyebaran Aplikasi Kontainer Azure.

## Tugas 1: Membuat dan mengonfigurasi Aplikasi Kontainer dan lingkungan Azure

Azure Container Apps mengambil konsep kluster Kubernetes terkelola selangkah lebih jauh dan mengelola lingkungan kluster sekaligus menyediakan layanan terkelola lainnya di samping kluster. Berbeda dari kluster Azure Kubernetes, yang masih mengharuskan Anda mengelola kluster, instans Azure Container Apps menghilangkan sebagian kompleksitas untuk menyiapkan kluster Kubernetes.

1. Dari portal Azure, telusuri dan pilih `Container Apps`.

1. Pilih **+ Buat**, dari menu drop-down, **Aplikasi** Kontainer. Perhatikan pilihan yang lain. 

1. Gunakan informasi berikut untuk mengisi detail di tab **Dasar**.*.

    | Pengaturan | Tindakan |
    |---|---|
    | Langganan | Pilih langganan Azure Anda |
    | Grup sumber daya | `az104-rg9` |
    | Nama aplikasi kontainer |  `my-app` |
    | Wilayah    | **US** Timur (|
    | Lingkungan Container Apps | Pilih **Buat > Baru** Atur Nama lingkungan ke > `my-environment`** Buat** |

1. Klik **Berikutnya: Tab kontainer** dan pastikan bahwa **Gunakan gambar** mulai cepat dicentang. Anda mungkin perlu menggulir ke atas untuk melihat pengaturan ini. 

1. Pastikan **gambar** Mulai Cepat diatur ke **kontainer** Halo dunia sederhana. Perhatikan pilihan yang lain. 

1. Pilih **Tinjau dan buat** lalu **Buat**.

    >**Catatan:** Tunggu hingga aplikasi kontainer disebarkan. Ini akan butuh waktu beberapa menit. 
 
## Tugas 2: Menguji dan memverifikasi penyebaran Aplikasi Kontainer Azure

Secara default, aplikasi kontainer Azure yang Anda buat akan menerima lalu lintas di port 80 menggunakan sampel aplikasi Halo Dunia. Azure Container Apps akan memberikan nama DNS untuk aplikasi tersebut. Salin dan navigasikan ke URL ini untuk memastikan bahwa aplikasi aktif dan berjalan.

1. Pilih **Buka sumber daya** untuk melihat aplikasi kontainer baru Anda.

1. Pilih tautan di samping *URL Aplikasi* untuk melihat aplikasi Anda.

    ![Cuplikan layar halaman gambaran umum ACA di portal.](../media/az104-lab09b-aca-overview.png)

1. Verifikasikan bahwa Anda menerima pesan **aplikasi Azure Container Apps Anda kini aktif**.
   
## Membersihkan sumber daya Anda

Jika Anda bekerja dengan **langganan Anda sendiri** luangkan waktu sebentar untuk menghapus sumber daya lab. Hal ini akan memastikan sumber daya dikosongkan dan biaya diminimalkan. Cara termudah untuk menghapus sumber daya lab adalah dengan menghapus grup sumber daya lab. 

+ Di portal Microsoft Azure, pilih grup sumber daya, pilih **Hapus grup sumber daya**, **Masukkan nama grup sumber daya**, lalu klik **Hapus**.
+ Menggunakan Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Menggunakan CLI, `az group delete --name resourceGroupName`.

## Perluas pemelajaran Anda dengan Copilot
Copilot dapat membantu Anda mempelajari cara menggunakan alat pembuatan skrip Azure. Copilot juga dapat membantu di area yang tidak tercakup dalam lab atau ketika Anda memerlukan informasi lebih lanjut. Buka browser Edge dan pilih Copilot (kanan atas) atau navigasi *copilot.microsoft.com*. Luangkan beberapa menit untuk mencoba perintah ini.

+ Ringkas langkah-langkah untuk membuat dan mengonfigurasi Aplikasi Kontainer Azure.
+ Membandingkan dan membedakan Azure Container Apps dengan Azure Kubernetes Service.

## Pelajari lebih lanjut dengan pelatihan mandiri

+ [Mengonfigurasi aplikasi kontainer di Azure Container Apps](https://learn.microsoft.com/training/modules/configure-container-app-azure-container-apps/). Memeriksa fitur dan kemampuan Azure Container Apps, lalu berfokus pada cara membuat, mengonfigurasi, menskalakan, dan mengelola aplikasi kontainer menggunakan Azure Container Apps.


## Poin penting

Selamat atas penyelesaian lab ini. Berikut adalah kesimpulan utama dari lab ini. 

+ Azure Container Apps (ACA) adalah platform tanpa server yang memungkinkan Anda mengelola lebih sedikit infrastruktur dan menghemat biaya saat menjalankan aplikasi kontainer.
+ Container Apps menyediakan konfigurasi server, orkestrasi kontainer, dan detail penyebaran. 
+ Beban kerja di ACA biasanya merupakan proses yang berjalan lama, misalnya Aplikasi Web.

     
