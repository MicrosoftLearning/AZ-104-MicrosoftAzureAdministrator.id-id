---
lab:
  title: 'Lab 05: Mengimplementasikan Konektivitas Antarsitus'
  module: Administer Intersite Connectivity
---

# Lab 05 - Menerapkan Konektivitas Antar Situs

## Pengantar lab

Di lab ini Anda menjelajahi komunikasi antara jaringan virtual. Anda mengimplementasikan peering jaringan virtual dan menguji koneksi. Anda juga akan membuat rute kustom. 

Lab ini memerlukan langganan Azure. Tipe langganan Anda dapat memengaruhi ketersediaan fitur di lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkah dalam lab ini ditulis menggunakan **US Timur**. 

## Perkiraan waktu: 50 menit
    
## Skenario lab 

Organisasi Anda membuat segmentasi aplikasi dan layanan TI inti (seperti DNS dan layanan keamanan) dari bagian lain bisnis, termasuk departemen manufaktur Anda. Namun, dalam beberapa skenario, aplikasi dan layanan di area inti perlu berkomunikasi dengan aplikasi dan layanan di area manufaktur. Di lab ini, Anda mengonfigurasi konektivitas antara area tersegmentasi. Ini adalah skenario umum untuk memisahkan produksi dengan pengembangan atau memisahkan satu anak perusahaan dengan anak perusahaan lainnya.  

## Diagram arsitektur

![Diagram arsitektur Lab 05](../media/az104-lab05-architecture.png)

## Keterampilan pekerjaan

+ Tugas 1: Membuat mesin virtual di jaringan virtual.
+ Tugas 2: Membuat mesin virtual di jaringan virtual yang berbeda.
+ Tugas 3: Menggunakan Network Watcher untuk menguji koneksi antara mesin virtual. 
+ Tugas 4: Mengonfigurasikan peering jaringan virtual antara jaringan virtual yang berbeda.
+ Tugas 5: Menggunakan Azure PowerShell untuk menguji koneksi antara mesin virtual.
+ Tugas 6: Membuat rute kustom. 

## Tugas 1:  Membuat mesin virtual dan jaringan virtual layanan inti

Dalam tugas ini, Anda membuat jaringan virtual layanan inti dengan mesin virtual. 

1. Masuk ke **portal Azure** - `https://portal.azure.com`.

1. Cari dan pilih `Virtual Machines`.

1. Dari halaman mesin virtual, pilih **Buat**, lalu pilih **Mesin Virtual Azure**.

1. Di tab Dasar, gunakan informasi berikut untuk melengkapi formulir, lalu pilih **Berikutnya: Disk >**. Untuk pengaturan yang tidak ditentukan, biarkan nilai default.
 
    | Pengaturan | Nilai | 
    | --- | --- |
    | Langganan |  *langganan Anda* |
    | Grup sumber daya |  `az104-rg5` (jika perlu, **Buat baru**. )
    | Nama komputer virtual |    `CoreServicesVM` |
    | Wilayah | **(AS) AS Timur** |
    | Opsi ketersediaan | Tidak ada redundansi infrastruktur yang diperlukan |
    | Jenis keamanan | **Standard**
           |
    | Gambar | **Pusat Data Windows Server 2019: x64 Gen2** (perhatikan pilihan Anda yang lain) |
    | Ukuran | **Standard_DS2_v3** |
    | Nama Pengguna | `localadmin` | 
    | Kata sandi | **Berikan kata sandi yang kompleks** |
    | Port masuk publik | **Tidak** |

    ![Cuplikan layar halaman pembuatan mesin virtual Dasar. ](../media/az104-lab05-createcorevm.png)
   
1. Di tab **Disk** gunakan default lalu pilih **Berikutnya: Jaringan >**.

1. Di tab **Jaringan**, untuk Jaringan virtual, pilih **Buat baru**.

1. Gunakan informasi berikut untuk mengonfigurasi jaringan virtual, lalu pilih **OK**. Jika perlu, hapus atau ganti informasi yang ada.

    | Pengaturan | Nilai | 
    | --- | --- |
    | Nama | `CoreServicesVnet` (Buat baru) |
    | Rentang alamat | `10.0.0.0/16`  |
    | Nama Subnet | `Core` | 
    | Rentang alamat subnet | `10.0.0.0/24` |

1. Pilih tab **Pemantauan**. Untuk Diagnostik Boot, pilih **Nonaktifkan**.

1. Pilih **Tinjauan + Buat**, kemudian pilih **Buat**.

1. Anda tidak perlu menunggu hingga sumber daya dibuat. Lanjutkan ke tugas berikutnya.

    >**Catatan:** Apakah Anda melihat bahwa dalam tugas ini Anda membuat jaringan virtual saat Anda membuat mesin virtual? Anda juga dapat membuat infrastruktur jaringan virtual lalu menambahkan mesin virtual. 

## Tugas 2: Membuat mesin virtual di jaringan virtual yang berbeda

Dalam tugas ini, Anda membuat jaringan virtual layanan manufaktur dengan mesin virtual. 

1. Di portal Microsoft Azure, cari dan navigasi ke **Virtual Machines.**

1. Dari halaman mesin virtual, pilih **Buat**, lalu pilih **Mesin Virtual Azure**.

1. Di tab Dasar, gunakan informasi berikut untuk melengkapi formulir, lalu pilih **Berikutnya: Disk >**. Untuk pengaturan yang tidak ditentukan, biarkan nilai default.
 
    | Pengaturan | Nilai | 
    | --- | --- |
    | Langganan |  *langganan Anda* |
    | Grup sumber daya |  `az104-rg5` |
    | Nama komputer virtual |    `ManufacturingVM` |
    | Wilayah | **(AS) AS Timur** |
    | Jenis keamanan | **Standard**
           |
    | Opsi ketersediaan | Tidak ada redundansi infrastruktur yang diperlukan |
    | Gambar | **Pusat Data Windows Server 2019: x64 Gen2** |
    | Ukuran | **Standard_DS2_v3** | 
    | Nama Pengguna | `localadmin` | 
    | Kata sandi | **Berikan kata sandi yang kompleks** |
    | Port masuk publik | **Tidak** |

1. Di tab **Disk** gunakan default lalu pilih **Berikutnya: Jaringan >**.

1. Di tab Jaringan, untuk Jaringan virtual, pilih **Buat baru**.

1. Gunakan informasi berikut untuk mengonfigurasi jaringan virtual, lalu pilih **OK**.  Jika perlu, hapus atau ganti rentang alamat yang ada.

    | Pengaturan | Nilai | 
    | --- | --- |
    | Nama | `ManufacturingVnet` |
    | Rentang alamat | `172.16.0.0/16`  |
    | Nama Subnet | `Manufacturing` |
    | Rentang alamat subnet | `172.16.0.0/24` |

1. Pilih tab **Pemantauan**. Untuk Diagnostik Boot, pilih **Nonaktifkan**.

1. Pilih **Tinjauan + Buat**, kemudian pilih **Buat**.

## Tugas 3: Menggunakan Network Watcher untuk menguji koneksi antara mesin virtual 


Dalam tugas ini, Anda memverifikasi bahwa sumber daya di jaringan virtual yang di-peering dapat berkomunikasi satu sama lain. Network Watcher akan digunakan untuk menguji koneksi tersebut. Sebelum melanjutkan, pastikan kedua mesin virtual telah disebarkan dan sedang berjalan. 

1. Dari portal Azure, telusuri dan pilih `Network Watcher`.

1. Dari Network Watcher, di menu Alat diagnostik jaringan, pilih **Pemecahan masalah koneksi**.

1. Gunakan informasi berikut untuk menyelesaikan bidang di halaman **Pemecahan masalah koneksi**.

    | Bidang | Nilai | 
    | --- | --- |
    | Jenis sumber           | **Mesin virtual**   |
    | Komputer virtual       | **CoreServicesVM**    | 
    | Tipe tujuan      | **Mesin virtual**   |
    | Komputer virtual       | **ManufacturingVM**   | 
    | Versi IP Pilihan  | **Keduanya**              | 
    | Protokol              | **TCP**               |
    | Port tujuan      | `3389`                |  
    | Port Sumber           | *Kosong*         |
    | Pengujian diagnostik      | *Default*      |

    ![Portal Microsoft Azure menampilkan pengaturan Pemecahan Masalah Koneksi.](../media/az104-lab05-connection-troubleshoot.png)

1. Pilih **Jalankan pengujian diagnostik**.

    >**Catatan**: Mungkin perlu waktu beberapa menit untuk menampilkan hasil. Pilihan layar akan berwarna abu-abu saat hasilnya sedang dikumpulkan. Perhatikan **Pengujian konektivitas** menunjukkan **UnReachable**. Ini masuk akal karena mesin virtual berada di jaringan virtual yang berbeda. 

 
## Tugas 4: Mengonfigurasikan peering jaringan virtual antara jaringan virtual

Dalam tugas ini, Anda membuat peering jaringan virtual untuk mengaktifkan komunikasi antara sumber daya di jaringan virtual. 

1. Di portal Azure, pilih jaringan virtual `CoreServicesVnet`.

1. Di CoreServicesVnet, di bagian **Pengaturan**, pilih **Peering**.

1. Di CoreServicesVnet, di bawah Peering, pilih **+ Tambahkan**. Jika tidak ditentukan, ambil default. 

    | **Parameter**                                    | **Nilai**                             |
    | --------------------------------------------- | ------------------------------------- |                                
    | Nama tautan penyerekan                             | `CoreServicesVnet-to-ManufacturingVnet` |
    | Jaringan virtual    | **ManufacturingVM-net (az104-rg5)**  |
    | Izinkan ManufacturingVnet untuk mengakses CoreServicesVnet  | dipilih (default) |
    | Izinkan ManufacturingVnet menerima lalu lintas yang diteruskan dari CoreServicesVnet | dipilih  |
    | Nama tautan penyerekan                             | `ManufacturingVnet-to-CoreServicesVnet` |
    | Izinkan CoreServicesVnet mengakses jaringan virtual yang di-peering            | dipilih (default) |
    | Izinkan CoreServicesVnet menerima lalu lintas yang diteruskan dari jaringan virtual yang di-peering | dipilih |

4. Klik **Tambahkan**.

5. Di CoreServicesVnet, di bawah Peering, verifikasi bahwa **peering CoreServicesVnet-to-ManufacturingVnet** tercantum. Refresh halaman untuk memastikan **Status peering** adalah **Terhubung**.

6. Beralih ke **ManufacturingVnet** dan verifikasi bahwa peering **ManufacturingVnet-to-CoreServicesVnet** tercantum. Pastikan bahwa **Status peering** adalah **Terhubung**. Anda mungkin perlu melakukan **Refresh** halamannya. 

## Tugas 5: Menggunakan Azure PowerShell untuk menguji koneksi antara mesin virtual

Dalam tugas ini, Anda menguji ulang koneksi antara mesin virtual di jaringan virtual yang berbeda. 

### Verifikasi alamat IP privat CoreServicesVM

1. Di portal Microsoft Azure, cari dan pilih mesin virtual `CoreServicesVM`.

1. Pada bilah **Gambaran Umum**, di bagian **Jaringan**, rekam **Alamat IP privat mesin**. Anda memerlukan informasi ini untuk menguji koneksi.
   
### Uji koneksi ke CoreServicesVM dari **ManufacturingVM**.

>**Tahukah Anda?** Ada banyak cara untuk memeriksa koneksi. Dalam tugas ini, Anda menggunakan **perintah Run**. Anda juga dapat terus menggunakan Network Watcher. Atau Anda dapat menggunakan [Koneksi Desktop Jauh](https://learn.microsoft.com/azure/virtual-machines/windows/connect-rdp#connect-to-the-virtual-machine) untuk mengakses mesin virtual. Setelah terhubung, gunakan **test-connection**. Karena Anda punya waktu, cobalah RDP. 

1. Beralihlah ke mesin virtual `ManufacturingVM`.

1. Di bilah **Operasi**, pilih bilah **perintah Run**.

1. Pilih **RunPowerShellScript** dan jalankan perintah **Test-NetConnection**. Pastikan untuk menggunakan alamat IP privat **CoreServicesVM**.

    ```Powershell
    Test-NetConnection <CoreServicesVM private IP address> -port 3389
    ```
1. Mungkin perlu waktu beberapa menit agar skrip kehabisan waktu. Bagian atas halaman menampilkan pesan informasi *Eksekusi Skrip sedang berlangsung.*

   
1. Uji koneksi harus berhasil karena peering telah dikonfigurasi. Nama komputer dan alamat jarak jauh Anda dalam gambar ini mungkin berbeda. 
   
   ![Jendela PowerShell dengan Test-NetConnection yang berhasil.](../media/az104-lab05-success.png)

## Tugas 6: Membuat rute kustom 

Dalam tugas ini, Anda ingin mengontrol lalu lintas jaringan antara subnet perimeter dan subnet layanan inti internal. Appliance jaringan virtual akan diinstal di subnet perimeter dan semua lalu lintas harus dirutekan di sana. 

1. Cari untuk memilih `CoreServicesVnet`.

1. Pilih **Subnet** lalu **+ Subnet**. Pastikan untuk memilih **Tambahkan** untuk menyimpan perubahan Anda. 

    | Pengaturan | Nilai | 
    | --- | --- |
    | Nama | `perimeter` |
    | Alamat awal | `10.0.1.0/24`  |

   
1. Di portal Azure, cari dan pilih `Route tables`, pilih **+ Buat**.

1. Masukkan detail berikut, pilih **Tinjau + Buat**, lalu pilih **Buat**. 

    | Pengaturan | Nilai | 
    | --- | --- |
    | Langganan | langganan Anda |
    | Grup sumber daya | `az104-rg5`  |
    | Wilayah | **US Timur** |
    | Nama | `rt-CoreServices` |
    | Merambat rute gateway | **Tidak** |

1. Setelah tabel rute disebarkan, Cari dan pilih **Tabel** Rute.
   
1. Pilih sumber daya (bukan kotak centang) **rt-CoreServices**

1. Perluas **Pengaturan lalu pilih **Rute** lalu **Tambahkan****. Buat rute dari Network Virtual Appliance (NVA) di masa mendatang ke jaringan virtual CoreServices. 

    | Pengaturan | Nilai | 
    | --- | --- |
    | Nama rute | `PerimetertoCore` |
    | Tipe tujuan | **Alamat IP** |
    | Alamat IP tujuan | `10.0.0.0/16` (jaringan virtual layanan inti) |
    | Jenis hop berikutnya | **Appliance virtual** (perhatikan pilihan Anda yang lain) |
    | Alamat lompatan berikutnya | `10.0.1.7` (NVA mendatang) |

1. Pilih **+ Tambahkan**. Hal terakhir yang harus dilakukan adalah mengaitkan rute dengan subnet.

1. Pilih **Subnet** lalu **+ Kaitkan**. Selesaikan konfigurasinya.

    | Pengaturan | Nilai | 
    | --- | --- |
    | Jaringan virtual | **CoreServicesVnet** |
    | Subnet | **Core** |    

>**Catatan**: Anda telah membuat rute yang ditentukan pengguna untuk mengarahkan lalu lintas dari DMZ ke NVA baru.  

## Bersihkan sumber daya Anda

Jika Anda bekerja dengan **langganan Anda sendiri** luangkan waktu sebentar untuk menghapus sumber daya lab. Hal ini akan memastikan sumber daya dikosongkan dan biaya diminimalkan. Cara termudah untuk menghapus sumber daya lab adalah dengan menghapus grup sumber daya lab. 

+ Di portal Microsoft Azure, pilih grup sumber daya, pilih **Hapus grup sumber daya**, **Masukkan nama grup sumber daya**, lalu klik **Hapus**.
+ Menggunakan Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Menggunakan CLI, `az group delete --name resourceGroupName`.

## Perluas pemelajaran Anda dengan Copilot
Copilot dapat membantu Anda mempelajari cara menggunakan alat pembuatan skrip Azure. Copilot juga dapat membantu di area yang tidak tercakup dalam lab atau ketika Anda memerlukan informasi lebih lanjut. Buka browser Edge dan pilih Copilot (kanan atas) atau navigasi *copilot.microsoft.com*. Luangkan beberapa menit untuk mencoba perintah ini.

+ Bagaimana cara menggunakan perintah Azure PowerShell atau Azure CLI untuk menambahkan peering jaringan virtual antara vnet1 dan vnet2?
+ Buat tabel yang menyoroti berbagai alat pemantauan Azure dan pihak ke-3 yang didukung di Azure. Sorot kapan harus menggunakan setiap alat. 
+ Kapan saya perlu membuat rute jaringan kustom di Azure?

## Pelajari lebih lanjut dengan pelatihan mandiri

+ [Distribusikan layanan Anda di seluruh jaringan virtual Azure dan integrasikan menggunakan peering jaringan virtual](https://learn.microsoft.com/en-us/training/modules/integrate-vnets-with-vnet-peering/). Menggunakan peering jaringan virtual untuk memungkinkan komunikasi di seluruh jaringan virtual dengan cara yang aman dan tingkat kerumitan yang kecil.
+ [Kelola dan kontrol arus lalu lintas dalam penyebaran Azure Anda dengan rute](https://learn.microsoft.com/training/modules/control-network-traffic-flow-with-routes/). Pelajari cara mengontrol lalu lintas jaringan virtual Azure dengan menerapkan rute kustom.


## Poin penting

Selamat atas penyelesaian lab ini. Berikut adalah kesimpulan utama lab ini. 

+ Secara default, sumber daya di jaringan virtual yang berbeda tidak dapat berkomunikasi.
+ Peering jaringan virtual memungkinkan Anda untuk menghubungkan dua Virtual Network atau lebih di Azure tanpa hambatan.
+ Jaringan virtual yang di-peering terlihat satu untuk tujuan konektivitas.
+ Lalu lintas antara mesin virtual di jaringan virtual yang di-peering menggunakan infrastruktur backbone Microsoft.
+ Rute yang ditentukan sistem secara otomatis dibuat untuk setiap subnet dalam jaringan virtual. Rute yang ditentukan pengguna mengambil alih atau menambahkan ke rute sistem default. 
+ Azure Network Watcher menyediakan serangkaian alat untuk memantau, mendiagnosis, serta melihat metrik dan log untuk sumber daya Azure IaaS.
