---
lab:
  title: 'Lab 01: Mengelola Identitas Microsoft Entra ID'
  module: Administer Identity
---

# Lab 01 - Mengelola Identitas Microsoft Entra ID

## Pengantar lab

Ini adalah yang pertama dalam serangkaian lab untuk Administrator Azure. Di lab ini, Anda mempelajari tentang pengguna dan grup. Pengguna dan grup adalah blok penyusun dasar untuk solusi identitas. 

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur di lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkahnya ditulis menggunakan **US Timur**. 


## Perkiraan waktu: 30 menit

## Skenario lab

Organisasi Anda sedang membangun lingkungan lab baru untuk pengujian pra-produksi aplikasi dan layanan.  Beberapa teknisi sedang dipekerjakan untuk mengelola lingkungan lab, termasuk komputer virtual. Untuk memungkinkan teknisi mengautentikasi dengan menggunakan Microsoft Entra ID, Anda telah ditugaskan untuk menyediakan pengguna dan grup. Untuk meminimalkan overhead administratif, keanggotaan grup harus diperbarui secara otomatis berdasarkan jabatan. 

## Simulasi lab interaktif

Lab ini menggunakan simulasi lab interaktif. Simulasi ini memungkinkan Anda mengklik skenario serupa dengan kecepatan Anda sendiri. Ada perbedaan antara simulasi interaktif dan lab ini, tetapi banyak konsep intinya sama. Langganan Azure tidak diperlukan.

>**Catatan:** Simulasi ini sedang diperbarui. Microsoft Entra ID adalah nama baru untuk Azure Active Directory (Azure AD). 

+ [Mengelola Identitas Entra ID](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%201). Membuat dan mengonfigurasi pengguna dan menetapkan ke grup. Buat penyewa Azure dan kelola akun tamu. 

## Diagram arsitektur
![Diagram arsitektur lab 01.](../media/az104-lab01-architecture.png)

## Keterampilan pekerjaan

+ Tugas 1: Membuat dan mengonfigurasi akun pengguna.
+ Tugas 2: Membuat grup dan menambahkan anggota.

## Tugas 1: Membuat dan mengonfigurasi akun pengguna

Dalam tugas ini, Anda akan membuat dan mengonfigurasi pengguna Azure AD. Akun pengguna akan menyimpan data pengguna seperti nama, departemen, lokasi, dan informasi kontak.

1. Masuk ke **portal Azure** - `https://portal.azure.com`.

    >**Catatan:** Portal Azure digunakan di semua lab. Jika Anda baru menggunakan Azure, cari dan pilih `Quickstart Center`. Luangkan beberapa menit untuk menonton video **Memulai di portal Azure**. Bahkan jika telah menggunakan portal sebelumnya, Anda akan menemukan beberapa tips dan trik tentang menavigasi dan menyesuaikan antarmuka.
    
1. Cari dan pilih `Microsoft Entra ID`. Microsoft Entra ID adalah solusi manajemen identitas dan akses berbasis cloud Azure. Luangkan beberapa menit untuk membiasakan diri dengan beberapa fitur yang tercantum di panel kiri. 

1. Pilih bilah **Gambaran Umum** lalu tab **Kelola penyewa**. 

    >**Tahukah Anda?** Penyewa adalah instans tertentu dari Microsoft Entra ID yang berisi akun dan grup. Bergantung pada situasi Anda, Anda dapat membuat lebih banyak penyewa dan **Beralih** di antara mereka. 

1. Kembali ke halaman **Entra ID** dan pilih **Lisensi**. Dari sini Anda dapat membeli lisensi, mengelola lisensi yang Anda miliki, dan menetapkan lisensi untuk pengguna dan grup. Pilih **Fitur berlisensi** untuk melihat apa yang tersedia.
   
###  Buat pengguna baru 

1. Pilih **Pengguna**, lalu di menu drop-down **Pengguna baru** pilih **Buat pengguna baru**. 

1. Buat pengguna baru dengan pengaturan berikut (biarkan yang lain dengan default). Pada tab **Properti**, perhatikan semua jenis informasi berbeda yang dapat disertakan dalam akun pengguna. 

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama principal pengguna | `az104-user1` |
    | Nama tampilan | `az104-user1` |
    | Buat Kata Sandi secara otomatis | **dicentang** |
    | Akun diaktifkan | **dicentang** |
    | Jabatan (tab Properti) | `IT Lab Administrator` |
    | Departemen (tab Properti) | `IT` |
    | Lokasi penggunaan (tab Properti) | **Amerika Serikat** |

1. Setelah Anda selesai meninjau, pilih **Tinjau + buat** lalu **Buat**.

1. Refresh halaman dan konfirmasikan bahwa pengguna baru Anda telah dibuat. 

### Mengundang pengguna eksternal

1. Di menu drop-down **Pengguna baru** pilih **Undang pengguna eksternal**. 

    | Pengaturan | Nilai |
    | --- | --- |
    | Email | alamat email anda |
    | Nama tampilan | nama Anda |
    | Kirim pesan undangan | **centang kotak** |
    | Pesan | `Welcome to Azure and our group project` |

1. Pindah ke tab **Properti**. Lengkapi informasi dasar, termasuk bidang ini. 

    | Pengaturan | Nilai |
    | --- | --- |
    | Judul pekerjaan  | `IT Lab Administrator` |
    | Departemen  | `IT` |
    | Lokasi penggunaan (tab Properti) | **Amerika Serikat** |

1. Pilih **Tinjau + undang**, lalu **Undang**.

1. **Refresh** halaman dan konfirmasikan bahwa pengguna yang diundang telah dibuat. Anda akan segera menerima email undangan. 

    >**Catatan:** Kecil kemungkinan Anda akan membuat akun pengguna satu per satu. Apakah Anda tahu bagaimana organisasi Anda berencana untuk membuat dan mengelola akun pengguna?
    
## Tugas 2: Membuat grup dan menambahkan anggota

Dalam tugas ini, Anda membuat akun grup. Akun grup dapat menyertakan akun pengguna atau perangkat. Ini adalah dua cara dasar anggota ditetapkan ke grup: Secara Statis dan Dinamis. Grup statis mengharuskan administrator untuk menambahkan dan menghapus anggota secara manual.  Grup dinamis diperbarui secara otomatis berdasarkan properti akun pengguna atau perangkat. Misalnya, jabatan. 

1. Di portal Azure, telusuri dan pilih `Groups`.

1. Luangkan waktu sebentar untuk membiasakan diri Anda dengan pengaturan grup di panel kiri.

   + **Kedaluwarsa** memungkinkan Anda mengonfigurasi masa pakai grup dalam hari. Setelah itu grup harus diperbarui oleh pemiliknya.
   + **Kebijakan penamaan** memungkinkan Anda mengonfigurasi kata yang diblokir dan menambahkan awalan atau akhiran ke nama grup.

1. Di bilah **Semua grup**, pilih **+ Grup baru** dan buat grup baru.     

    | Pengaturan | Nilai |
    | --- | --- |
    | Jenis grup | **Keamanan** |
    | Nama grup | `IT Lab Administrators` |
    | Deskripsi grup | `Administrators that manage the IT lab` |
    | Jenis keanggotaan | **Ditetapkan** |

    >**Catatan**: Lisensi Entra ID Premium P1 atau P2 diperlukan untuk keanggotaan dinamis. Jika **Jenis keanggotaan** lainnya tersedia, opsi akan muncul di menu drop-down. 
    
    ![Cuplikan layar buat grup yang ditetapkan.](../media/az104-lab01-create-assigned-group.png)

1. Pilih **Tidak ada pemilik yang dipilih**.

1. Di halaman **Tambahkan pemilik**, cari dan **pilih** diri Anda sebagai pemilik. Perhatikan bahwa Anda dapat memiliki lebih dari satu pemilik. 

1. Pilih **Tidak ada anggota yang dipilih**.

1. Di panel **Tambahkan anggota**, cari dan **pilih** **az104-user1** dan **pengguna tamu** yang Anda undang. Tambahkan kedua pengguna ke grup. 

1. Pilih **Buat** untuk menyebarkan grup.

1. **Refresh** halaman dan pastikan grup Anda dibuat.

1. Pilih grup baru dan tinjau informasi **Anggota** dan **Pemilik**.

>**Catatan:** Anda mungkin mengelola sejumlah besar grup. Apakah organisasi Anda memiliki rencana untuk membuat grup dan menambahkan anggota?
   
## Membersihkan sumber daya Anda

Jika Anda bekerja dengan **langganan Anda sendiri**, luangkan waktu sebentar untuk menghapus sumber daya lab. Hal ini akan memastikan sumber daya dibebaskan dan biaya diminimalkan. Cara termudah untuk menghapus sumber daya lab adalah dengan menghapus grup sumber daya lab. 

+ Di portal Microsoft Azure, pilih grup sumber daya, pilih **Hapus grup sumber daya**, **Masukkan nama grup sumber daya**, lalu klik **Hapus**.
+ Menggunakan Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Menggunakan CLI, `az group delete --name resourceGroupName`.
  

## Perluas pembelajaran Anda dengan Copilot

Copilot dapat membantu Anda mempelajari cara menggunakan alat pembuatan skrip Azure. Salinan juga dapat membantu di area yang tidak tercakup dalam lab atau di mana Anda memerlukan informasi lebih lanjut. Buka browser Edge dan pilih Copilot (kanan atas) atau navigasi ke *copilot.microsoft.com*. Luangkan beberapa menit untuk mencoba perintah ini.
+ Apa saja perintah Azure PowerShell dan CLI untuk membuat grup keamanan yang disebut Admin TI? Berikan halaman referensi perintah resmi.  
+ Berikan strategi langkah demi langkah untuk mengelola pengguna dan grup di Microsoft Entra ID.
+ Apa saja langkah-langkah di portal Microsoft Azure untuk membuat pengguna dan grup secara massal?
+ Berikan tabel perbandingan akun pengguna Microsoft Entra ID internal dan eksternal. 


## Pelajari lebih lanjut dengan pelatihan mandiri

+ [Memahami Microsoft Entra ID](https://learn.microsoft.com/training/modules/understand-azure-active-directory/). Bandingkan Microsoft Entra ID dengan Active Directory DS, pelajari tentang Microsoft Entra ID P1 dan P2, dan jelajahi Layanan Domain Microsoft Entra untuk mengelola perangkat dan aplikasi yang bergabung dengan domain di cloud.
+ [Buat pengguna dan grup Azure di Microsoft Entra Id](https://learn.microsoft.com//training/modules/create-users-and-groups-in-azure-active-directory/). Buat pengguna di Microsoft Entra ID. Pahami berbagai jenis grup. Buat grup dan tambahkan anggota. Kelola akun tamu bisnis ke bisnis.
+ [Izinkan pengguna menyetel ulang sandi mereka dengan penyetelan ulang sandi mandiri Microsoft Entra](https://learn.microsoft.com/training/modules/allow-users-reset-their-password/). Evaluasi setel ulang kata sandi layanan mandiri untuk memungkinkan pengguna di organisasi Anda menyetel ulang kata sandi mereka atau membuka kunci akun mereka. Siapkan, konfigurasikan, dan uji setel ulang kata sandi layanan mandiri.


## Poin penting

Selamat atas penyelesaian lab. Berikut beberapa hal penting yang dapat diambil dari lab ini:

+ Penyewa mewakili organisasi Anda dan membantu Anda mengelola instans layanan cloud Microsoft tertentu untuk pengguna internal dan eksternal Anda.
+ Microsoft Entra ID memiliki akun pengguna dan tamu. Setiap akun memiliki tingkat akses spesifik terhadap lingkup pekerjaan yang diharapkan dapat dilakukan.
+ Grup menggabungkan pengguna atau perangkat terkait. Ada dua jenis grup termasuk Keamanan dan Microsoft 365.
+ Keanggotaan grup dapat ditetapkan secara statis atau dinamis.
