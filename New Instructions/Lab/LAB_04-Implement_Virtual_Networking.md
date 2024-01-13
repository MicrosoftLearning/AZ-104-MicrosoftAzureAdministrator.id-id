# Lab 04 - Menerapkan Virtual Network

## Pengenalan lab

Lab ini adalah lab pertama dari tiga lab yang berfokus pada jaringan virtual. Di lab ini, Anda mempelajari dasar-dasar jaringan virtual dan subnet. Anda juga mempelajari cara melindungi jaringan Anda dengan kelompok keamanan jaringan dan kelompok keamanan aplikasi. 

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur di lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkahnya ditulis menggunakan **US** Timur dan **Eropa** Barat. 

## Perkiraan waktu: 40 menit

## Skenario lab 

Organisasi global Anda berencana untuk menerapkan jaringan virtual. Jaringan ini berada di US Timur, Eropa Barat, dan Asia Tenggara. Tujuan langsungnya adalah untuk mengakomodasi semua sumber daya yang ada. Namun, organisasi berada dalam fase pertumbuhan dan ingin memastikan ada kapasitas tambahan untuk pertumbuhan tersebut.

Jaringan virtual **CoreServicesVnet** disebarkan di wilayah **US Timur**. Jaringan virtual ini memiliki jumlah sumber daya terbesar. Jaringan memiliki konektivitas ke jaringan lokal melalui koneksi VPN. Jaringan ini memiliki layanan web, database, dan sistem lain yang menjadi kunci operasi bisnis. Layanan bersama, seperti pengendali domain dan DNS terletak di sini. Sejumlah besar pertumbuhan diantisipasi, sehingga ruang alamat besar diperlukan untuk jaringan virtual ini.

Jaringan virtual **ManufacturingVnet** disebarkan di wilayah **Eropa Barat**, di dekat lokasi fasilitas manufaktur organisasi Anda. Jaringan virtual ini berisi sistem untuk pengoperasian fasilitas manufaktur. Organisasi ini mengantisipasi sejumlah besar perangkat internal yang terhubung agar sistem mereka mengambil data, seperti suhu, dan membutuhkan ruang alamat IP yang dapat diperluas.

## Simulasi lab interaktif

Ada beberapa simulasi lab interaktif yang mungkin berguna bagi Anda untuk topik ini. Simulasi ini memungkinkan Anda mengklik skenario serupa dengan kecepatan Anda sendiri. Ada perbedaan antara simulasi interaktif dan lab ini, tetapi banyak konsep intinya sama. Langganan Azure tidak diperlukan. 

+ [Mengamankan lalu lintas](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2013) jaringan. Buat komputer virtual, jaringan virtual, dan grup keamanan jaringan. Tambahkan aturan kelompok keamanan jaringan untuk mengizinkan dan melarang lalu lintas.
  
+ [Buat jaringan](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%204) virtual sederhana. Buat jaringan virtual dengan dua komputer virtual. Menunjukkan komputer virtual dapat berkomunikasi. 

+ [Merancang dan mengimplementasikan jaringan virtual di Azure](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Design%20and%20implement%20a%20virtual%20network%20in%20Azure). Buat grup sumber daya dan buat jaringan virtual dengan subnet.  

+ [Menerapkan jaringan](https://mslabs.cloudguides.com/en-us/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%208) virtual. Membuat dan mengonfigurasi jaringan virtual, menyebarkan komputer virtual, mengonfigurasi grup keamanan jaringan, dan mengonfigurasi Azure DNS.

## Diagram arsitektur

![Tata letak jaringan](../media/az104-lab04-arch-diagram.png)

Jaringan virtual dan subnet ini disusun dengan cara yang mengakomodasi sumber daya yang ada namun memungkinkan pertumbuhan yang diproyeksikan. Mari kita buat jaringan virtual dan subnet ini untuk meletakkan fondasi untuk infrastruktur jaringan kita.

>**Tahukah Anda?**: Ini adalah praktik yang baik untuk menghindari rentang alamat IP yang tumpang tindih untuk mengurangi masalah dan menyederhanakan pemecahan masalah. Tumpang tindih adalah masalah di seluruh jaringan, baik di cloud atau lokal. Banyak organisasi merancang skema alamat IP di seluruh perusahaan untuk menghindari tumpang tindih dan merencanakan pertumbuhan di masa mendatang.

## Tugas

+ Tugas 1: Membuat grup sumber daya.
+ Tugas 2: Buat jaringan virtual dan subnet CoreServicesVnet.
+ Tugas 3: Buat jaringan virtual dan subnet ManufacturingVnet.
+ Tugas 4: Mengonfigurasi komunikasi antara Kelompok Keamanan Aplikasi dan Kelompok Keamanan Jaringan. 


## Tugas 1: Membuat grup sumber daya

### Buat grup sumber daya untuk semua sumber daya di lab ini. 

1. Masuk ke **portal Azure** - `https://portal.azure.com`.

1. Cari dan pilih **Grup** sumber daya, lalu pilih **+ Buat**.  

1. Buat grup sumber daya dengan pengaturan ini. 

    | **Tab**         | **Opsi**                                 | **Nilai**            |
    | --------------- | ------------------------------------------ | -------------------- |
    | Dasar          | Grup sumber daya                             | `az104-rg4` |
    |                 | Wilayah                                     | (US) **US Timur**     |
    | Tag            | Perubahan tidak diperlukan                        |                      |
   
1. Setelah selesai pilih **Tinjau + buat** lalu **Buat**.
   
## Tugas 2: Membuat jaringan virtual dan subnet CoreServicesVnet

Organisasi merencanakan sejumlah besar pertumbuhan untuk layanan inti. Dalam tugas ini, Anda membuat jaringan virtual dan subnet terkait untuk mengakomodasi sumber daya yang ada dan pertumbuhan yang direncanakan.

1. Cari dan pilih `Virtual Networks`.

1. Pilih **Buat** di halaman Jaringan virtual dan lengkapi **tab Dasar** dan **alamat** IP. 

1. Gunakan informasi dalam tabel berikut untuk membuat jaringan virtual CoreServicesVnet.  

    | **Tab**      | **Opsi**         | **Nilai**            |
    | ------------ | ------------------ | -------------------- |
    | Dasar       | Grup Sumber Daya     | **az104-rg4** |
    |              | Nama               | `CoreServicesVnet`     |
    |              | Wilayah             | (US) **US Timur**         |
    | Alamat IP | Ruang alamat IPv4 | `10.20.0.0/16` (pisahkan entri)    |


1. Pilih **+ Tambahkan subnet**. Lengkapi informasi nama dan alamat untuk setiap subnet. Pastikan untuk memilih **Tambahkan** untuk setiap subnet baru. 

    | **Subnet**             | **Opsi**           | **Nilai**              |
    | ---------------------- | -------------------- | ---------------------- |
    | SharedServicesSubnet   | Nama subnet          | `SharedServicesSubnet`   |
    |                        | Alamat awal     | `10.20.10.0`          |
    |                        | Ukuran                 | `/24` |
    | DatabaseSubnet         | Nama subnet          | `DatabaseSubnet`         |
    |                        | Alamat awal     | `10.20.20.0`        |
    |                        | Ukuran                 | `/24` |

1. Untuk menyelesaikan pembuatan CoreServicesVnet dan subnet terkait, pilih **Tinjau + buat**.

1. Verifikasi konfigurasi Anda lulus validasi, lalu pilih **Buat**.

1. Tunggu hingga jaringan virtual disebarkan lalu pilih **Buka sumber daya**. 

1. Di bagian **Automation** , pilih **Ekspor templat**, lalu tunggu hingga templat dibuat.

1. **Unduh** templat.

1. Navigasikan pada komputer lokal ke **folder Unduhan** dan **Ekstrak semua** file dalam file zip yang diunduh. 

1. Sebelum melanjutkan, pastikan Anda memiliki dua file **template.json** dan **parameters.json**. Luangkan waktu satu menit untuk meninjau file dan informasi tentang CoreServicesVnet. Anda akan menggunakan templat ini untuk membuat ManufacturingVnet di tugas berikutnya. 
 
## Tugas 3: Membuat jaringan virtual dan subnet ManufacturingVnet

Dalam tugas ini, Anda membuat jaringan virtual ManufacturingVnet dan subnet terkait. Organisasi mengantisipasi pertumbuhan untuk kantor manufaktur sehingga subnet berukuran untuk pertumbuhan yang diharapkan.

1. **Temukan file template.json** yang diekspor di tugas sebelumnya. Seharusnya ada di folder Unduhan** Anda**.

1. Edit file menggunakan editor pilihan Anda. Jika Anda menggunakan Visual Studio Code, pastikan Anda bekerja di **jendela** tepercaya dan bukan dalam **mode** terbatas.

   >**Catatan:** Untuk tugas ini kami menunjukkan cara mengedit dan menyebarkan ulang templat. Jika terlalu membingungkan, templat yang sudah selesai disediakan. 

### Membuat perubahan untuk jaringan virtual ManufacturingVnet

1. Ganti semua kemunculan **CoreServicesVnet** dengan `ManufacturingVnet`. 

1. Ganti semua kemunculan **eastus** dengan `westeurope`. 

1. Ganti semua kemunculan **10.20.0.0/16** dengan `10.30.0.0/16`. 

### Membuat perubahan untuk subnet ManufacturingVnet

1. Ubah semua kemunculan **SharedServicesSubnet** menjadi `SensorSubnet1`.

1. Ubah semua kemunculan **10.20.10.0/24** menjadi `10.30.20.0/24`.

1. Ubah semua kemunculan **DatabaseSubnet** menjadi `SensorSubnet2`.

1. Ubah semua kemunculan **10.20.20.0/24** menjadi `10.30.21.0/24`.

1. Baca kembali file dan pastikan semuanya terlihat benar.

1. Pastikan untuk **Menyimpan** perubahan Anda.

### Menyebarkan templat kustom

1. Di portal, cari dan pilih **Sebarkan templat** kustom.

1. Pilih **Bangun templat Anda sendiri di editor** lalu **Muat file**.

1. **Pilih file templates.json** dengan perubahan Manufaktur Anda, lalu pilih **Simpan**.

1. Pilih **Ulas + buat**, lalu pilih **Buat**.

1. Tunggu hingga templat disebarkan, lalu konfirmasikan (di portal) jaringan virtual Manufaktur dibuat. 
   
## Tugas 4: Mengonfigurasi komunikasi antara Kelompok Keamanan Aplikasi dan Kelompok Keamanan Jaringan 

Dalam tugas ini, kami membuat Kelompok Keamanan Aplikasi dan Kelompok Keamanan Jaringan. NSG akan memiliki aturan keamanan masuk yang memungkinkan lalu lintas dari ASG. 

### Membuat Kelompok Keamanan Aplikasi (ASG)

1. Di portal Azure, cari dan pilih **Kelompok keamanan aplikasi**.

1. Klik **Buat** dan berikan informasi dasar.

    | Pengaturan | Nilai |
    | -- | -- |
    | Langganan | *langganan Anda* |
    | Grup sumber daya | **az104-rg4** |
    | Nama | `asg-web` |
    | Wilayah | **Eropa Barat**  |

1. Klik **Tinjau + buat** lalu setelah validasi klik **Buat**.

### Buat Grup Keamanan Jaringan dan kaitkan dengan subnet ASG

1. Di portal Microsoft Azure, cari dan pilih **Grup keamanan jaringan**.

1. Pilih **+ Buat** dan berikan informasi pada tab **Dasar** . 

    | Pengaturan | Nilai |
    | -- | -- |
    | Langganan | *langganan Anda* |
    | Grup sumber daya | **az104-rg4** |
    | Nama | `myNSGSecure` |
    | Wilayah | **(AS) AS Timur**  |

1. Klik **Tinjau + buat** lalu setelah validasi klik **Buat**.

1. Setelah NSG dibuat, klik **Buka sumber daya**.

1. Di bawah **Pengaturan** klik **Subnet** lalu **Kaitkan**.

    | Pengaturan | Nilai |
    | -- | -- |
    | Jaringan virtual | **CoreServicesVnet (az104-rg4)** |
    | Subnet | **SharedServicesSubnet** |

1. Klik **OK** untuk menyimpan asosiasi.

### Mengonfigurasi aturan keamanan masuk

1. **Di area Pengaturan**, pilih **Aturan** keamanan masuk.

1. Tinjau aturan masuk default. Perhatikan bahwa hanya jaringan virtual dan load balancer lain yang diizinkan mengakses.

1. Pilih **+ Tambah**.

1. Pada bilah **Tambahkan aturan** keamanan masuk, gunakan informasi berikut untuk menambahkan aturan port masuk, lalu pilih **Tambahkan**.

    | Pengaturan | Nilai |
    | -- | -- |
    | Sumber | **any** |
    | Source port ranges |  ***** |
    | Tujuan | **Kelompok keamanan aplikasi** |
    | Kelompok keamanan aplikasi tujuan | **asg-web** |
    | Service | **Kustom** (perhatikan pilihan Anda yang lain) |
    | Rentang port tujuan | **80,443** |
    | Protokol | **TCP** |
    | Tindakan
           | **Izinkan** |
    | Prioritas | **100** |
    | Nama | **AllowASG** |

1. Setelah membuat aturan NSG Anda, luangkan waktu satu menit untuk meninjau aturan** keamanan Keluar default**.

## Poin penting

Selamat atas penyelesaian lab. Berikut adalah takeaway utama untuk lab ini. 

+ Jaringan virtual adalah representasi jaringan Anda sendiri di cloud. 
+ Saat merancang jaringan virtual, ini adalah praktik yang baik untuk menghindari rentang alamat IP yang tumpang tindih. Ini akan mengurangi masalah dan menyederhanakan pemecahan masalah.
+ Subnet adalah rangkaian alamat IP di jaringan virtual. Anda dapat membagi jaringan virtual menjadi beberapa subnet untuk organisasi dan keamanan.
+ Kelompok keamanan jaringan berisi aturan keamanan yang mengizinkan atau menolak lalu lintas jaringan. Ada aturan masuk dan keluar default yang dapat Anda sesuaikan dengan kebutuhan Anda.
+ Grup keamanan aplikasi digunakan untuk melindungi grup server dengan fungsi umum, seperti server web atau server database. 

## Membersihkan sumber daya Anda

Jika Anda bekerja dengan langganan Anda sendiri membutuhkan waktu satu menit untuk menghapus sumber daya lab. Ini akan memastikan sumber daya dibebankan dan biaya diminimalkan. Cara term mudah untuk menghapus sumber daya lab adalah dengan menghapus grup sumber daya lab. 

+ Di portal Azure, pilih grup sumber daya, pilih **Hapus grup** sumber daya, **Masukkan nama** grup sumber daya, lalu klik **Hapus**.
+ Menggunakan Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Menggunakan CLI, `az group delete --name resourceGroupName`.
