---
lab:
  title: 'Lab 06: Menerapkan Manajemen Lalu Lintas'
  module: Administer Network Traffic Management
---

# Lab 06 - Menerapkan Manajemen Lalu Lintas

## Pengenalan lab

Di lab ini, Anda mempelajari cara mengonfigurasi dan menguji Load Balancer publik dan Application Gateway. 

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur di lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkahnya ditulis menggunakan US Timur.

## Perkiraan waktu: 40 menit

## Skenario lab

Organisasi Anda memiliki situs web publik. Anda perlu menyeimbangkan beban permintaan publik yang masuk di berbagai komputer virtual. Anda juga perlu menyediakan gambar dan video dari komputer virtual yang berbeda. Anda berencana menerapkan dan Azure Load Balancer dan Azure Application Gateway. Semua sumber daya berada di wilayah yang sama. 

## Simulasi lab interaktif

Ada simulasi lab interaktif yang mungkin berguna bagi Anda untuk topik ini. Simulasi ini memungkinkan Anda mengklik skenario serupa dengan kecepatan Anda sendiri. Ada perbedaan antara simulasi interaktif dan lab ini, tetapi banyak konsep intinya sama. Langganan Azure tidak diperlukan.

+ [Membuat dan mengonfigurasi dan Azure load balancer](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Create%20and%20configure%20an%20Azure%20load%20balancer). Buat jaringan virtual, server backend, load balancer, lalu uji load balancer. 
+ [Menyebarkan Azure Application Gateway](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Deploy%20Azure%20Application%20Gateway). Buat gateway aplikasi, buat komputer virtual, buat kumpulan backend, dan uji gateway. 
+ [Menerapkan manajemen](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2010) lalu lintas. Terapkan jaringan hub dan spoke lengkap termasuk komputer virtual, jaringan virtual, peering, load balancer, dan gateway aplikasi.

## Tugas

+ Tugas 1: Memprovisikan lingkungan lab
+ Tugas 2: Menerapkan Azure Load Balancer
+ Tugas 3: Menerapkan Azure Application Gateway



## Tugas 1: Memprovisikan lingkungan lab

Dalam tugas ini, Anda akan menggunakan templat untuk menyebarkan satu jaringan virtual, satu grup keamanan jaringan, dan dua komputer virtual bersama dengan kartu antarmuka jaringan virtual terkait. VM akan berada di jaringan virtual hub bernama **az104-vnet1**.

1. Jika perlu, unduh **\\file lab Allfiles\\Lab06\\az104-06-vms-loop-template.json** . 

1. Masuk ke **portal Azure** - `https://portal.azure.com`.

1. Cari dan pilih `Deploy a custom template`.

1. Pada halaman penyebaran kustom, pilih **Bangun templat Anda sendiri di editor**.

1. Pada halaman edit templat, pilih **Muat file**.

1. Temukan dan pilih **\\file Allfiles\\Lab06\\az104-06-vms-loop-template.json** dan pilih **Buka**.

   >**Catatan:** Perhatikan bahwa file ini menyertakan Parameter di bagian atas file. Jadi, file Parameter terpisah tidak diperlukan. 

1. Pilih **Simpan**.

1. Gunakan informasi berikut untuk menyelesaikan bidang pada halaman penyebaran kustom, meninggalkan semua bidang lain dengan nilai default.

    | Pengaturan       | Nilai         | 
    | ---           | ---           |
    | Langganan  | Langganan Azure Anda |
    | Grup sumber daya| `az104-rg6` (Jika perlu, pilih **Buat baru**)
    | Wilayah        | **US Timur**   |
    | Ukuran Komputer Virtual       | **Standar DS2 v3** |
    | Nama Pengguna Admin| `localadmin` |
    | Kata sandi      | Berikan kata sandi yang aman |

     >**Catatan**: Jika Anda menerima kesalahan bahwa ukuran VM tidak tersedia, pilih SKU yang tersedia di langganan Anda dan memiliki setidaknya 2 core.

1. Pilih **Ulas + buat**, lalu pilih **Buat**.

    >**Catatan**: Tunggu hingga penyebaran selesai sebelum pindah ke tugas berikutnya. Penyebaran harus selesai dalam waktu sekitar 5 menit.
    
    >**Catatan:** Saat Anda menunggu, cari dan pilih **Network Watcher**. Pilih bilah **Topologi** untuk mendapatkan tampilan infrastruktur jaringan virtual. Arahkan mouse ke atas jaringan untuk melihat subnet dan informasi alamat IP. 

## Tugas 2: Menerapkan Azure Load Balancer

Dalam tugas ini, Anda akan menerapkan Azure Load Balancer di depan dua mesin virtual Azure di jaringan virtual hub. Load Balancer di Azure menyediakan konektivitas lapisan 4 di seluruh sumber daya, seperti komputer virtual. Konfigurasi Load Balancer mencakup alamat IP front-end untuk menerima koneksi, kumpulan backend, dan aturan yang menentukan bagaimana koneksi harus melintasi load balancer.


## Diagram arsitektur - Load Balancer

>**Catatan**: Perhatikan bahwa Load Balancer mendistribusikan di dua komputer virtual dalam jaringan virtual yang sama. 


![Diagram tugas lab.](../media/az104-lab06lb-architecture-diagram.png)


1. Di portal Azure, cari dan pilih `Load balancers` dan, pada bilah **Load balancer**, klik **+ Buat**.

1. Buat load balancer dengan pengaturan berikut (biarkan orang lain dengan nilai defaultnya) lalu klik **Berikutnya : Konfigurasi** IP frontend:

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure Anda |
    | Grup sumber daya | **az104-rg6** |
    | Nama | `az104-lb` |
    | Wilayah | Wilayah **yang sama dengan** yang Anda sebarkan VM |
    | SKU  | **Standard**
           |
    | Jenis | **Publik** |
    | Tingkat | **Regional** |
    
     ![Cuplikan layar halaman buat load balancer.](../media/az104-lab06-create-lb1.png)

1. Pada tab **Konfigurasi** IP Frontend, klik **Tambahkan konfigurasi** IP frontend dan gunakan pengaturan berikut:  
     
    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | `az104-fe` |
    | Jenis IP | Alamat IP |
    | Alamat IP publik | Pilih **Create new**|
    | Penyeimbang Beban Gateway | Tidak |
    
1. **Pada popup Tambahkan alamat** IP publik, gunakan pengaturan berikut sebelum mengklik **OK** lalu **Tambahkan**. Setelah selesai klik **Berikutnya: Kumpulan backend**. 
     
    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | `az104-pip` |
    | SKU | Standard |
    | Tingkat | Wilayah |
    | Penugasan | Statis |
    | Preferensi Perutean | **Jaringan Microsoft** |

1. Pada tab **Kumpulan backend**, klik **Tambahkan kumpulan backend** dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya). Klik **+ Tambahkan** (dua kali) lalu klik  **Berikutnya: Aturan** masuk. 

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | `az104-be` |
    | Jaringan virtual | **az104-06-vnet1** |
    | Konfigurasi Kumpulan Backend | **NIC** | 
    | Versi IP | **IPv4** |
    | Klik **Tambahkan** untuk menambahkan mesin virtual |  |
    | az104-vm0 | **centang kotak** |
    | az104-vm1 | **centang kotak** |

1. Pada tab **Aturan masuk**, klik **Tambahkan aturan penyeimbangan beban**. Tambahkan aturan penyeimbangan beban dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya). Setelah selesai klik **Tambahkan**.

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | `az104-lbrule` |
    | Versi IP | **IPv4** |
    | Alamat IP Frontend | **az104-fe** |
    | Kumpulan backend | **az104-be** |
    | Protokol | **TCP** |
    | Port | `80` |
    | Port ujung belakang | `80` |
    | Pemeriksaan kesehatan | **Buat baru** |
    | Nama | `az104-hp` |
    | Protokol | **TCP** |
    | Port | `80` |
    | Interval | `5` |
    | Tutup jendela buat pemeriksaan kesehatan | **Simpan** | 
    | Kegigihan sesi | **Tidak** |
    | Waktu idle habis (menit) | `4` |
    | Reset TCP | **Nonaktif** |
    | IP Float | **Nonaktif** |
    | Terjemahan alamat jaringan sumber keluar (SNAT) | **Direkomendasikan** |

1. Saat luang, tinjau tab lainnya, lalu klik **Tinjau dan buat**. Pastikan tidak ada kesalahan validasi, lalu klik **Buat**. 

1. Tunggu hingga penyeimbang beban diterapkan, lalu klik **Buka sumber daya**.  

1. Pilih **Konfigurasi IP frontend** dari halaman sumber daya Load Balancer. Salin alamat IP publik.

1. Buka tab browser lain dan arahkan ke alamat IP. Pastikan jendela browser menampilkan pesan **Halo Dunia dari az104-06-vm0** atau **Halo Dunia dari az104-06-vm1**.

1. Refresh jendela untuk memverifikasi perubahan pesan ke mesin virtual lainnya. Hal ini menunjukkan load balancer berputar melalui mesin virtual.

    > **Catatan**: Anda mungkin perlu merefresh lebih dari sekali atau membuka jendela browser baru dalam mode InPrivate.

### Menguji koneksi antara vm0 dan vm1 

1. Dari portal Azure, cari dan pilih `Network Watcher`.

1. Dari Network Watcher, di menu Alat diagnostik jaringan, pilih **pemecahan masalah** Koneksi ion.

1. Gunakan informasi berikut untuk menyelesaikan bidang di **halaman pemecahan masalah** Koneksi ion.

    | Bidang | Nilai | 
    | --- | --- |
    | Jenis sumber           | **Mesin virtual**   |
    | Komputer virtual       | **vm0**    | 
    | Tipe tujuan      | **Mesin virtual**   |
    | Komputer virtual       | **vm1**   | 
    | Versi IP Pilihan  | **Kedua**              | 
    | Protokol              | **TCP**               |
    | Port tujuan      | `3389`                |  
    | Port Sumber           | *Kosong*         |
    | Tes diagnostik      | *Default*      |

    ![Portal Microsoft Azure memperlihatkan pengaturan Pemecahan Masalah Koneksi ion.](../media/az104-lab06-connection-troubleshoot.png)

1. Pilih **Jalankan pengujian** diagnostik.

    >**Catatan**: Mungkin perlu waktu beberapa menit agar hasilnya kembali. Pilihan layar akan berwarna abu-abu saat hasilnya sedang dikumpulkan. Perhatikan bahwa **pengujian Koneksi ivity menunjukkan Dapat Dijangkau****.** Ini masuk akal karena komputer virtual berada dalam jaringan virtual yang sama. 

## Tugas 3: Menerapkan Azure Application Gateway

Dalam tugas ini, Anda akan menerapkan Azure Application Gateway di depan dua komputer virtual Azure. Application Gateway menyediakan penyeimbangan beban lapisan 7, Web Application Firewall (WAF), penghentian SSL, dan enkripsi end-to-end ke sumber daya yang ditentukan dalam kumpulan backend. Application Gateway akan merutekan gambar ke satu komputer virtual dan video ke komputer virtual lainnya. 

## Diagram arsitektur - Application Gateway

>**Catatan**: Application Gateway ini berfungsi di jaringan virtual yang sama dengan Load Balancer di tugas sebelumnya. Ini tidak khas di lingkungan produksi.

![Diagram tugas lab.](../media/az104-lab06gw-architecture-diagram.png)

1. Di portal Azure, cari dan pilih `Virtual networks`.

1. Pada bilah **Jaringan** virtual, dalam daftar jaringan virtual, klik **az104-vnet1**.

1. Pada bilah **jaringan virtual az104-vnet1**, di bagian **Pengaturan**, klik **Subnet**, lalu klik **+ Subnet**.

1. Tambahkan subnet dengan pengaturan berikut (biarkan orang lain dengan nilai defaultnya).

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | `subnet-appgw` |
    | Rentang alamat subnet | `10.60.3.224/27` |

1. Klik **Simpan**

    > **Catatan**: Subnet ini akan digunakan oleh instans Azure Application Gateway. Application Gateway memerlukan subnet khusus dengan ukuran /27 atau lebih besar. 

1. Di portal Azure, cari dan pilih `Application Gateways` dan, pada bilah **Application Gateways**, klik **+ Buat**.

1. Pada tab **Dasar**, tentukan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | `az104-rg6` |
    | Nama gateway aplikasi | `az104-appgw` |
    | Wilayah | Wilayah **Azure yang sama dengan** yang Anda gunakan di Tugas 1 |
    | Tingkat | **Standard V2** |
    | Aktifkan penskalaan otomatis | **Tidak** |
    | Jumlah Instans | `2` |
    | Zona ketersediaan | **Tidak** |
    | HTTP2 | **Nonaktif** |
    | Jaringan virtual | **az104-06-vnet1** |
    | Subnet | **subnet-appgw (10.60.3.224/27)** |

    ![Cuplikan layar halaman buat gateway aplikasi.](../media/az104-lab06-create-appgw.png)

1. Klik **Berikutnya: Frontend >** dan tentukan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya). Setelah selesai, klik **OKE**. 

    | Pengaturan | Nilai |
    | --- | --- |
    | Jenis alamat IP frontend | **Publik** |
    | Alamat IP publik| **Tambah **yang baru | 
    | Nama | `az104-gwpip` |
    | Zona ketersediaan | **Tidak** |

1. Klik **Berikutnya: Backend >** lalu **Tambahkan kumpulan backend**. Ini adalah kumpulan backend untuk **gambar**. Tentukan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya). Setelah selesai klik **Tambahkan**.

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | `az104-appgwbe` |
    | Menambahkan kumpulan backend tanpa target | **Tidak** |
    | Komputer virtual | **az104-rg6-nic1 (10.60.1.4)** |
    | Komputer virtual | **az104-rg6-nic2 (10.60.2.4)** | 

1. Klik **Berikutnya: Konfigurasi >** lalu **+ Tambahkan aturan perutean**. Tentukan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama aturan | `az104-gwrule` |
    | Prioritas | `10` |
    | Nama listener | `az104-listener` |
    | IP Frontend | **Publik** |
    | Protokol | **HTTP** |
    | Port | `80` |
    | Tipe listener | **Dasar** |

    ![Cuplikan layar halaman buat aturan gateway aplikasi.](../media/az104-lab06-appgw-rule.png)

1. Beralih ke tab **Target backend** dan tentukan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya). 

    | Pengaturan | Nilai |
    | --- | --- |
    | Jenis target | **Kumpulan backend** |
    | Target ujung belakang | **az104-appgwbe** |
    | Pengaturan backend | **Tambah **yang baru |
    | Nama pengaturan backend | `az104-http` |
    | Protokol backend | **HTTP** |
    | Port ujung belakang | `80` |
    | Pengaturan tambahan | **biarkan default** |
    | Nama Host | **biarkan default** |

   >**Catatan:** Gunakan ikon informasi untuk mempelajari selengkapnya tentang **pengurasan afinitas** dan **Koneksi ion** berbasis Cookie. 

1. Pilih **Tambahkan beberapa target untuk membuat aturan** berbasis jalur. Anda akan membuat dua aturan.

    **Aturan - perutean ke backend gambar**

    | Pengaturan | Nilai |
    | --- | --- |
    | Jalur | `images/*` |
    | Nama target | `images` |
    | Pengaturan backend | **appgw-settings** |
    | Target ujung belakang | `az104-appgw-images` |

    **Aturan - perutean ke backend video**

    | Pengaturan | Nilai |
    | --- | --- |
    | Jalur | `video/*` |
    | Nama target | `videos` |
    | Pengaturan backend | **appgw-settings** |
    | Target ujung belakang | `az104-appgw-videos` |

1. Pilih **Tambahkan** dua kali lalu pilih **Berikutnya: Tag >**.

1. Pilih **Berikutnya: Tinjau + buat >** lalu klik **Buat**.

    > **Catatan**: Tunggu hingga instans Application Gateway dibuat. Ini akan memakan waktu sekitar 5-10 menit.

1. Di portal Azure, cari dan pilih **az104-appgw**.

1. **Di Application Gateway** pilih **Kesehatan** backend.

1. Pastikan kedua server di kumpulan backend menampilkan **Sehat**. 

1. Pada bilah **Application Gateway az104-appgw** , salin nilai **alamat** IP publik Frontend.

1. Mulai jendela browser lain dan uji URL - `http://<frontend ip address>/image/`.

1. Verifikasi bahwa Anda diarahkan ke server gambar (vm1). 

1. Mulai jendela browser lain dan uji URL - `http://<frontend ip address>/video/`.

1. Verifikasi bahwa Anda diarahkan ke server gambar (vm2). 

> **Catatan**: Anda mungkin perlu merefresh lebih dari sekali atau membuka jendela browser baru dalam mode InPrivate.

## Poin penting

Selamat atas penyelesaian lab. Berikut adalah takeaway utama untuk lab ini. 

+ Azure Load Balancer adalah pilihan yang sangat baik untuk mendistribusikan lalu lintas jaringan di beberapa komputer virtual di lapisan transportasi (lapisan OSI 4 - TCP dan UDP).
+ Load Balancer Publik digunakan untuk menyeimbangkan beban lalu lintas internet ke VM Anda. Penyeimbang beban internal (atau privat) digunakan jika IP privat hanya diperlukan di frontend.
+ Load balancer Dasar adalah untuk aplikasi skala kecil yang tidak memerlukan ketersediaan tinggi atau redundansi. Load balancer Standar adalah untuk performa tinggi dan latensi sangat rendah.
+ Azure Application Gateway adalah penyeimbang beban lalu lintas web (OSI lapisan 7) yang memungkinkan Anda mengelola lalu lintas ke aplikasi web Anda. 
+ Application Gateway dapat membuat keputusan perutean berdasarkan atribut tambahan permintaan HTTP, misalnya jalur URI atau header host. 

## Membersihkan sumber daya Anda

Jika Anda bekerja dengan langganan Anda sendiri membutuhkan waktu satu menit untuk menghapus sumber daya lab. Ini akan memastikan sumber daya dibebankan dan biaya diminimalkan. Cara term mudah untuk menghapus sumber daya lab adalah dengan menghapus grup sumber daya lab. 

+ Di portal Azure, pilih grup sumber daya, pilih **Hapus grup** sumber daya, **Masukkan nama** grup sumber daya, lalu klik **Hapus**.
+ Menggunakan Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Menggunakan CLI, `az group delete --name resourceGroupName`.
