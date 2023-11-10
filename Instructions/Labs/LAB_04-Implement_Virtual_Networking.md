---
lab:
  title: 'Lab 04: Menerapkan Virtual Networking'
  module: Administer Virtual Networking
---

# Lab 04 - Menerapkan Virtual Network

# Panduan lab siswa

## Skenario laboratorium

Anda perlu menjelajahi kemampuan jaringan virtual Azure. Untuk memulai, Anda berencana membuat jaringan virtual di Azure yang akan menghosting beberapa mesin virtual Azure. Karena Anda bermaksud menerapkan segmentasi berbasis jaringan, Anda akan menerapkannya ke subnet yang berbeda dari jaringan virtual. Anda juga ingin memastikan bahwa alamat IP pribadi dan publik mereka tidak akan berubah seiring waktu. Untuk mematuhi persyaratan keamanan Contoso, Anda perlu melindungi titik akhir publik dari mesin virtual Azure yang dapat diakses dari internet. Terakhir, Anda perlu menerapkan resolusi nama DNS untuk mesin virtual Azure baik di dalam jaringan virtual maupun dari Internet.

**Catatan:** Tersedia **[simulasi lab interaktif](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%208)** yang memungkinkan Anda mengklik lab ini sesuai keinginan Anda. Anda mungkin menemukan sedikit perbedaan antara simulasi interaktif dan lab yang dihosting, tetapi konsep dan ide utama yang ditunjukkan sama. 

## Tujuan

Di lab ini Anda akan:

+ Tugas 1: Membuat dan mengonfigurasi jaringan virtual
+ Tugas 2: Menyebarkan komputer virtual ke jaringan virtual
+ Tugas 3: Mengonfigurasi alamat IP privat dan publik Azure VM
+ Tugas 4: Mengonfigurasi grup keamanan jaringan
+ Tugas 5: Mengonfigurasi Azure DNS untuk resolusi nama internal
+ Tugas 6: Mengonfigurasi Azure DNS untuk resolusi nama eksternal

## Perkiraan waktu: 40 menit

## Diagram arsitektur

![gambar](../media/lab04.png)

### Petunjuk

## Latihan 1

## Tugas 1: Membuat dan mengonfigurasi jaringan virtual

Dalam tugas ini, Anda akan membuat jaringan virtual dengan beberapa subnet menggunakan portal Microsoft Azure

1. Masuk ke [portal Azure](https://portal.azure.com).

1. Di portal Microsoft Azure, cari dan pilih **Jaringan virtual**, dan, pada panel **Jaringan virtual**, klik **+ Buat**.

1. Buat jaringan virtual dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang akan Anda gunakan di lab ini |
    | Grup Sumber Daya | nama grup sumber daya **baru** **az104-04-rg1** |
    | Nama | **az104-04-vnet1** |
    | Wilayah | nama wilayah Azure yang tersedia dalam langganan yang akan Anda gunakan di lab ini |

1. Klik **Berikutnya: Alamat IP**. Alamat **** awal adalah **10.40.0.0**. Ukuran **** ruang Alamat adalah **/20**. Pastikan untuk mengklik **Tambahkan**. 

1. Klik **+ Tambahkan subnet** masukkan nilai berikut lalu klik **Tambahkan**.

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama subnet | **subnet0** |
    | Alamat awal | **10.40.0.0** |
    | Ukuran subnet | **/24 (256 alamat)** |

1. Terima defaultnya dan klik **Tinjau dan Buat**. Biarkan validasi berjalan, dan tekan **Buat** lagi untuk mengirimkan penyebaran Anda.

    >**Catatan:** Tunggu hingga jaringan virtual diprovisikan. Ini seharusnya memakan waktu kurang dari satu menit.

1. Klik **Buka sumber daya**

1. Pada panel jaringan virtual **az104-04-vnet1**, klik **Subnet**, lalu klik **+ Subnet**.

1. Buat subnet dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **subnet1** |
    | Rentang alamat (blok CIDR) | **10.40.1.0/24** |
    | Grup keamanan jaringan | **Tidak ada** |
    | Tabel rute | **Tidak ada** |

1. Klik **Simpan**

## Tugas 2: Menyebarkan komputer virtual ke jaringan virtual

Dalam tugas ini, Anda akan menyebarkan mesin virtual Azure ke subnet yang berbeda dari jaringan virtual dengan menggunakan template ARM

1. Di portal Microsoft Azure, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**.

    >**Catatan**: Jika ini adalah pertama kalinya Anda memulai **Cloud Shell** dan Anda disajikan dengan **pesan Anda tidak memiliki penyimpanan yang dipasang** , pilih langganan yang Anda gunakan di lab ini, dan klik **Buat penyimpanan**.

1. Di bilah alat panel Cloud Shell, klik ikon **Unggah/Unduh file**, lalu klik **Unggah** di menu dropdown. Unggah **\\Allfiles\\Labs\\04\\az104-04-vms-loop-template.json** and **\\Allfiles\\Labs\\04\\az104-04-vms-loop-parameters.json** ke direktori beranda Cloud Shell.

    >**Catatan **: Anda harus mengunggah setiap file secara terpisah. Setelah mengunggah, gunakan **dir** untuk memastikan kedua file berhasil diunggah.

1. Dari panel Cloud Shell, jalankan perintah berikut untuk menyebarkan dua mesin virtual menggunakan file template dan parameter:
    >**Catatan**: Anda akan diminta untuk memberikan kata sandi Admin.
    
   ```powershell
   $rgName = 'az104-04-rg1'

   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-04-vms-loop-template.json `
      -TemplateParameterFile $HOME/az104-04-vms-loop-parameters.json
   ```
   
    >**Catatan**: Metode penyebaran templat ARM ini menggunakan Azure PowerShell. Anda dapat melakukan tugas yang sama dengan menjalankan perintah Azure CLI yang setara **az deployment create** (untuk informasi selengkapnya, lihat [Menyebarkan sumber daya dengan template Resource Manager dan Azure CLI](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/deploy-cli).

    >**Catatan**: Tunggu hingga penyebaran selesai sebelum melanjutkan ke tugas berikutnya. Proses ini memerlukan waktu sekitar 2 menit.

    >**Catatan**: Jika Anda mendapatkan kesalahan yang menyatakan ukuran VM tidak tersedia, silakan minta bantuan instruktur Anda dan coba langkah-langkah berikut:
    > 1. Klik tombol `{}` di CloudShell Anda, pilih **az104-04-vms-loop-parameters.json** dari panel sisi kiri dan catat nilai parameter `vmSize`.
    > 1. Periksa lokasi di mana grup sumber daya 'az104-04-rg1' diterapkan. Anda dapat menjalankan `az group show -n az104-04-rg1 --query location` di CloudShell Anda untuk mendapatkannya.
    > 1. Jalankan `az vm list-skus --location <Replace with your location> -o table --query "[? contains(name,'Standard_D2s')].name"` di CloudShell Anda. Jika tidak ada SKU yang terdaftar (yaitu tidak ada hasil), maka Anda tidak dapat menyebarkan mesin virtual D2S di wilayah tersebut. Anda perlu menemukan wilayah yang memungkinkan Anda menggunakan mesin virtual D2S. Setelah memilih lokasi yang sesuai, hapus grup sumber daya AZ104-04-rg1 dan mulai ulang lab.
    > 1. Ganti nilai parameter `vmSize` dengan salah satu nilai yang dikembalikan oleh perintah yang baru saja Anda jalankan.
    > 1. Sekarang sebarkan ulang template Anda dengan menjalankan perintah `New-AzResourceGroupDeployment` lagi. Anda dapat menekan tombol atas beberapa kali yang akan membawa ke perintah yang dieksekusi terakhir.

1. Tutup panel Cloud Shell.

## Tugas 3: Mengonfigurasi alamat IP privat dan publik Azure VM

Dalam tugas ini, Anda akan mengonfigurasi penetapan statis alamat IP publik dan pribadi yang ditetapkan ke antarmuka jaringan mesin virtual Azure.

   >**Catatan**: Alamat IP privat dan publik benar-benar ditetapkan ke antarmuka jaringan, yang, pada gilirannya dilampirkan ke komputer virtual Azure, namun, cukup umum untuk merujuk ke alamat IP yang ditetapkan ke Azure VM sebagai gantinya.

   >**Catatan**: Anda akan memerlukan **dua** alamat IP publik untuk menyelesaikan lab ini. 

1. Di portal Azure, cari dan pilih **Alamat** IP publik, lalu pilih **+ Buat**.

1. **Pastikan grup** sumber daya adalah **az104-04-rg1**,

1. Dalam Detail** Konfigurasi pastikan **namanya** **adalah az104-04-pip0**.**

1. Pilih **Tinjau dan buat** lalu **Buat**.

1. Di portal Azure, cari dan pilih **Alamat** IP publik, lalu pilih **+ Buat**.

1. **Pastikan grup** sumber daya adalah **az104-04-rg1**,

1. Dalam Detail** Konfigurasi pastikan **namanya** **adalah az104-04-pip1**.**

1. Pilih **Tinjau dan buat** lalu **Buat**.

1. Di portal Microsoft Azure, cari dan pilih **Grup sumber daya**, dan, pada panel **Grup sumber daya**, klik **az104-04-rg1**.

1. Pada panel grup sumber daya **az104-04-rg1**, dalam daftar sumber dayanya, klik **az104-04-vnet1**.

1. Pada panel jaringan virtual **az104-04-vnet1**, tinjau bagian **Perangkat yang terhubung** dan verifikasi bahwa ada dua antarmuka jaringan **az104-04-nic0** dan **az104-04-nic1** terpasang ke jaringan virtual.

1. Klik **az104-04-nic0** dan, pada panel **az104-04-nic0**, klik **Konfigurasi IP**.

    >**Catatan**: Verifikasi bahwa **ipconfig1** saat ini disiapkan dengan alamat IP privat dinamis.

1. Di daftar konfigurasi IP, klik **ipconfig1**.

1. Pastikan Alokasi** Statis****.**

1. Pilih **Kaitkan alamat** IP publik dan di **drop-down Alamat** IP publik pilih **az104-04-pip0**.

1. Pilih **Simpan**.

1. Navigasi kembali ke bilah **az104-04-vnet1** .

1. Klik **az104-04-nic1** dan, pada panel **az104-04-nic1**, klik **Konfigurasi IP**.

    >**Catatan**: Verifikasi bahwa **ipconfig1** saat ini disiapkan dengan alamat IP privat dinamis.

1. Di daftar konfigurasi IP, klik **ipconfig1**.

1. Pastikan Alokasi** Statis****.**

1. Pilih **Kaitkan alamat** IP publik dan di **drop-down Alamat** IP publik pilih **az104-04-pip1**.

1. Pilih **Simpan**.
   
1. Navigasikan kembali ke panel grup sumber daya **az104-04-rg1**, dalam daftar sumber dayanya, klik **az104-04-vm0**, dan dari panel mesin virtual **az104-04-vm0 **, perhatikan entri alamat IP publik.

1. Navigasikan kembali ke panel grup sumber daya **az104-04-rg1**, dalam daftar sumber dayanya, klik **az104-04-vm1**, dan dari panel mesin virtual **az104-04-vm1 **, perhatikan entri alamat IP publik.

    >**Catatan**: Anda akan memerlukan kedua alamat IP dalam tugas terakhir lab ini.

## Tugas 4: Mengonfigurasi grup keamanan jaringan

Dalam tugas ini, Anda akan mengonfigurasi kelompok keamanan jaringan untuk mengizinkan konektivitas terbatas ke mesin virtual Azure.

1. Di portal Microsoft Azure, navigasikan kembali ke panel grup sumber daya **az104-04-rg1**, dan dalam daftar sumber dayanya, klik **az104-04-vm0**.

1. Pada panel ringkasan **az104-04-vm0**, klik **Hubungkan**, klik **RDP** di menu dropdown, di panel **Hubungkan dengan RDP**, klik **Unduh File RDP** menggunakan alamat IP Publik dan ikuti petunjuk untuk memulai sesi Desktop Jarak Jauh.

1. Perhatikan bahwa upaya koneksi gagal.

    >**Catatan**: Ini diharapkan, karena alamat IP publik SKU Standar, secara default, mengharuskan antarmuka jaringan yang ditetapkan dilindungi oleh kelompok keamanan jaringan. Untuk mengizinkan koneksi Desktop Jarak Jauh, Anda akan membuat kelompok keamanan jaringan yang secara eksplisit mengizinkan lalu lintas RDP masuk dari Internet dan menetapkannya ke antarmuka jaringan kedua mesin virtual.

1. Hentikan mesin virtual **az104-04-vm0** dan **az104-04-vm1**.

    >**Catatan**: Ini dilakukan untuk kedaluwarsa lab. Jika mesin virtual berjalan saat kelompok keamanan jaringan dilampirkan ke antarmuka jaringan mereka, diperlukan waktu lebih dari 30 menit agar lampiran diterapkan. Setelah kelompok keamanan jaringan dibuat dan dilampirkan, mesin virtual akan dimulai ulang, dan lampiran akan segera diterapkan.

1. Di portal Microsoft Azure, cari dan pilih **Kelompok keamanan jaringan**, dan, pada panel **Kelompok keamanan jaringan**, klik **+ Buat**.

1. Buat kelompok keamanan jaringan dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup Sumber Daya | **az104-04-rg1** |
    | Nama | **az104-04-nsg01** |
    | Wilayah | nama wilayah Azure tempat Anda menerapkan semua sumber daya lainnya di lab ini |

1. Klik **Tinjau dan Buat**. Biarkan validasi berjalan, dan tekan **Buat** untuk mengirimkan penyebaran Anda.

    >**Catatan**: Tunggu hingga penyebaran selesai. Proses ini memerlukan waktu sekitar 2 menit.

1. Pada panel penyebaran, klik **Buka sumber daya** untuk membuka panel kelompok keamanan jaringan **az104-04-nsg01**.

1. Pada panel kelompok keamanan jaringan **az104-04-nsg01**, di bagian **Pengaturan**, klik **Aturan keamanan masuk**.

1. Tambahkan aturan masuk dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Sumber | **Mana pun** |
    | Source port ranges | * |
    | Tujuan | **Mana pun** |
    | Layanan | **RDP** |
    | Tindakan
           | **Izinkan** |
    | Prioritas | **300** |
    | Nama | **AllowRDPInBound** |

1. Pada panel kelompok keamanan jaringan **az104-04-nsg01**, di bagian **Pengaturan**, klik **Antarmuka jaringan** lalu klik **+ Kaitkan**.

1. Kaitkan kelompok keamanan jaringan **az104-04-nsg01** dengan antarmuka jaringan **az104-04-nic0** dan **az104-04-nic1**.

    >**Catatan**: Mungkin perlu waktu hingga 5 menit agar aturan dari Kelompok Keamanan Jaringan yang baru dibuat diterapkan ke Kartu Antarmuka Jaringan.

1. Mulai mesin virtual **az104-04-vm0** dan **az104-04-vm1**.

1. Navigasikan kembali ke panel mesin virtual **az104-04-vm0**.

    >**Catatan**: Pada langkah-langkah berikutnya, Anda akan memverifikasi bahwa Anda dapat berhasil terhubung ke komputer virtual target.

1. Pada panel **az104-04-vm0**, klik **Hubungkan**, klik **RDP**, pada panel **Hubungkan dengan RDP**, klik **Unduh File RDP** menggunakan alamat IP Publik dan ikuti petunjuk untuk memulai sesi Desktop Jarak Jauh.

    >**Catatan**: Langkah ini mengacu pada menyambungkan melalui Desktop Jauh dari komputer Windows. Di Mac, Anda dapat menggunakan Klien Desktop Jauh dari Mac App Store dan di komputer Linux Anda dapat menggunakan perangkat lunak klien RDP sumber terbuka.

    >**Catatan**: Anda dapat mengabaikan perintah peringatan saat menyambungkan ke komputer virtual target.

1. Saat diminta, masuk dengan pengguna dan kata sandi.

    >**Catatan**: Biarkan sesi Desktop Jauh terbuka. Anda akan membutuhkannya dalam tugas berikutnya.

## Tugas 5: Mengonfigurasi Azure DNS untuk resolusi nama internal

Dalam tugas ini, Anda akan mengonfigurasi resolusi nama DNS dalam jaringan virtual dengan menggunakan zona DNS pribadi Azure.

1. Di portal Microsoft Azure, cari dan pilih **Zona DNS Pribadi** dan, pada panel **Zona DNS Pribadi**, klik **+ Buat**.

1. Buat zona DNS pribadi dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup Sumber Daya | **az104-04-rg1** |
    | Nama | **contoso.org** |

1. Klik **Tinjau dan Buat**. Biarkan validasi berjalan, dan tekan **Buat** lagi untuk mengirimkan penyebaran Anda.

    >**Catatan**: Tunggu hingga zona DNS privat dibuat. Proses ini memerlukan waktu sekitar 2 menit.

1. Klik **Buka sumber daya** untuk membuka panel zona pribadi DNS **contoso.org**.

1. Pada panel zona DNS pribadi **contoso.org**, di bagian **Pengaturan**, klik **Tautan jaringan virtual**

1. Klik **+ Tambah** untuk membuat tautan jaringan virtual dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama tautan | **az104-04-vnet1-link** |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Jaringan virtual | **az104-04-vnet1** |
    | Aktifkan pendaftaran otomatis | diaktifkan |

1. Klik **OK**.

    >**Catatan:** Tunggu hingga tautan jaringan virtual dibuat. Ini akan memerlukan waktu kurang dari 1 menit.

1. Pada panel zona DNS pribadi **contoso.org**, di panel sisi, klik **Ringkasan**

1. Verifikasi bahwa catatan DNS untuk **az104-04-vm0** dan **az104-04-vm1** muncul dalam daftar kumpulan catatan sebagai **Terdaftar otomatis**.

    >**Catatan:** Anda mungkin perlu menunggu beberapa menit dan me-refresh halaman jika kumpulan catatan tidak tercantum.

1. Beralih ke sesi Desktop Jarak Jauh ke **az104-04-vm0**, klik kanan tombol **Mulai** dan, di menu klik kanan, klik **Windows PowerShell (Admin)**.

1. Di jendela konsol Windows PowerShell, jalankan perintah berikut ini untuk menguji resolusi nama internal di zona DNS pribadi yang baru dibuat:

   ```powershell
   nslookup az104-04-vm0.contoso.org
   nslookup az104-04-vm1.contoso.org
   ```

1. Verifikasi bahwa output perintah menyertakan alamat IP pribadi **az104-04-vm1** (**10.40.1.4**).

## Tugas 6: Mengonfigurasi Azure DNS untuk resolusi nama eksternal

Dalam tugas ini, Anda akan mengonfigurasi resolusi nama DNS eksternal dengan menggunakan zona DNS publik Azure.

1. Di browser web, buka tab baru dan navigasikan ke <https://www.godaddy.com/domains/domain-name-search>.

1. Gunakan pencarian nama domain untuk mengidentifikasi nama domain yang tidak digunakan.

1. Di portal Microsoft Azure, cari dan pilih **zona DNS** dan, pada panel **zona DNS**, klik **+ Buat**.

1. Buat zona DNS dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup Sumber Daya | **az104-04-rg1** |
    | Nama | nama domain DNS yang Anda identifikasi sebelumnya dalam tugas ini |

1. Klik **Tinjau dan Buat**. Biarkan validasi berjalan, dan tekan **Buat** lagi untuk mengirimkan penyebaran Anda.

    >**Catatan**: Tunggu hingga zona DNS dibuat. Proses ini memerlukan waktu sekitar 2 menit.

1. Klik **Buka sumber daya** untuk membuka panel zona DNS yang baru dibuat.

1. Pada panel zona DNS, klik **+ Kumpulan catatan**.

1. Tambahkan kumpulan catatan dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **az104-04-vm0** |
    | Jenis | **A**
           |
    | Kumpulan catatan alias | **Tidak** |
    | TTL | **1** |
    | Unit TTL | **Jam** |
    | Alamat IP | alamat IP publik **az104-04-vm0** yang Anda identifikasi dalam latihan ketiga lab ini |

1. Klik **OK**

1. Pada panel zona DNS, klik **+ Kumpulan catatan**.

1. Tambahkan kumpulan catatan dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **az104-04-vm1** |
    | Jenis | **A**
           |
    | Kumpulan catatan alias | **Tidak** |
    | TTL | **1** |
    | Unit TTL | **Jam** |
    | Alamat IP | alamat IP publik **az104-04-vm1** yang Anda identifikasi dalam latihan ketiga lab ini |

1. Klik **OK**

1. Pada panel zona DNS, catat nama entri **Name server 1**.

1. Di portal Azure, buka sesi **PowerShell** di **Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Dari panel Cloud Shell, jalankan yang berikut ini untuk menguji resolusi **nama eksternal dari kumpulan catatan DNS az104-04-vm0** di zona DNS yang baru dibuat (ganti tempat penampung `[Name server 1]` dengan nama **server Nama 1 yang** Anda catat sebelumnya dalam tugas ini dan `[domain name]` tempat penampung dengan nama domain DNS yang Anda buat sebelumnya dalam tugas ini):

   ```powershell
   nslookup az104-04-vm0.[domain name] [Name server 1]
   ```

1. Verifikasi bahwa output perintah menyertakan alamat IP publik **az104-04-vm0**.

1. Dari panel Cloud Shell, jalankan yang berikut ini untuk menguji resolusi **nama eksternal dari kumpulan catatan DNS az104-04-vm1** di zona DNS yang baru dibuat (ganti tempat penampung `[Name server 1]` dengan nama **server Nama 1 yang** Anda catat sebelumnya dalam tugas ini dan `[domain name]` tempat penampung dengan nama domain DNS yang Anda buat sebelumnya dalam tugas ini):

   ```powershell
   nslookup az104-04-vm1.[domain name] [Name server 1]
   ```

1. Verifikasi bahwa output perintah menyertakan alamat IP publik **az104-04-vm1**.

## Membersihkan sumber daya

 > **Catatan**: Ingatlah untuk menghapus sumber daya Azure yang baru dibuat yang tidak lagi Anda gunakan. Dengan menghapus sumber daya yang tidak digunakan, Anda tidak akan melihat biaya yang tak terduga.

 > **Catatan**: Jangan khawatir jika sumber daya lab tidak dapat segera dihapus. Terkadang sumber daya memiliki dependensi dan membutuhkan waktu lebih lama untuk dihapus. Ini adalah tugas Administrator yang umum untuk memantau penggunaan sumber daya, jadi tinjau sumber daya Anda secara berkala di Portal untuk melihat bagaimana pembersihannya. 

1. Di portal Azure, buka sesi **PowerShell** dalam panel **Cloud Shell**.

1. Buat daftar semua grup sumber daya yang dibuat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-04*'
   ```

1. Hapus semua grup sumber daya yang Anda buat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-04*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Catatan**: Perintah dijalankan secara asinkron (seperti yang ditentukan oleh parameter -AsJob), jadi sementara Anda akan dapat menjalankan perintah PowerShell lain segera setelah itu dalam sesi PowerShell yang sama, akan memakan waktu beberapa menit sebelum grup sumber daya benar-benar dihapus.

## Tinjau

Di lab ini, Anda telah:

+ Membuat dan mengonfigurasi jaringan virtual
+ Menyebarkan mesin virtual ke dalam jaringan virtual
+ Alamat IP pribadi dan publik yang dikonfigurasi dari VM Azure
+ Kelompok keamanan jaringan yang dikonfigurasi
+ Azure DNS yang dikonfigurasi untuk resolusi nama internal
+ Azure DNS yang dikonfigurasi untuk resolusi nama eksternal
