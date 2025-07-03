---
lab:
  title: 'Lab 06: Menerapkan Manajemen Lalu Lintas Jaringan'
  module: Administer Network Traffic Management
---

# Lab 06 - Menerapkan Manajemen Lalu Lintas Jaringan

## Pengenalan lab

Di lab ini, Anda mempelajari cara untuk mengonfigurasi dan menguji Load Balancer publik dan Application Gateway.

Lab ini memerlukan langganan Azure. Tipe langganan Anda dapat memengaruhi ketersediaan fitur di lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkah dalam lab ini ditulis menggunakan **US Timur**.

## Perkiraan waktu: 50 menit

## Skenario lab

Organisasi Anda memiliki situs web publik. Anda perlu menyeimbangkan beban permintaan publik yang masuk di seluruh mesin virtual. Anda juga perlu menyediakan gambar dan video dari mesin virtual yang berbeda. Anda berencana menerapkan Azure Load Balancer dan Azure Application Gateway. Semua sumber daya berada di wilayah yang sama.

## Simulasi lab interaktif

>**Catatan**: Simulasi lab yang sebelumnya disediakan telah dihentikan.

## Keterampilan pekerjaan

+ Tugas 1: Gunakan templat untuk menyediakan infrastruktur.
+ Tugas 2: Konfigurasikan Azure Load Balancer.
+ Tugas 3: Mengonfigurasi Azure Application Gateway.

## Tugas 1: Gunakan templat untuk menyediakan infrastruktur

Dalam tugas ini, Anda akan menggunakan templat untuk menyebarkan satu jaringan virtual, satu kelompok keamanan jaringan, dan tiga komputer virtual.

1. Unduh file lab **\\Allfiles\\Lab06** (templat dan parameter).

1. Masuk ke **portal Azure** - `https://portal.azure.com`.

1. Cari dan pilih `Deploy a custom template`.

1. Di halaman penyebaran kustom, pilih **Buat templat Anda sendiri di editor**.

1. Di halaman edit templat, pilih **Muat file**.

1. Temukan dan pilih file **\\Allfiles\\Lab06\\az104-06-vms-template.json** lalu pilih **Buka**.

1. Pilih **Simpan**.

1. Pilih **Edit parameter** dan muat file **\\Allfiles\\Lab06\\az104-06-vms-parameters.json**.

1. Pilih **Simpan**.

1. Gunakan informasi berikut untuk melengkapi bidang pada halaman penyebaran kustom, membiarkan semua bidang lain dengan nilai default.

    | Pengaturan       | Nilai         |
    | ---           | ---           |
    | Langganan  | langganan Azure Anda |
    | Grup sumber daya | `az104-rg6` (Jika perlu, pilih **Buat baru**) |
    | Kata sandi      | Berikan kata sandi yang aman |

    >**Catatan**: Jika Anda menerima kesalahan bahwa ukuran VM tidak tersedia, pilih SKU yang tersedia di langganan Anda dan memiliki setidaknya 2 inti.

1. Pilih **Ulas + buat**, lalu pilih **Buat**.

    >**Catatan**: Tunggu hingga penyebaran selesai sebelum berpindah ke tugas berikutnya. Penyebaran akan memakan waktu sekitar 5 menit.

    >**Catatan**: Tinjau sumber daya yang sedang disebarkan. Akan ada satu jaringan virtual dengan tiga subnet. Setiap subnet akan memiliki satu mesin virtual.

## Tugas 2: Konfigurasikan Azure Load Balancer

Dalam tugas ini, Anda menerapkan Azure Load Balancer di depan dua mesin virtual Azure dalam jaringan virtual. Load Balancer di Azure menyediakan konektivitas lapisan 4 di seluruh sumber daya, seperti mesin virtual. Konfigurasi Load Balancer mencakup alamat IP front-end untuk menerima koneksi, kumpulan backend, dan aturan yang menentukan cara koneksi melintasi load balancer.

## Diagram arsitektur - Load Balancer

>**Catatan**: Perhatikan bahwa Load Balancer mendistribusikan di dua mesin virtual dalam jaringan virtual yang sama.

![Diagram tugas lab.](../media/az104-lab06-lb-architecture.png)

1. Di portal Azure, cari dan pilih `Load balancers` dan, pada blade **Load balancer**, klik **+ Buat**.

1. Buat load balancer dengan pengaturan berikut (biarkan pengaturan lain diatur ke nilai defaultnya) lalu klik **Berikutnya: Konfigurasi IP frontend**:

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | langganan Azure Anda |
    | Grup sumber daya | **az104-rg6** |
    | Nama | `az104-lb` |
    | Wilayah | Wilayah yang **sama** dengan wilayah tempat Anda menyebarkan VM |
    | SKU  | **Standard**
           |
    | Jenis | **Publik** |
    | Tingkat | **Regional** |

     ![Cuplikan layar halaman buat load balancer.](../media/az104-lab06-create-lb1.png)

1. Pada tab **Konfigurasi IP frontend**, klik **Tambahkan konfigurasi IP frontend** dan gunakan pengaturan berikut:  

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | `az104-fe` |
    | Jenis IP | Alamat IP |
    | Penyeimbang Beban Gateway | Tidak |
    | Alamat IP publik | Pilih **Buat baru** (gunakan instruksi di langkah berikutnya) |

1. **Pada popup Tambahkan alamat** IP publik, gunakan pengaturan berikut sebelum mengklik **Simpan** dua kali. Setelah selesai klik **Berikutnya: Kumpulan backend**.

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | `az104-lbpip` |
    | SKU | Standard |
    | Tingkat | Wilayah |
    | Penugasan | Statis |
    | Preferensi Perutean | **Jaringan Microsoft** |

    >**Catatan:** SKU Standar menyediakan alamat IP statik. Alamat IP statik ditetapkan dengan sumber daya dibuat dan dirilis saat sumber daya dihapus.  

1. Pada tab **Kumpulan backend**, klik **Tambahkan kumpulan backend** dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya). Klik **Tambahkan** lalu **Simpan**. Klik **Berikutnya: Aturan** masuk.

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | `az104-be` |
    | Jaringan virtual | **az104-06-vnet1** |
    | Konfigurasi Kumpulan Backend | **NIC** |
    | Klik **Tambahkan** untuk menambahkan mesin virtual |  |
    | az104-06-vm0 | **centang kotak** |
    | az104-06-vm1 | **centang kotak** |

1. Saat luang, tinjau tab lainnya, lalu klik **Tinjau + buat**. Pastikan tidak ada kesalahan validasi, lalu klik **Buat**.

1. Tunggu hingga penyeimbang beban diterapkan, lalu klik **Buka sumber daya**.

**Tambahkan aturan untuk menentukan cara lalu lintas masuk didistribusikan**

1. Di bawah blade **Pengaturan**, pilih **Aturan penyeimbangan beban**.

1. Pilih **+ Tambah**. Tambahkan aturan penyeimbangan beban dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya).  Saat Anda mengonfigurasi aturan, gunakan ikon informasi untuk mempelajari setiap pengaturan. Saat selesai, klik **Simpan**.

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
    | IP Mengambang | **Nonaktif** |
    | Terjemahan alamat jaringan sumber keluar (SNAT) | **Direkomendasikan** |

1. Pilih **Konfigurasi IP frontend** dari halaman Load Balancer. Salin alamat IP publik.

1. Buka tab browser lain dan arahkan ke alamat IP. Pastikan jendela browser menampilkan pesan **Halo Dunia dari az104-06-vm0** atau **Halo Dunia dari az104-06-vm1**.

1. Refresh jendela untuk memverifikasi perubahan pesan ke mesin virtual lainnya. Hal ini menunjukkan load balancer berputar melalui mesin virtual.

    > **Catatan**: Anda mungkin perlu merefresh lebih dari sekali atau membuka jendela browser baru dalam mode InPrivate.

## Tugas 3: Mengonfigurasi Azure Application Gateway

Dalam tugas ini, Anda menerapkan Azure Application Gateway di depan dua mesin virtual Azure. Application Gateway menyediakan penyeimbangan beban lapisan 7, Web Application Firewall (WAF), penghentian SSL, dan enkripsi menyeluruh ke sumber daya yang ditentukan dalam kumpulan backend. Application Gateway merutekan gambar ke satu mesin virtual dan video ke mesin virtual lainnya.

## Diagram arsitektur - Application Gateway

>**Catatan**: Application Gateway ini berfungsi di jaringan virtual yang sama dengan Load Balancer. Ini mungkin tidak biasa di lingkungan produksi.

![Diagram tugas lab.](../media/az104-lab06-gw-architecture.png)

1. Di portal Azure, cari dan pilih `Virtual networks`.

1. Pada blade **Jaringan virtual**, di daftar jaringan virtual, klik **az104-06-vnet1**.

1. Pada blade jaringan virtual **az104-06-vnet1**, di bagian **Pengaturan**, klik **Subnet**, lalu klik **+ Subnet**.

1. Tambahkan subnet dengan pengaturan berikut (biarkan pengaturan lain diatur ke nilai defaultnya).

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | `subnet-appgw` |
    | Alamat awal| `10.60.3.224` |
    | Ukuran | `/27` - Pastikan **alamat** awal masih **10.60.3.224**|

1. Klik **Tambahkan**

    > **Catatan**: Subnet ini akan digunakan oleh Azure Application Gateway. Application Gateway memerlukan subnet khusus dengan ukuran /27 atau lebih besar.

1. Di portal Azure, cari dan pilih `Application gateways` dan, pada blade **Application Gateway**, klik **+ Buat**.

1. Pada tab **Dasar**, tentukan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | langganan Azure Anda |
    | Grup sumber daya | `az104-rg6` |
    | Nama gateway aplikasi | `az104-appgw` |
    | Wilayah | Wilayah Azure yang **sama** dengan yang Anda gunakan di Tugas 1 |
    | Tingkat | **Standard V2** |
    | Aktifkan penskalaan otomatis | **Tidak** |
    | Jumlah Instans | `2` |
    | HTTP2 | **Nonaktif** |
    | Jaringan virtual | **az104-06-vnet1** |
    | Subnet | **subnet-appgw (10.60.3.224/27)** |

1. Klik **Berikutnya: Frontend >** dan tentukan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya). Setelah selesai, klik **OKE**.

    | Pengaturan | Nilai |
    | --- | --- |
    | Jenis alamat IP frontend | **Publik** |
    | Alamat IP publik| **Tambah **yang baru |
    | Nama | `az104-gwpip` |
    | Zona ketersediaan | **1** |

    >**Catatan:** Application Gateway dapat memiliki alamat IP publik dan privat.
 
1. Klik **Berikutnya : Backend >** lalu **Tambahkan kumpulan backend**. Tentukan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya). Setelah selesai klik **Tambahkan**.

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | `az104-appgwbe` |
    | Menambahkan kumpulan backend tanpa target | **Tidak** |
    | Komputer virtual | **az104-06-nic1 (10.60.1.4)** |
    | Mesin virtual | **az104-06-nic2 (10.60.2.4)** |

1. klik **tambahkan kumpulan backend**. Ini adalah kumpulan backend untuk **gambar**. Tentukan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya). Setelah selesai klik **Tambahkan**.

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | `az104-imagebe` |
    | Menambahkan kumpulan backend tanpa target | **Tidak** |
    | Komputer virtual | **az104-06-nic1 (10.60.1.4)** |

1. klik **tambahkan kumpulan backend**. Ini adalah kumpulan backend untuk **video**. Tentukan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya). Setelah selesai klik **Tambahkan**.

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | `az104-videobe` |
    | Menambahkan kumpulan backend tanpa target | **Tidak** |
    | Komputer virtual | **az104-06-nic2 (10.60.2.4)** |

1. Pilih **Berikutnya : Konfigurasi >** lalu **Tambahkan aturan perutean**. Lengkapi informasi.

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama aturan | `az104-gwrule` |
    | Prioritas | `10` |
    | Nama listener | `az104-listener` |
    | IP Frontend | **IPv4 publik** |
    | Protokol | **HTTP** |
    | Port | `80` |
    | Tipe listener | **Dasar** |

1. Pindah ke tab **Target backend**. Pilih **Tambahkan** setelah melengkapi informasi dasar.

   | Pengaturan | Nilai |
    | --- | --- |
    | Target ujung belakang | `az104-appgwbe` |
    | Pengaturan backend | `az104-http` (buat baru) |

   >**Catatan:** Luangkan waktu semenit untuk membaca informasi tentang **Afinitas berbasis Cookie** dan **Pengurasan koneksi**.

1. Di bagian **Perutean berbasis jalur**, pilih **Tambahkan beberapa target untuk membuat aturan berbasis jalur**. Anda akan membuat dua aturan. Klik **Tambahkan** setelah aturan pertama lalu **Tambahkan** setelah aturan kedua. 

    **Aturan - perutean ke backend gambar**

    | Pengaturan | Nilai |
    | --- | --- |
    | Jalur | `/image/*` |
    | Nama target | `images` |
    | Pengaturan backend | **az104-http** |
    | Target ujung belakang | `az104-imagebe` |

    **Aturan - perutean ke backend video**

    | Pengaturan | Nilai |
    | --- | --- |
    | Jalur | `/video/*` |
    | Nama target | `videos` |
    | Pengaturan backend | **az104-http** |
    | Target ujung belakang | `az104-videobe` |

1. Pastikan untuk memeriksa perubahan Anda, lalu pilih **Berikutnya : Tag >**. Tidak diperlukan perubahan.

1. Pilih **Berikutnya : Tinjau + buat >** lalu klik **Buat**.

    > **Catatan**: Tunggu hingga instans Application Gateway dibuat. Proses ini akan perlu waktu sekitar 5-10 menit. Saat Anda menunggu, pertimbangkan untuk meninjau beberapa tautan pelatihan mandiri di akhir halaman ini.

1. Setelah application gateway disebarkan, cari dan pilih **az104-appgw**.

1. Di sumber daya **Application Gateway**, di bagian **Pemantauan**, pilih **Kondisi backend**.

1. Pastikan kedua server di tampilan kumpulan backend dalam **Kondisi baik**.

1. Pada blade **Gambaran Umum**, salin nilai **Alamat IP publik frontend**.

1. Mulai jendela browser lain dan uji URL ini - `http://<frontend ip address>/image/`.

1. Verifikasi bahwa Anda diarahkan ke server gambar (vm1).

1. Mulai jendela browser lain dan uji URL ini - `http://<frontend ip address>/video/`.

1. Verifikasi bahwa Anda diarahkan ke server video (vm2).

> **Catatan**: Anda mungkin perlu merefresh lebih dari sekali atau membuka jendela browser baru dalam mode InPrivate.

## Bersihkan sumber daya Anda

Jika Anda bekerja dengan **langganan Anda sendiri** luangkan waktu sebentar untuk menghapus sumber daya lab. Hal ini akan memastikan sumber daya dikosongkan dan biaya diminimalkan. Cara termudah untuk menghapus sumber daya lab adalah dengan menghapus grup sumber daya lab. 

+ Di portal Microsoft Azure, pilih grup sumber daya, pilih **Hapus grup sumber daya**, **Masukkan nama grup sumber daya**, lalu klik **Hapus**.
+ Menggunakan Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Menggunakan CLI, `az group delete --name resourceGroupName`.

## Perluas pemelajaran Anda dengan Copilot

Copilot dapat membantu Anda mempelajari cara menggunakan alat pembuatan skrip Azure. Copilot juga dapat membantu di area yang tidak tercakup dalam lab atau ketika Anda memerlukan informasi lebih lanjut. Buka browser Edge dan pilih Copilot (kanan atas) atau navigasi *copilot.microsoft.com*. Luangkan beberapa menit untuk mencoba perintah ini.

+ Bandingkan dan bedakan Azure Load Balancer dengan Azure Application Gateway. Bantu saya memutuskan skenario mana saya harus menggunakan setiap produk.
+ Alat apa yang tersedia untuk memecahkan masalah koneksi ke Azure Load Balancer? 
+ Apa langkah-langkah dasar untuk mengonfigurasi Azure Application Gateway? Berikan daftar periksa tingkat tinggi. 
+ Buat tabel yang menyoroti tiga solusi penyeimbangan beban Azure. Untuk setiap solusi menunjukkan protokol yang didukung, kebijakan perutean, afinitas sesi, dan offloading TLS.
  
## Pelajari lebih lanjut dengan pelatihan mandiri

+ [Meningkatkan skalabilitas dan ketahanan aplikasi dengan menggunakan Azure Load Balancer](https://learn.microsoft.com/training/modules/improve-app-scalability-resiliency-with-load-balancer/). Bahas beragam load balancer di Azure dan cara memilih solusi load balancer Azure yang tepat untuk memenuhi kebutuhan Anda.
+ [Menyeimbangkan beban lalu lintas layanan web Anda dengan Application Gateway](https://learn.microsoft.com/training/modules/load-balance-web-traffic-with-application-gateway/). Tingkatkan ketahanan aplikasi dengan mendistribusikan muatan di beberapa server dan gunakan perutean berbasis jalur untuk mengarahkan lalu lintas web.

## Poin penting

Selamat atas penyelesaian lab ini. Berikut adalah poin-poin penting untuk lab ini.

+ Azure Load Balancer adalah pilihan yang sangat baik untuk mendistribusikan lalu lintas jaringan pada beberapa mesin virtual di lapisan transportasi (OSI lapisan 4 - TCP dan UDP).
+ Load Balancer Publik digunakan untuk menyeimbangkan beban lalu lintas internet ke VM Anda. Penyeimbang beban internal (atau privat) digunakan jika IP privat hanya diperlukan di frontend.
+ Load balancer Dasar adalah untuk aplikasi skala kecil yang tidak memerlukan high availability atau redundansi. Load balancer Standar adalah untuk performa tinggi dan latensi ultra rendah.
+ Azure Application Gateway adalah penyeimbang beban lalu lintas web (OSI lapisan 7) yang memungkinkan Anda mengelola lalu lintas ke aplikasi web Anda.
+ Application Gateway tingkat Standar menawarkan semua fungsionalitas L7, termasuk penyeimbangan beban, tingkat WAF menambahkan firewall untuk memeriksa lalu lintas berbahaya.
+ Application Gateway dapat membuat keputusan perutean berdasarkan atribut tambahan dari permintaan HTTP, misalnya jalur URI atau header host.
