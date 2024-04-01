---
lab:
  title: 'Lab 09c: Menerapkan Azure Container Apps'
  module: Administer PaaS Compute Options
---

# Lab 09c - Menerapkan Azure Container Apps

## Pengenalan lab

Di lab ini, Anda mempelajari cara menerapkan dan menyebarkan Azure Container Apps.

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur di lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkahnya ditulis menggunakan **US** Timur.

## Perkiraan waktu: 15 menit

## Skenario lab

Organisasi Anda memiliki aplikasi web yang berjalan pada komputer virtual di pusat data lokal Anda. Organisasi ingin memindahkan semua aplikasi ke cloud tetapi tidak ingin memiliki sejumlah besar server untuk dikelola. Anda memutuskan untuk mengevaluasi Azure Container Apps.

## Simulasi lab interaktif

Tidak ada simulasi lab interaktif untuk topik ini. 

## Diagram arsitektur

![Diagram tugas.](../media/az104-lab09b-aca-architecture.png)

## Keterampilan pekerjaan

- Tugas 1: Membuat dan mengonfigurasi Azure Container App dan lingkungan.
- Tugas 2: Menguji dan memverifikasi penyebaran Aplikasi Kontainer Azure.

## Tugas 1: Membuat dan mengonfigurasi Azure Container App dan lingkungan

Azure Container Apps mengambil konsep kluster Kubernetes terkelola selangkah lebih jauh dan mengelola lingkungan kluster serta menyediakan layanan terkelola lainnya di atas kluster. Tidak seperti kluster Azure Kubernetes, di mana Anda masih harus mengelola kluster, instans Azure Container Apps menghapus beberapa kompleksitas untuk menyiapkan kluster Kubernetes.

1. Dari portal Azure, cari dan pilih `Container Apps`.

1. Dari **Aplikasi** Kontainer, pilih **Buat**.

1. Gunakan informasi berikut untuk mengisi detail pada tab **Dasar** .*.

    | Pengaturan | Tindakan |
    |---|---|
    | Langganan | Pilih langganan Azure Anda |
    | Grup sumber daya | `az104-rg9` |
    | Nama aplikasi kontainer |  `my-app` |
    | Wilayah    | **US** Timur (Atau wilayah yang tersedia di dekat Anda) |
    | Lingkungan Aplikasi Kontainer | Biarkan default |

1. Pada tab **Kontainer** , pastikan bahwa **Gunakan gambar** mulai cepat diaktifkan dan gambar mulai cepat diatur ke **kontainer** Halo dunia sederhana.

1. Pilih **Tinjau dan buat** lalu **Buat**.

    >**Catatan:** Tunggu hingga aplikasi kontainer disebarkan. Ini akan butuh waktu beberapa menit. 
 
## Tugas 2: Menguji dan memverifikasi penyebaran Aplikasi Kontainer Azure

Secara default, aplikasi kontainer Azure yang Anda buat akan menerima lalu lintas pada port 80 menggunakan sampel Halo Dunia aplikasi. Azure Container Apps akan memberikan nama DNS untuk aplikasi tersebut. Salin dan navigasikan ke URL ini untuk memastikan bahwa aplikasi aktif dan berjalan.

1. Pilih **Buka sumber daya** untuk melihat aplikasi kontainer baru Anda.

1. Pilih tautan di samping *URL Aplikasi* untuk melihat aplikasi Anda.

    ![Cuplikan layar halaman gambaran umum ACA di portal.](../media/az104-lab09b-aca-overview.png)

1. Verifikasi bahwa Anda menerima **aplikasi Azure Container Apps Anda adalah pesan langsung** .
   
## Membersihkan sumber daya Anda

Jika Anda bekerja dengan **langganan** Anda sendiri membutuhkan waktu satu menit untuk menghapus sumber daya lab. Ini akan memastikan sumber daya dibebankan dan biaya diminimalkan. Cara term mudah untuk menghapus sumber daya lab adalah dengan menghapus grup sumber daya lab. 

+ Di portal Azure, pilih grup sumber daya, pilih **Hapus grup** sumber daya, **Masukkan nama** grup sumber daya, lalu klik **Hapus**.
+ Menggunakan Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Menggunakan CLI, `az group delete --name resourceGroupName`.



## Poin penting

Selamat atas penyelesaian lab. Berikut adalah takeaway utama untuk lab ini. 

+ Azure Container Apps (ACA) adalah platform tanpa server yang memungkinkan Anda mempertahankan lebih sedikit infrastruktur dan menghemat biaya saat menjalankan aplikasi kontainer.
+ Container Apps menyediakan konfigurasi server, orkestrasi kontainer, dan detail penyebaran. 
+ Beban kerja di ACA biasanya merupakan proses yang berjalan lama seperti Aplikasi Web.

## Pelajari lebih lanjut dengan pelatihan mandiri

+ [Mengonfigurasi aplikasi kontainer di Azure Container Apps](https://learn.microsoft.com/training/modules/configure-container-app-azure-container-apps/). Memeriksa fitur dan kemampuan Azure Container Apps, lalu berfokus pada cara membuat, mengonfigurasi, menskalakan, dan mengelola aplikasi kontainer menggunakan Azure Container Apps.
     
