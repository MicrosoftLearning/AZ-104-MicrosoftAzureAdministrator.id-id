---
lab:
  title: 'Lab 05: Menerapkan intersite Koneksi ivity'
  module: Administer Intersite Connectivity
---

# Lab 05 - Menerapkan Konektivitas Antar Situs

## Pengenalan lab

Di lab ini Anda akan menjelajahi komunikasi antara jaringan virtual. Anda akan menerapkan peering jaringan virtual dan menjalankan perintah jarak jauh untuk menguji koneksi.   

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur di lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkahnya ditulis menggunakan **US** Timur. 

## Perkiraan waktu: 30 menit

## Skenario lab 

Organisasi Anda mengegmentasi aplikasi dan layanan IT inti (seperti DNS dan layanan keamanan) dari bagian lain dari bisnis, termasuk departemen manufaktur Anda. Namun, dalam beberapa skenario, aplikasi dan layanan di area inti perlu berkomunikasi dengan aplikasi dan layanan di area manufaktur. Di lab ini, Anda mengonfigurasi konektivitas antara area tersegmentasi. Ini adalah skenario umum untuk memisahkan produksi dari pengembangan atau memisahkan satu anak perusahaan dari anak perusahaan lainnya.

## Simulasi lab interaktif

Ada beberapa simulasi lab interaktif yang mungkin berguna bagi Anda untuk topik ini. Simulasi ini memungkinkan Anda mengklik skenario serupa dengan kecepatan Anda sendiri. Ada perbedaan antara simulasi interaktif dan lab ini, tetapi banyak konsep intinya sama. Langganan Azure tidak diperlukan. 

+ [Koneksi dua jaringan virtual Azure menggunakan peering](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Connect%20two%20Azure%20virtual%20networks%20using%20global%20virtual%20network%20peering) jaringan virtual global. Uji koneksi antara dua komputer virtual di jaringan virtual yang berbeda. Buat peering jaringan virtual dan coba lagi.
+ [Menerapkan konektivitas](https://mslabs.cloudguides.com/en-us/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%209) antarsitus. Jalankan templat untuk membuat infrastruktur jaringan virtual dengan beberapa komputer virtual. Konfigurasikan peering jaringan virtual dan uji koneksi. 

## Diagram arsitektur

![Diagram arsitektur Lab 05](../media/az104-lab05-architecture-diagram.png)

## Tugas

+ Tugas 1: Buat komputer virtual layanan inti dan jaringan virtual.
+ Tugas 2: Buat mesin virtual dan jaringan virtual layanan manufaktur.
+ Tugas 3: Uji koneksi antara komputer virtual. 
+ Tugas 4: Buat peering VNet antara jaringan virtual. 
+ Tugas 5: Coba lagi koneksi antara komputer virtual. 
 

## Tugas 1: Membuat komputer virtual layanan inti dan jaringan virtual

Dalam tugas ini, kami membuat jaringan virtual layanan inti dengan komputer virtual. 

1. Masuk ke **portal Azure** - `https://portal.azure.com`.

1. Cari dan pilih `Virtual Machines`.

1. Dari halaman komputer virtual, pilih **Buat lalu pilih **Azure Virtual Machine****.

1. Pada tab Dasar, gunakan informasi berikut untuk melengkapi formulir, lalu pilih **Berikutnya: Disk >**. Untuk pengaturan apa pun yang tidak ditentukan, biarkan nilai default.
 
    | Pengaturan | Nilai | 
    | --- | --- |
    | Langganan |  *langganan Anda* |
    | Grup sumber daya |  `az104-rg5` (Jika perlu, pilih **Buat baru**. Gunakan grup ini untuk semua sumber daya lab Anda.)
    | Nama komputer virtual |    `CoreServicesVM` |
    | Wilayah | **US Timur** |
    | Opsi ketersediaan | Tidak ada redundansi infrastruktur yang diperlukan |
    | Gambar | **Pusat Data Windows Server 2019: x64 Gen2** |
    | Ukuran | **Standard_DS2_v3** |
    | Nama Pengguna | `localadmin` | 
    | Kata sandi | **Berikan kata sandi yang kompleks** |

    ![Cuplikan layar halaman pembuatan komputer virtual Dasar. ](../media/az104-lab05-createcorevm.png)
   
1. Pada tab Disk, atur jenis disk OS ke **HDD** Standar, lalu pilih **Berikutnya: Jaringan >**.

1. Pada tab Jaringan, untuk Jaringan virtual, pilih **Buat baru**.

1. Gunakan informasi berikut untuk mengonfigurasi jaringan virtual, lalu pilih **Ok**. Jika perlu, hapus atau ganti rentang alamat yang ada.

    | Pengaturan | Nilai | 
    | --- | --- |
    | Nama | `CoreServicesVNet` (Buat baru) |
    | Ruang alamat | `10.0.0.0/16`  |
    | Nama Subnet | `Core` | 
    | Rentang alamat subnet | `10.0.0.0/24` |

1. Pilih tab **Pemantauan** . Untuk Diagnostik Boot, pilih **Nonaktifkan**.

1. Pilih **Tinjauan + Buat**, kemudian pilih **Buat**.

1. Anda tidak perlu menunggu sumber daya dibuat. Lanjutkan ke tugas berikutnya.

    >**Catatan:** Apakah Anda melihat dalam tugas ini Anda membuat jaringan virtual saat membuat komputer virtual? 

## Tugas 2: Membuat mesin virtual dan jaringan virtual layanan manufaktur

Dalam tugas ini, kami membuat jaringan virtual layanan manufaktur dengan komputer virtual. 

1. Dari portal Azure, cari dan navigasi ke **Virtual Machines**.

1. Dari halaman komputer virtual, pilih **Buat lalu pilih **Azure Virtual Machine****.

1. Pada tab Dasar, gunakan informasi berikut untuk melengkapi formulir, lalu pilih **Berikutnya: Disk >**. Untuk pengaturan apa pun yang tidak ditentukan, biarkan nilai default.
 
    | Pengaturan | Nilai | 
    | --- | --- |
    | Langganan |  *langganan Anda* |
    | Grup sumber daya |  `az104-rg5` |
    | Nama komputer virtual |    `ManufacturingVM` |
    | Wilayah | **US Timur** |
    | Opsi ketersediaan | Tidak ada redundansi infrastruktur yang diperlukan |
    | Gambar | **Pusat Data Windows Server 2019: x64 Gen2** |
    | Ukuran | **Standard_DS2_v3** | 
    | Nama Pengguna | `localadmin` | 
    | Kata sandi | **Berikan kata sandi yang kompleks** |

1. Pada tab Disk, atur jenis disk OS ke **HDD** Standar, lalu pilih **Berikutnya: Jaringan >**.

1. Pada tab Jaringan, untuk Jaringan virtual, pilih **Buat baru**.

1. Gunakan informasi berikut untuk mengonfigurasi jaringan virtual, lalu pilih **Ok**.  Jika perlu, hapus atau ganti rentang alamat yang ada.

    | Pengaturan | Nilai | 
    | --- | --- |
    | Nama | `ManufacturingVNet` |
    | Ruang alamat | `172.16.0.0/16`  |
    | Nama Subnet | `Manufacturing` |
    | Rentang alamat subnet | `172.16.0.0/24` |

1. Pilih tab **Pemantauan** . Untuk Diagnostik Boot, pilih **Nonaktifkan**.

1. Pilih **Tinjauan + Buat**, kemudian pilih **Buat**.

## Tugas 3: Uji koneksi antara komputer virtual

Dalam tugas ini, Anda menguji koneksi antara komputer virtual di jaringan virtual yang berbeda.

### Memverifikasi alamat IP privat CoreServicesVM

1. Dari portal Azure, cari dan pilih `CoreServicesVM` komputer virtual.

1. Pada bilah **Gambaran Umum** , di bagian **Jaringan** , rekam **alamat** IP Privat komputer. Anda memerlukan informasi ini untuk menguji koneksi.
   
### Uji koneksi ke CoreServicesVM dari **ManufacturingVM**.

1. Di portal, pilih dan pilih komputer `ManufacturingVM` virtual.

1. Di bagian **Operasi** , pilih bilah **Jalankan perintah** .

1. Pilih **RunPowerShellScript** dan jalankan **perintah Test-Net Koneksi ion**. Pastikan untuk menggunakan alamat **IP privat CoreServicesVM**.

   ```Powershell
    Test-NetConnection <CoreServicesVM private IP address> -port 3389
   ```
   
1. Mungkin perlu beberapa menit agar skrip berjalan. Bagian atas halaman memperlihatkan eksekusi Skrip pesan *informasi yang sedang berlangsung.*
   
1. Koneksi pengujian harus gagal. Komputer virtual di jaringan virtual yang berbeda harus, secara default, tidak dapat berkomunikasi.
   
   ![Jendela PowerShell dengan Test-Net Koneksi ion gagal.](../media/az104-lab05-fail.png)

 
## Tugas 4: Membuat peering VNet di antara jaringan virtual

Dalam tugas ini, Anda membuat peering jaringan virtual untuk mengaktifkan komunikasi antar VNet.

1. Di portal Azure, pilih **Virtual Networks**, lalu pilih **CoreServicesVnet**.

1. Di CoreServicesVnet, di bagian **Pengaturan**, pilih **Peering**.

1. Di CoreServicesVnet | Peering, pilih **+ Tambahkan**.

1. Gunakan informasi dalam tabel berikut untuk membuat peering.

    | **Bagian**                          | **Opsi**                                    | **Nilai**                             |
    | ------------------------------------ | --------------------------------------------- | ------------------------------------- |
    | Jaringan virtual ini                 |                                               |                                       |
    |                                      | Nama tautan penyerekan                             | `CoreServicesVnet-to-ManufacturingVnet` |
    |                                      | Izinkan Izinkan CoreServicesVNet mengakses jaringan virtual yang di-peering            | dipilih (default)                       |
    |                                      | Izinkan CoreServicesVNet menerima lalu lintas yang diteruskan dari jaringan virtual yang di-peering | dipilih                       |
    |                                      | Aktifkan CoreServicesVNet untuk menggunakan gateway jarak jauh jaringan virtual yang di-peering       | Tidak dipilih (default)                        |
    | Jaringan virtual jarak jauh               |                                               |                                       |
    |                                      | Nama tautan penyerekan                             | `ManufacturingVnet-to-CoreServicesVnet` |
    |                                      | Model penyebaran jaringan virtual              | **Manajer sumber daya**                      |
    |                                      | Saya mengetahui ID sumber daya saya                         | Tidak dipilih                          |
    |                                      | Langganan                                  | *langganan Anda*    |
    |                                      | Jaringan virtual                               | **ManufacturingVnet**                     |
    |                                      | Izinkan ManufacturingVNet mengakses CoreServicesVNet  | dipilih (default)                       |
    |                                      | Izinkan ManufacturingVNet menerima lalu lintas yang diteruskan dari CoreServicesVNet | dipilih                        |
    |                                      | Mengaktifkan ManufacturingVNet untuk menggunakan gateway jarak jauh CoreServicesVNet       | Tidak dipilih (default)                        |

1. Tinjau pengaturan Anda dan pilih **Tambahkan**.

    ![Cuplikan layar halaman peering.](../media/az104-lab05-peering.png)
 
1. Di CoreServicesVnet | Peering, verifikasi bahwa peering **CoreServicesVnet-to-ManufacturingVnet** dicantumkan. Refresh halaman untuk memastikan **status** **Peering Koneksi**.

1. Beralih ke **ManufacturingVnet** dan verifikasi **peering ManufacturingVnet-to-CoreServicesVnet** tercantum. **Pastikan status** **Peering Koneksi**.

 
## Tugas 5: Uji koneksi antara VM

Dalam tugas ini, Anda memverifikasi komputer virtual di jaringan virtual yang berbeda dapat berkomunikasi satu sama lain.

1. Cari dan pilih **ManufacturingVM**.

1. Di bagian **Operasi** , pilih bilah **Jalankan perintah** .

1. Pilih **RunPowerShellScript** dan tambahkan perintah Test-Net Koneksi ion. Pastikan untuk menggunakan alamat **IP privat CoreServicesVM**.

      ```Powershell
     Test-NetConnection <CoreServicesVM private IP address> -port 3389
      ```

1. Mungkin perlu beberapa menit agar skrip berjalan. Bagian atas halaman memperlihatkan eksekusi Skrip ikon *informasi yang sedang berlangsung.*
    
1. Koneksi pengujian harus berhasil. 
   ![Jendela Powershell dengan Test-Net Koneksi ion berhasil](../media/az104-lab05-success.png)


## Tinjau titik utama lab

Selamat atas penyelesaian lab. Berikut adalah takeaway utama untuk lab ini. 

+ Secara default, sumber daya di jaringan virtual yang berbeda tidak dapat berkomunikasi.
+ Peering jaringan virtual memungkinkan Anda menyambungkan dua jaringan virtual atau lebih dengan lancar di Azure.
+ Jaringan virtual yang di-peering muncul sebagai satu untuk tujuan konektivitas.
+ Lalu lintas antara mesin virtual di jaringan virtual yang di-peering menggunakan infrastruktur backbone Microsoft.

## Membersihkan sumber daya Anda

Jika Anda bekerja dengan langganan Anda sendiri membutuhkan waktu satu menit untuk menghapus sumber daya lab. Ini akan memastikan sumber daya dibebankan dan biaya diminimalkan. Cara term mudah untuk menghapus sumber daya lab adalah dengan menghapus grup sumber daya lab. 

+ Di portal Azure, pilih grup sumber daya, pilih **Hapus grup** sumber daya, **Masukkan nama** grup sumber daya, lalu klik **Hapus**.
+ Menggunakan Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Menggunakan CLI, `az group delete --name resourceGroupName`.

