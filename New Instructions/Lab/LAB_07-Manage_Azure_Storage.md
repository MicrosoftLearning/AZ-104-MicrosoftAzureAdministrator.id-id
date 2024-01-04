---
lab:
  title: 'Lab 07: Mengelola penyimpanan Azure'
  module: Administer Azure Storage
---

# Lab 07 - Mengelola Azure Storage

## Pengenalan lab

Di lab ini, Anda belajar membuat akun penyimpanan untuk blob Azure dan file Azure. Anda belajar mengonfigurasi dan mengamankan kontainer blob. Anda juga belajar menggunakan Browser Penyimpanan untuk mengonfigurasi dan mengamankan berbagi file Azure. 

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur di lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkahnya ditulis menggunakan US Timur.

## Perkiraan waktu: 40 menit

## Skenario lab

Organisasi Anda saat ini menyimpan data di penyimpanan data lokal. Sebagian besar file ini tidak sering diakses. Anda ingin meminimalkan biaya penyimpanan dengan menempatkan file yang jarang diakses di tingkat penyimpanan dengan harga lebih rendah. Selain itu, rencanakan untuk menjelajahi berbagai mekanisme perlindungan yang ditawarkan Azure Storage, termasuk akses jaringan, autentikasi, otorisasi, dan replikasi. Terakhir, Anda ingin menentukan sejauh mana Azure Files cocok untuk menghosting berbagi file lokal Anda.

## Simulasi lab interaktif

Ada simulasi lab interaktif yang mungkin berguna bagi Anda untuk topik ini. Simulasi ini memungkinkan Anda mengklik skenario serupa dengan kecepatan Anda sendiri. Ada perbedaan antara simulasi interaktif dan lab ini, tetapi banyak konsep intinya sama. Langganan Azure tidak diperlukan. 

+ [Buat penyimpanan](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%205) blob. Buat akun penyimpanan, kelola penyimpanan blob, dan pantau aktivitas penyimpanan. 
  
+ [Mengelola penyimpanan](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2011) Azure. Buat akun penyimpanan dan tinjau konfigurasinya. Mengelola kontainer penyimpanan blob. Mengonfigurasi jaringan penyimpanan. 

## Diagram arsitektur

![Diagram tugas.](../media/az104-lab07-architecture-diagram.png)

## Tugas

+ Tugas 1: Membuat dan mengonfigurasi akun penyimpanan. 
+ Tugas 2: Membuat dan mengonfigurasi penyimpanan blob aman.
+ Tugas 3: Membuat dan mengonfigurasi penyimpanan file Azure yang aman.

## Tugas 1: Membuat dan mengonfigurasi akun penyimpanan privat. 

Dalam tugas ini, Anda akan membuat dan mengonfigurasi akun penyimpanan.

1. Masuk ke **portal Azure** - `https://portal.azure.com`.

1. Cari dan pilih **Akun penyimpanan**, lalu klik **+ Buat**.

1. Pada tab **Dasar** dari bilah **Buat akun** penyimpanan, tentukan pengaturan berikut (biarkan orang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan          | nama langganan Azure Anda  |
    | Grup sumber daya        | **az104-rg7** (buat baru) |
    | Nama akun penyimpanan  | nama unik global apa pun yang panjangnya antara 3 dan 24 yang terdiri atas huruf dan angka |
    | Wilayah                | **US Timur**  |
    | Performa           | **Standar** (perhatikan opsi Premium) |
    | Redundansi geografis            | **Penyimpanan geo-redundan** (perhatikan opsi lainnya)|
    | Membuat akses baca ke data jika terjadi ketersediaan regional | Centang kotak |

1. Pada tab **Tingkat Lanjut** , tinjau opsi yang tersedia, terima default.

1. Pada tab **Jaringan** , tinjau opsi yang tersedia, pilih **Nonaktifkan akses publik dan gunakan akses privat.**.

1. Tinjau tab **Perlindungan** data. Pemberitahuan 7 hari adalah kebijakan penyimpanan penghapusan sementara default. Perhatikan bahwa Anda dapat mengaktifkan penerapan versi blob. Terima default.

1. Tinjau tab **Enkripsi** . Perhatikan opsi keamanan tambahan. Terima default.

1. Pilih **Tinjau**, tunggu hingga proses validasi selesai, lalu klik **Buat**.

1. Setelah akun penyimpanan disebarkan, pilih **Buka sumber daya**.

1. Tinjau bilah **Gambaran Umum** dan konfigurasi tambahan yang dapat diubah. Ini adalah pengaturan global untuk akun penyimpanan. Perhatikan bahwa akun penyimpanan dapat digunakan untuk kontainer Blob, Berbagi file, Antrean, dan Tabel.

1. Di bagian **Keamanan + Jaringan** , pilih **Jaringan**. Perhatikan bahwa akses jaringan publik dinonaktifkan.

+ **Ubah tingkat** akses publik menjadi **Diaktifkan dari jaringan virtual dan alamat** IP yang dipilih.
+ Di bagian **Firewall** , centang kotak untuk **Tambahkan alamat IP klien Anda.**
+ Pastikan untuk **Menyimpan** perubahan Anda. 
  
1. Di bagian **Manajemen data** , lihat bilah **Redundansi** . Perhatikan informasi tentang lokasi pusat data primer dan sekunder Anda.

1. Di bagian **Manajemen data** , pilih **Manajemen** siklus hidup, lalu pilih **Tambahkan aturan**.

+ **Beri nama** aturan `Movetocool`. Perhatikan opsi Anda untuk membatasi cakupan aturan.

+ Pada tab **Blob** dasar, *jika* blob berbasis terakhir dimodifikasi lebih dari `30 days` yang lalu *, pindahkan* **ke penyimpanan** dingin.

+ Perhatikan bahwa Anda dapat mengonfigurasi kondisi lain. Pilih **Tambahkan** saat Anda selesai menjelajahi.

    ![Cuplikan layar berpindah ke kondisi aturan dingin.](../media/az104-lab07-movetocool.png)

## Tugas 2: Mengelola penyimpanan blob

Dalam tugas ini, Anda akan membuat kontainer blob dan mengunggah blob. Kontainer blob adalah struktur seperti direktori yang menyimpan data yang tidak terstruktur.

### Membuat kontainer blob dan kebijakan penyimpanan berbasis waktu

1. Lanjutkan di portal Azure, bekerja dengan akun penyimpanan Anda.

1. Di bagian **Penyimpanan data**, klik **Wadah**. 

1. Klik **+ Kontainer** dan **Buat** kontainer dengan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | `data`  |
    | Tingkat akses publik | Perhatikan tingkat akses diatur ke privat |

    ![Cuplikan layar buat kontainer.](../media/az104-lab07-create-container.png)

1. Pilih kontainer Anda dan di bagian **Pengaturan**, pilih **Kebijakan** Akses.

1. **Di area penyimpanan** blob yang tidak dapat diubah, pilih **Tambahkan kebijakan**.

    | Pengaturan | Nilai |
    | --- | --- |
    | Jenis kebijakan | **Retensi berbasis waktu**  |
    | Mengatur periode retensi untuk | `180` hari |

1. Pilih **Simpan**.

### Mengelola unggahan blob

1. Kembali ke halaman kontainer, pilih kontainer data** Anda **lalu klik **Unggah**.

1. Pada bilah **Unggah blob** , perluas bagian **Tingkat Lanjut** .

   >**Catatan**: Temukan file untuk diunggah. Ini bisa berupa semua jenis file, tetapi file kecil adalah yang terbaik. 

    | Pengaturan | Nilai |
    | --- | --- |
    | Telusuri file | tambahkan file yang telah Anda pilih untuk diunggah |
    | Jenis blob | **Blob blok** |
    | Ukuran blok | **4 MiB** |
    | Tingkat penyimpanan | **Panas**  (perhatikan opsi lainnya) |
    | Unggah ke folder | `securitytest` |
    | Cakupan enkripsi | Gunakan lingkup kontainer default yang ada |

    > **Catatan**: Tingkat akses dapat diatur untuk blob individual.

1. Klik **Unggah**.

1. Konfirmasikan bahwa Anda memiliki folder baru, dan file Anda telah diunggah. 

1. Pilih file unggahan Anda dan tinjau opsi termasuk Unduh, Hapus **, **** Ubah tingkat**, dan **Dapatkan sewa**.****

1. Salin URL** file **dan tempelkan ke jendela penjelajahan Inprivate** baru**.

1. Anda akan diberikan pesan berformat XML yang menyatakan **ResourceNotFound** atau **PublicAccessNotPermitted**.

    > **Catatan**: Ini diharapkan, karena kontainer yang Anda buat memiliki tingkat akses publik yang diatur ke **Privat (tidak ada akses anonim)**.

### Mengonfigurasi akses terbatas ke penyimpanan blob

1. Kembali ke file yang Anda unggah dan pilih tab **Hasilkan SAS** . Tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Kunci penandatanganan | **Kunci 1** |
    | Izin | **Baca** |
    | Tanggal mulai | tanggal kemarin |
    | Waktu mulai | waktu saat ini |
    | Tanggal kedaluwarsa | tanggal besok |
    | Waktu kedaluwarsa | waktu saat ini |
    | Alamat IP yang diizinkan | biarkan kosong |

1. Klik **Hasilkan token SAS dan URL**.

1. Klik tombol **Salin ke papan klip** di sebelah entri **Blob SAS URL**.

1. Buka jendela browser lain dengan menggunakan mode InPrivate dan arahkan ke URL yang Anda salin di langkah sebelumnya.

    > **Catatan**: Anda harus dapat melihat konten file dengan mengunduhnya dan membukanya dengan Notepad. Jika Anda menerima kesalahan Windows SmartScreen, lanjutkan ke halaman.

## Tugas 5: Membuat dan mengonfigurasi berbagi File Azure

Dalam tugas ini, Anda akan membuat dan mengonfigurasi pembagian Azure Files. 

### Membuat berbagi file dan mengunggah file

1. Di portal Azure, navigasi kembali ke akun penyimpanan Anda, di bagian **Penyimpanan data**, klik **Berbagi** file.

1. Klik **+ Berbagi** file dan pada tab **Dasar** memberi nama berbagi file, `share1`. Tinjau pengaturan lain pada tab ini. 

1. **Perhatikan opsi Tingkat**. Tetap optimalkan** Transaksi default**.
   
1. Pindah ke tab **Cadangan** dan pastikan **Aktifkan Pencadangan** tidak** dicentang**. Kami memutar cadangan untuk menyederhanakan konfigurasi lab.

1. Klik **Tinjau + buat**, lalu **Buat**. Tunggu hingga berbagi file disebarkan.

    ![Cuplikan layar halaman buat berbagi file.](../media/az104-lab07-create-share.png)

### Menjelajahi Browser Penyimpanan dan mengunggah file

1. Kembali ke akun penyimpanan Anda dan pilih **Browser** Penyimpanan. Browser Azure Storage adalah alat portal yang memungkinkan Anda melihat semua layanan penyimpanan dengan cepat di bawah akun Anda.

1. Pilih **Berbagi** file dan verifikasi bahwa direktori share1** Anda **ada. Perhatikan bahwa Anda dapat **+ Menambahkan direktori**.

1. Pilih direktori share1** Anda **dan perhatikan bahwa Anda dapat **+ Tambahkan direktori**. Ini memungkinkan Anda membuat struktur folder.

1. **Unggah** file yang Anda pilih.

1. Pilih **Unggah**. Telusuri ke file pilihan Anda, lalu klik **Unggah**.

    >**Catatan**: Anda dapat melihat berbagi file dan mengelola berbagi tersebut di Browser Penyimpanan. Saat ini tidak ada batasan.

### Membatasi akses jaringan ke akun penyimpanan

1. Di portal, cari dan pilih **Jaringan virtual**.

1. Pilih **+ Buat.** Pilih grup sumber daya Anda. dan beri nama** jaringan **virtual, `vnet1`.

1. Ambil default untuk parameter lain, pilih **Tinjau + buat**, lalu **Buat**.

1. Tunggu hingga sumber daya disebarkan, lalu pilih **Buka sumber daya**.

1. Di bagian **Pengaturan**, pilih bilah **Subnet**.
    + Pilih subnet **default**.
    + Di bagian **Titik** akhir layanan pilih **Microsoft.Storage** di **menu drop-down Layanan** .
    + Jangan membuat perubahan lain.    
    + Pastikan untuk **Menyimpan** perubahan Anda. 

1. Kembali ke akun penyimpanan Anda.

1. Di bagian **Keamanan + jaringan** , pilih bilah **Jaringan** .

1. Pilih tambahkan jaringan virtual yang ada dan pilih **vnet1** dan **subnet default** , pilih **Tambahkan**.

1. Di bagian **Firewall** , **Hapus** alamat IP komputer Anda. Lalu lintas yang diizinkan hanya boleh berasal dari jaringan virtual. 

1. Pastikan untuk **Menyimpan** perubahan Anda.

    >**Catatan:** Akun penyimpanan sekarang hanya boleh diakses dari jaringan virtual yang baru saja Anda buat. 

1. **Pilih browser** Penyimpanan dan **Refresh** halaman. Navigasikan ke berbagi file atau konten blob Anda.  

    >**Catatan:** Anda harus menerima pesan *yang tidak berwenang untuk melakukan operasi* ini. Anda tidak tersambung dari jaringan virtual. Mungkin perlu waktu beberapa menit agar ini berlaku.

## Poin penting

Selamat atas penyelesaian lab. Berikut adalah takeaway utama untuk lab ini. 

+ Akun penyimpanan Azure berisi semua objek data Azure Storage Anda: blob, file, antrean, dan tabel. Akun penyimpanan menyediakan ruang nama unik untuk data Microsoft Azure Storage Anda yang dapat diakses dari mana saja di seluruh dunia melalui HTTP atau HTTPS.
+ Penyimpanan Azure menyediakan beberapa model redundansi termasuk Penyimpanan redundan lokal (LRS), Penyimpanan zona redundan (ZRS), dan penyimpanan Geo-redundan (GRS). 
+ Penyimpanan blob Azure memungkinkan Anda menyimpan sejumlah besar data yang tidak terstruktur di platform penyimpanan data Microsoft. Blob adalah singkatan dari Binary Large Object, yang mencakup objek seperti gambar dan file multimedia.
+ Azure file Storage menyediakan penyimpanan bersama untuk data terstruktur. Data dapat diatur dalam folder.
+ Penyimpanan yang tidak dapat diubah menyediakan kemampuan untuk menyimpan data dalam status menulis sekali, membaca banyak (WORM). Kebijakan penyimpanan yang tidak dapat diubah menjadi penahanan berbasis waktu atau hukum.


## Membersihkan sumber daya Anda

Jika Anda bekerja dengan langganan Anda sendiri membutuhkan waktu satu menit untuk menghapus sumber daya lab. Ini akan memastikan sumber daya dibebankan dan biaya diminimalkan. Cara term mudah untuk menghapus sumber daya lab adalah dengan menghapus grup sumber daya lab. 

+ Di portal Azure, pilih grup sumber daya, pilih **Hapus grup** sumber daya, **Masukkan nama** grup sumber daya, lalu klik **Hapus**.

+ Menggunakan Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.

+ Menggunakan CLI, `az group delete --name resourceGroupName`.
