---
lab:
  title: 'Lab 04: Menerapkan Virtual Networking'
  module: Implement Virtual Networking
---

# Lab 04 - Menerapkan Virtual Network

## Pengenalan lab

Lab ini adalah lab pertama dari tiga lab yang berfokus pada jaringan virtual. Di lab ini, Anda mempelajari dasar-dasar jaringan virtual dan subnet. Anda mempelajari cara melindungi jaringan Anda dengan kelompok keamanan jaringan dan kelompok keamanan aplikasi. Anda juga mempelajari tentang zona dan rekaman DNS. 

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur di lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkahnya ditulis menggunakan **US** Timur.

## Perkiraan waktu: 50 menit

## Skenario lab 

Organisasi global Anda berencana untuk menerapkan jaringan virtual. Tujuan langsungnya adalah untuk mengakomodasi semua sumber daya yang ada. Namun, organisasi berada dalam fase pertumbuhan dan ingin memastikan ada kapasitas tambahan untuk pertumbuhan tersebut.

Jaringan **virtual CoreServicesVnet** memiliki jumlah sumber daya terbesar. Sejumlah besar pertumbuhan diantisipasi, sehingga ruang alamat besar diperlukan untuk jaringan virtual ini.

Jaringan **virtual ManufacturingVnet** berisi sistem untuk pengoperasian fasilitas manufaktur. Organisasi ini mengantisipasi sejumlah besar perangkat yang terhubung secara internal agar sistem mereka dapat mengambil data. 

## Simulasi lab interaktif

Ada beberapa simulasi lab interaktif yang mungkin berguna bagi Anda untuk topik ini. Simulasi ini memungkinkan Anda mengklik skenario serupa dengan kecepatan Anda sendiri. Ada perbedaan antara simulasi interaktif dan lab ini, tetapi banyak konsep intinya sama. Langganan Azure tidak diperlukan. 

+ [Mengamankan lalu lintas](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2013) jaringan. Buat komputer virtual, jaringan virtual, dan grup keamanan jaringan. Tambahkan aturan kelompok keamanan jaringan untuk mengizinkan dan melarang lalu lintas.
  
+ [Buat jaringan](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%204) virtual sederhana. Buat jaringan virtual dengan dua komputer virtual. Menunjukkan komputer virtual dapat berkomunikasi. 

+ [Merancang dan mengimplementasikan jaringan virtual di Azure](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Design%20and%20implement%20a%20virtual%20network%20in%20Azure). Buat grup sumber daya dan buat jaringan virtual dengan subnet.  

+ [Menerapkan jaringan](https://mslabs.cloudguides.com/en-us/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%208) virtual. Membuat dan mengonfigurasi jaringan virtual, menyebarkan komputer virtual, mengonfigurasi grup keamanan jaringan, dan mengonfigurasi Azure DNS.

## Diagram arsitektur

![Tata letak jaringan](../media/az104-lab04-architecture.png)

Jaringan virtual dan subnet ini disusun dengan cara yang mengakomodasi sumber daya yang ada namun memungkinkan pertumbuhan yang diproyeksikan. Mari kita buat jaringan virtual dan subnet ini untuk meletakkan fondasi untuk infrastruktur jaringan kita.

>**Tahukah Anda?**: Ini adalah praktik yang baik untuk menghindari rentang alamat IP yang tumpang tindih untuk mengurangi masalah dan menyederhanakan pemecahan masalah. Tumpang tindih adalah masalah di seluruh jaringan, baik di cloud atau lokal. Banyak organisasi merancang skema alamat IP di seluruh perusahaan untuk menghindari tumpang tindih dan merencanakan pertumbuhan di masa mendatang.

## Keterampilan pekerjaan

+ Tugas 1: Buat jaringan virtual dengan subnet menggunakan portal.
+ Tugas 2: Buat jaringan virtual dan subnet menggunakan templat.
+ Tugas 3: Membuat dan mengonfigurasi komunikasi antara Kelompok Keamanan Aplikasi dan Grup Keamanan Jaringan.
+ Tugas 4: Mengonfigurasi zona Azure DNS publik dan privat.
  
## Tugas 1: Membuat jaringan virtual dengan subnet menggunakan portal

Organisasi merencanakan sejumlah besar pertumbuhan untuk layanan inti. Dalam tugas ini, Anda membuat jaringan virtual dan subnet terkait untuk mengakomodasi sumber daya yang ada dan pertumbuhan yang direncanakan. Dalam tugas ini, Anda akan menggunakan portal Azure. 

1. Masuk ke **portal Azure** - `https://portal.azure.com`.
   
1. Cari dan pilih `Virtual Networks`.

1. Pilih **Buat** pada halaman Jaringan virtual.

1. Lengkapi tab **Dasar** untuk CoreServicesVnet.  

    |  **Opsi**         | **Nilai**            |
    | ------------------ | -------------------- |
    | Grup Sumber Daya     | `az104-rg4` (jika perlu, buat baru) |
    | Nama               | `CoreServicesVnet`     |
    | Wilayah             | (US) **US Timur**         |

1. Pindahkan ke tab **Alamat** IP.

    |  **Opsi**         | **Nilai**            |
    | ------------------ | -------------------- |
    | Ruang alamat IPv4 | `10.20.0.0/16` (pisahkan entri)    |

1. Pilih **+ Tambahkan subnet**. Lengkapi informasi nama dan alamat untuk setiap subnet. Pastikan untuk memilih **Tambahkan** untuk setiap subnet baru. 

    | **Subnet**             | **Opsi**           | **Nilai**              |
    | ---------------------- | -------------------- | ---------------------- |
    | SharedServicesSubnet   | Nama subnet          | `SharedServicesSubnet`   |
    |                        | Alamat awal     | `10.20.10.0`          |
    |                        | Ukuran                 | `/24` |
    | DatabaseSubnet         | Nama subnet          | `DatabaseSubnet`         |
    |                        | Alamat awal     | `10.20.20.0`        |
    |                        | Ukuran                 | `/24` |

    >**Catatan:** Setiap jaringan virtual harus memiliki setidaknya satu subnet. Pengingat bahwa lima alamat IP akan selalu dicadangkan, jadi pertimbangkan bahwa dalam perencanaan Anda. 

1. Untuk menyelesaikan pembuatan CoreServicesVnet dan subnet terkait, pilih **Tinjau + buat**.

1. Verifikasi konfigurasi Anda lulus validasi, lalu pilih **Buat**.

1. Tunggu hingga jaringan virtual disebarkan lalu pilih **Buka sumber daya**.

1. Luangkan waktu satu menit untuk memverifikasi **ruang** Alamat dan **Subnet**. Perhatikan pilihan Anda yang lain di bilah **Pengaturan**. 

1. Di bagian **Automation** , pilih **Ekspor templat**, lalu tunggu hingga templat dibuat.

1. **Unduh** templat.

1. Navigasikan pada komputer lokal ke **folder Unduhan** dan **Ekstrak semua** file dalam file zip yang diunduh. 

1. Sebelum melanjutkan, pastikan Anda memiliki **file template.json** . Anda akan menggunakan templat ini untuk membuat ManufacturingVnet di tugas berikutnya. 
 
## Tugas 2: Membuat jaringan virtual dan subnet menggunakan templat

Dalam tugas ini, Anda membuat jaringan virtual ManufacturingVnet dan subnet terkait. Organisasi mengantisipasi pertumbuhan untuk kantor manufaktur sehingga subnet berukuran untuk pertumbuhan yang diharapkan. Untuk tugas ini, Anda menggunakan templat untuk membuat sumber daya. 

1. Temukan file template.json** yang **diekspor di tugas sebelumnya. Seharusnya ada di folder Unduhan** Anda**.

1. Edit file menggunakan editor pilihan Anda. Banyak editor memiliki *fitur perubahan semua kemunculan* . Jika Anda menggunakan Visual Studio Code, pastikan Anda bekerja di **jendela** tepercaya dan bukan dalam **mode** terbatas. Lihat diagram arsitektur untuk memverifikasi detailnya. 

### Membuat perubahan untuk jaringan virtual ManufacturingVnet

1. Ganti semua kemunculan **CoreServicesVnet** dengan `ManufacturingVnet`. 

1. Ganti semua kemunculan **10.20.0.0** dengan `10.30.0.0`. 

### Membuat perubahan untuk subnet ManufacturingVnet

1. Ubah semua kemunculan **SharedServicesSubnet** menjadi `SensorSubnet1`.

1. Ubah semua kemunculan **10.20.10.0/24** menjadi `10.30.20.0/24`.

1. Ubah semua kemunculan **DatabaseSubnet** menjadi `SensorSubnet2`.

1. Ubah semua kemunculan **10.20.20.0/24** menjadi `10.30.21.0/24`.

1. Baca kembali file dan pastikan semuanya terlihat benar.

1. Pastikan untuk **Menyimpan** perubahan Anda.

>**Catatan:** Ada file templat yang telah selesai di direktori file lab. 

### Membuat perubahan pada file parameter

1. Temukan file parameters.json** yang **diekspor di tugas sebelumnya. Seharusnya ada di folder Unduhan** Anda**.

1. Edit file menggunakan editor pilihan Anda.

1. Ganti satu kemunculan **CoreServicesVnet** dengan `ManufacturingVnet`.

1. **Simpan** perubahan Anda.
   
### Menyebarkan templat kustom

1. Di portal, cari dan pilih **Sebarkan templat** kustom.

1. Pilih **Bangun templat Anda sendiri di editor** lalu **Muat file**.

1. Pilih **file templates.json** dengan perubahan Manufaktur Anda, lalu pilih **Simpan**.

1. Pilih **Ulas + buat**, lalu pilih **Buat**.

1. Tunggu hingga templat disebarkan, lalu konfirmasikan (di portal) jaringan virtual Manufaktur dan subnet dibuat.

>**Catatan:** Jika Anda harus menyebarkan lebih dari satu kali Anda mungkin menemukan beberapa sumber daya berhasil diselesaikan dan penyebaran gagal. Anda dapat menghapus sumber daya tersebut secara manual dan mencoba lagi. 
   
## Tugas 3: Membuat dan mengonfigurasi komunikasi antara Kelompok Keamanan Aplikasi dan Grup Keamanan Jaringan

Dalam tugas ini, kami membuat Kelompok Keamanan Aplikasi dan Kelompok Keamanan Jaringan. NSG akan memiliki aturan keamanan masuk yang memungkinkan lalu lintas dari ASG. NSG juga akan memiliki aturan keluar yang menolak akses ke internet. 

### Membuat Kelompok Keamanan Aplikasi (ASG)

1. Di portal Azure, cari dan pilih `Application security groups`.

1. Klik **Buat** dan berikan informasi dasar.

    | Pengaturan | Nilai |
    | -- | -- |
    | Langganan | *langganan Anda* |
    | Grup sumber daya | **az104-rg4** |
    | Nama | `asg-web` |
    | Wilayah | **US Timur**  |

1. Klik **Tinjau + buat** lalu setelah validasi klik **Buat**.

### Buat Grup Keamanan Jaringan dan kaitkan dengan subnet ASG

1. Di portal Azure, cari dan pilih `Network security groups`.

1. Pilih **+ Buat** dan berikan informasi pada tab **Dasar** . 

    | Pengaturan | Nilai |
    | -- | -- |
    | Langganan | *langganan Anda* |
    | Grup sumber daya | **az104-rg4** |
    | Nama | `myNSGSecure` |
    | Wilayah | **US Timur**  |

1. Klik **Tinjau + buat** lalu setelah validasi klik **Buat**.

1. Setelah NSG disebarkan, klik **Buka sumber daya**.

1. Di bawah **Pengaturan** klik **Subnet** lalu **Kaitkan**.

    | Pengaturan | Nilai |
    | -- | -- |
    | Jaringan virtual | **CoreServicesVnet (az104-rg4)** |
    | Subnet | **SharedServicesSubnet** |

1. Klik **OK** untuk menyimpan asosiasi.

### Mengonfigurasi aturan keamanan masuk untuk mengizinkan lalu lintas ASG

1. Lanjutkan bekerja dengan NSG Anda. **Di area Pengaturan**, pilih **Aturan** keamanan masuk.

1. Tinjau aturan masuk default. Perhatikan bahwa hanya jaringan virtual dan load balancer lain yang diizinkan mengakses.

1. Pilih **+ Tambah**.

1. Pada bilah **Tambahkan aturan** keamanan masuk, gunakan informasi berikut untuk menambahkan aturan port masuk. Aturan ini memungkinkan lalu lintas ASG. Setelah selesai, pilih **Tambahkan**.

    | Pengaturan | Nilai |
    | -- | -- |
    | Sumber | **Kelompok keamanan aplikasi** |
    | Grup keamanan aplikasi sumber | **asg-web** |
    | Source port ranges |  * |
    | Tujuan | **Mana pun** |
    | Layanan | **Kustom** (perhatikan pilihan Anda yang lain) |
    | Rentang port tujuan | **80,443** |
    | Protokol | **TCP** |
    | Tindakan
           | **Izinkan** |
    | Prioritas | **100** |
    | Nama | `AllowASG` |

### Mengonfigurasi aturan NSG keluar yang menolak akses Internet

1. Setelah membuat aturan NSG masuk Anda, pilih **Aturan** keamanan keluar. 

1. **Perhatikan aturan AllowInternetOutboundRule**. Perhatikan juga aturan tidak dapat dihapus dan prioritasnya adalah 65001.

1. Pilih **+ Tambahkan** lalu konfigurasikan aturan keluar yang menolak akses ke internet. Setelah selesai, pilih **Tambahkan**.

    | Pengaturan | Nilai |
    | -- | -- |
    | Sumber | **Mana pun** |
    | Source port ranges |  * |
    | Tujuan | **Tag layanan** |
    | Tag layanan tujuan | **Internet** |
    | Layanan | **Adat** |
    | Rentang port tujuan | **8080** |
    | Protokol | **Mana pun** |
    | Tindakan
           | **Tolak** |
    | Prioritas | **4096** |
    | Nama | **DenyAnyCustom8080Outbound** |


## Tugas 4: Mengonfigurasi zona Azure DNS publik dan privat

Dalam tugas ini, Anda akan membuat dan mengonfigurasi zona DNS publik dan privat. 

### Mengonfigurasi zona DNS publik

Anda bisa mengonfigurasi Azure DNS untuk meresolusi nama host di domain publik Anda. Misalnya, jika Anda membeli nama domain contoso.xyz dari pencatat nama domain, Anda dapat mengonfigurasi Azure DNS untuk menghosting `contoso.com` domain dan menyelesaikan www.contoso.xyz ke alamat IP server web atau aplikasi web Anda.

1. Di portal, cari dan pilih `DNS zones`.

1. Pilih **+ Buat.**

1. Konfigurasikan tab **Dasar** .

    | Properti | Nilai    |
    |:---------|:---------|
    | Langganan | **Pilih langganan Anda** |
    | Grup sumber daya | **az-104-rg4** |
    | Nama | `contoso.com` (jika dipesan sesuaikan nama) |
    | Wilayah |**US** Timur (tinjau ikon informasi) |

1. Pilih **Tinjau buat** lalu **Buat**.
   
1. Tunggu hingga zona DNS disebarkan lalu pilih **Buka sumber daya**.

1. Pada bilah **Gambaran Umum** , perhatikan nama empat server nama Azure DNS yang ditetapkan ke zona tersebut. **Salin** salah satu alamat server nama. Anda akan membutuhkannya di langkah mendatang. 
  
1. Pilih **Kumpulan catatan**. Anda menambahkan rekaman tautan jaringan virtual untuk setiap jaringan virtual yang memerlukan dukungan resolusi nama privat.

    | Properti | Nilai    |
    |:---------|:---------|
    | Nama | **Www** |
    | Jenis | **A** |
    | TTL | **1** |
    | Alamat IP | **10.1.1.4** |

>**Catatan:**  Dalam skenario dunia nyata, Anda akan memasukkan alamat IP publik server web Anda.

1. Pilih **OK** dan verifikasi **contoso.com** memiliki kumpulan catatan A bernama **www**.

1. Buka perintah dan jalankan perintah berikut:

   ```sh
   nslookup www.contoso.com <name server name>
   ```
1. Verifikasi nama host www.contoso.com diselesaikan ke alamat IP yang Anda berikan. Ini mengonfirmasi resolusi nama berfungsi dengan benar.

### Mengonfigurasi zona DNS privat

Zona DNS privat menyediakan layanan resolusi nama dalam jaringan virtual. Zona DNS privat hanya dapat diakses dari jaringan virtual yang ditautkan dan tidak dapat diakses dari internet. 

1. Di portal, cari dan pilih `Private dns zones`.

1. Pilih **+ Buat.**

1. Pada tab **Dasar** dari Buat zona DNS privat, masukkan informasi seperti yang tercantum dalam tabel di bawah ini:

    | Properti | Nilai    |
    |:---------|:---------|
    | Langganan | **Pilih langganan Anda** |
    | Grup sumber daya | **az-104-rg4** |
    | Nama | `private.contoso.com` (sesuaikan jika Anda harus mengganti nama) |
    | Wilayah |**US Timur** |

1. Pilih **Tinjau buat** lalu **Buat**.
   
1. Tunggu hingga zona DNS disebarkan lalu pilih **Buka sumber daya**.

1. Perhatikan pada bilah **Gambaran Umum** tidak ada catatan server nama. 

1. Pilih **+ Tautan jaringan virtual** lalu pilih **+ Tambahkan**. 

    | Properti | Nilai    |
    |:---------|:---------|
    | Nama tautan | `manufacturing-link` |
    | Jaringan virtual | `ManufacturingVnet` |

1. Pilih **OK** dan tunggu hingga tautan dibuat. 

1. Dari bilah **Gambaran Umum** pilih **+ Kumpulan catatan**. Anda sekarang akan menambahkan catatan untuk setiap komputer virtual yang membutuhkan dukungan resolusi nama privat.

    | Properti | Nilai    |
    |:---------|:---------|
    | Nama | **sensorvm** |
    | Jenis | **A** |
    | TTL | **1** |
    | Alamat IP | **10.1.1.4** |

 >**Catatan:**  Dalam skenario dunia nyata, Anda akan memasukkan alamat IP untuk mesin virtual manufaktur tertentu.

## Membersihkan sumber daya Anda

Jika Anda bekerja dengan **langganan** Anda sendiri membutuhkan waktu satu menit untuk menghapus sumber daya lab. Ini akan memastikan sumber daya dibebankan dan biaya diminimalkan. Cara term mudah untuk menghapus sumber daya lab adalah dengan menghapus grup sumber daya lab. 

+ Di portal Azure, pilih grup sumber daya, pilih **Hapus grup** sumber daya, **Masukkan nama** grup sumber daya, lalu klik **Hapus**.
+ Menggunakan Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Menggunakan CLI, `az group delete --name resourceGroupName`.
 
## Poin penting

Selamat atas penyelesaian lab. Berikut adalah takeaway utama untuk lab ini. 

+ Jaringan virtual adalah representasi jaringan Anda sendiri di cloud. 
+ Saat merancang jaringan virtual, ini adalah praktik yang baik untuk menghindari rentang alamat IP yang tumpang tindih. Ini akan mengurangi masalah dan menyederhanakan pemecahan masalah.
+ Subnet adalah rangkaian alamat IP di jaringan virtual. Anda dapat membagi jaringan virtual menjadi beberapa subnet untuk organisasi dan keamanan.
+ Kelompok keamanan jaringan berisi aturan keamanan yang mengizinkan atau menolak lalu lintas jaringan. Ada aturan masuk dan keluar default yang dapat Anda sesuaikan dengan kebutuhan Anda.
+ Grup keamanan aplikasi digunakan untuk melindungi grup server dengan fungsi umum, seperti server web atau server database.
+ Azure DNS adalah layanan hosting untuk domain DNS yang menyediakan resolusi nama. Anda bisa mengonfigurasi Azure DNS untuk meresolusi nama host di domain publik Anda.  Anda juga dapat menggunakan zona DNS privat untuk menetapkan nama DNS ke komputer virtual (VM) di jaringan virtual Azure Anda.

## Pelajari lebih lanjut dengan pelatihan mandiri

+ [Pengantar Azure Virtual Networks](https://learn.microsoft.com/training/modules/introduction-to-azure-virtual-networks/). Merancang dan mengimplementasikan infrastruktur Azure Networking inti seperti jaringan virtual, IP publik dan privat, DNS, peering jaringan virtual, perutean, dan Azure Virtual NAT.
+ [Merancang skema](https://learn.microsoft.com/training/modules/design-ip-addressing-for-azure/) alamat IP. Identifikasi kemampuan alamat IP privat dan publik Azure dan jaringan virtual lokal.
+ [Mengamankan dan mengisolasi akses ke sumber daya Azure dengan menggunakan grup keamanan jaringan dan titik](https://learn.microsoft.com/training/modules/secure-and-isolate-with-nsg-and-service-endpoints/) akhir layanan. Grup keamanan jaringan dan titik akhir layanan membantu Anda mengamankan komputer virtual dan layanan Azure dari akses jaringan yang tidak sah.
+ [Host domain Anda di Azure DNS](https://learn.microsoft.com/training/modules/host-domain-azure-dns/). Membuat zona DNS untuk nama domain Anda. Buat catatan DNS untuk memetakan domain ke alamat IP. Uji apakah nama domain tersebut ditetapkan ke server web Anda.
  
