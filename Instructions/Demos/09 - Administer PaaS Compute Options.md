---
demo:
  title: 'Demonstrasi 09: Mengelola Opsi Komputasi PaaS'
  module: Administer PaaS Compute Options
---

# 09 - Mengelola Opsi Komputasi PaaS

## Mengonfigurasi Paket Azure App Service

Dalam demonstrasi ini, kita akan membuat dan bekerja dengan rencana Azure App Service.

**Referensi**: [Mengelola paket App Service - Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-plan-manage)

**Referensi**: [Meningkatkan skala aplikasi di Azure App Service](https://learn.microsoft.com/azure/app-service/manage-scale-up)

**Referensi**: [Penskalakan otomatis di Azure App Service](https://learn.microsoft.com/azure/app-service/manage-automatic-scaling?tabs=azure-portal)

1. Gunakan portal Azure. 

1. Cari dan pilih **paket** App Service.

1. Buat paket App Service sederhana. Diskusikan kebutuhan untuk memilih Windows atau Linux. Diskusikan paket harga sekarang atau di langkah berikutnya. 

1. Sebarkan paket layanan aplikasi baru Anda. 

1. Tinjau bilah **Skalakan (Paket App Service).** Diskusikan perbedaan antara **paket Dev/Test** dan **Production** . Tinjau daftar fitur. 

1. Tinjau bilah **Peluasan skala (Paket App Service).** Tinjau perbedaan antara **Manual** dan **Berbasis** aturan. 

## Mengonfigurasi Azure App Services

Dalam demonstrasi ini, kami akan membuat aplikasi web baru yang menjalankan kontainer Docker. Kontainer menampilkan pesan Selamat Datang.

**Referensi**: [Membuat Aplikasi Web](https://learn.microsoft.com/training/modules/host-a-web-app-with-azure-app-service/3-exercise-create-a-web-app-in-the-azure-portal?pivots=csharp)

Dalam tugas ini, kita akan membuat Aplikasi Web Azure App Service.

1. Gunakan portal Azure. 

1. Cari dan pilih **App Services**.

1. **Membuat** **Aplikasi** Web.

    - Terbitkan: **Kode**. Tinjau pilihan lain.
    - Tumpukan runtime: **.Net**. Tinjau pilihan lain.
    - Sistem operasi: **Linux**

1. Pilih paket **layanan F1** Gratis.

1. **Tinjau + buat** aplikasi web. Tunggu hingga sumber daya disebarkan.

1. Pada halaman **Gambaran Umum** , pastikan **Status** **Berjalan**.

1. **Pilih URL** dan pastikan halaman tempat penampung default dimuat.

1. Saat Anda punya waktu, jelajahi **opsi Slot** penyebaran.
   
## Mengonfigurasi Azure Container Instances

Dalam demonstrasi ini kami membuat, mengonfigurasi, dan menyebarkan kontainer dengan menggunakan Azure Container Instances (ACI) dari Portal Microsoft Azure. Aplikasi ACI menampilkan halaman HTML statis dengan gambar microsoft Halo Dunia publik. 

**Referensi**: [Mulai Cepat - Menyebarkan kontainer Docker ke instans kontainer](https://learn.microsoft.com/en-us/azure/container-instances/container-instances-quickstart-portal)

1. Gunakan portal Azure.

1. Cari dan pilih **Instans** kontainer.

1. **Buat** instans kontainer baru. 

1. **Isi grup** Sumber Daya dan **Nama** kontainer. 

1. Diskusikan **opsi Sumber gambar** . Gunakan **gambar** Mulai Cepat.

1. Untuk **Gambar** kontainer gunakan **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)**. Sampel gambar Linux ini mengemas aplikasi web kecil yang ditulis di Node.js yang menyajikan halaman HTML statis.

1. Pada halaman **Jaringan**, tentukan **Label nama DNS** untuk kontainer Anda. 

1. Biarkan pengaturan lain sebagai default, lalu pilih **Tinjau + buat**.

1. Tunggu hingga sumber daya disebarkan.

1. Pada halaman **Gambaran Umum** untuk sumber daya, pastikan **Status** **Berjalan**.

1. Navigasikan ke **FQDN** untuk instans kontainer dan pastikan halaman selamat datang ditampilkan. 

**Catatan**: Untuk menghindari biaya tambahan, hapus sumber daya. 

## Mengonfigurasi Azure Container Apps

Dalam demonstrasi ini, kita akan membuat dan bekerja dengan Azure Container Apps. 

**Referensi**: [Mulai cepat: Menyebarkan aplikasi kontainer pertama Anda menggunakan portal Azure](https://learn.microsoft.com/azure/container-apps/quickstart-portal)

1. Cari dan pilih **Aplikasi** Kontainer.

1. Selesaikan **detail** Proyek dan buat lingkungan** aplikasi **kontainer.

1. **Tinjau dan buat** aplikasi kontainer.

1. **Gunakan tautan URL** Aplikasi untuk melihat aplikasi Anda.

1. Verifikasi browser menampilkan pesan Selamat Datang di **Azure Container Apps** . 






