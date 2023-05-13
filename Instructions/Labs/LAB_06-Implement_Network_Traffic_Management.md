---
lab:
  title: 06 - Menerapkan Manajemen Lalu Lintas
  module: Administer Network Traffic Management
---

# Lab 06 - Menerapkan Manajemen Lalu Lintas
# Panduan lab siswa

## Skenario lab

Anda ditugaskan untuk menguji pengelolaan lalu lintas jaringan yang menargetkan mesin virtual Azure di topologi jaringan hub dan spoke, yang Contoso pertimbangkan untuk diterapkan di lingkungan Azure-nya (sebagai ganti membuat topologi mesh, yang Anda uji di lab sebelumnya). Pengujian ini perlu menyertakan penerapan konektivitas antar spoke dengan mengandalkan rute yang ditentukan pengguna yang memaksa lalu lintas mengalir melalui hub, serta distribusi lalu lintas di seluruh mesin virtual dengan menggunakan load balancer lapisan 4 dan lapisan 7. Untuk tujuan ini, Anda ingin menggunakan Azure Load Balancer (lapisan 4) dan Azure Application Gateway (lapisan 7).

**Catatan:** Tersedia **[simulasi lab interaktif](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2010)** yang memungkinkan Anda mengklik lab ini sesuai keinginan Anda. Anda mungkin menemukan sedikit perbedaan antara simulasi interaktif dan lab yang dihosting, tetapi konsep dan ide utama yang ditunjukkan sama. 

>**Catatan**: Lab ini, secara default, memerlukan total 8 vCPU yang tersedia dalam seri Standard_Dsv3 di wilayah yang Anda pilih untuk penerapan, karena melibatkan penerapan empat VM Azure dari SKU Standard_D2s_v3. Jika siswa Anda menggunakan akun uji coba, dengan batas 4 vCPU, Anda dapat menggunakan ukuran VM yang hanya memerlukan satu vCPU (seperti Standard_B1s).

## Tujuan

Di lab ini Anda akan:

+ Tugas 1: Memprovisikan lingkungan lab
+ Tugas 2: Mengonfigurasi topologi jaringan spoke dan hub
+ Tugas 3: Menguji transitivitas peering jaringan virtual
+ Tugas 4: Mengonfigurasi perutean di topologi spoke dan hub
+ Tugas 5: Menerapkan Azure Load Balancer
+ Tugas 6: Menerapkan Azure Application Gateway

## Perkiraan waktu: 60 menit

## Diagram arsitektur

![gambar](../media/lab06.png)


### Petunjuk

## Latihan 1

## Tugas 1: Memprovisikan lingkungan lab

Dalam tugas ini, Anda akan menyebarkan empat mesin virtual ke wilayah Azure yang sama. Dua yang pertama akan berada di jaringan virtual hub, sementara masing-masing dari dua sisanya akan berada di jaringan virtual spoke yang terpisah.

1. Masuk ke [portal Microsoft Azure](https://portal.azure.com).

1. Di portal Microsoft Azure, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**.

    >**Catatan**: Jika ini pertama kalinya Anda memulai **Cloud Shell** dan Anda melihat pesan **Anda tidak memiliki penyimpanan yang terinstal**, pilih langganan yang Anda gunakan di lab ini, dan klik **Buat penyimpanan**.

1. Di bilah alat panel Cloud Shell, klik ikon **Unggah/Unduh file**, di menu menurun, klik **Unggah** dan unggah file **\\Allfiles\\Labs\\06\\az104-06-vms-loop-template.json** dan **\\Allfiles\\Labs\\06\\az104-06-vms-loop -parameters.json** ke dalam direktori beranda Cloud Shell.

1. Dari panel Cloud Shell, jalankan perintah berikut untuk membuat grup sumber daya pertama yang akan menghosting lingkungan lab (ganti tempat penampung '[Azure_region]' dengan nama wilayah Azure tempat Anda ingin menyebarkan mesin virtual Azure)(Anda dapat menggunakan cmdlet "(Get-AzLocation).Location" untuk mendapatkan daftar wilayah):

    ```powershell 
    $location = '[Azure_region]'
    ```
    
    Sekarang nama grup sumber daya:
    ```powershell
    $rgName = 'az104-06-rg1'
    ```
    
    Dan terakhir, buat grup sumber daya di lokasi yang Anda inginkan:
    ```powershell
    New-AzResourceGroup -Name $rgName -Location $location
    ```


1. Dari panel Cloud Shell, jalankan perintah berikut untuk membuat tiga jaringan virtual dan empat VM Azure ke dalamnya dengan menggunakan template dan file parameter yang Anda unggah:

    >**Catatan**: Anda akan diminta untuk memberikan kata sandi Admin.

   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-06-vms-loop-template.json `
      -TemplateParameterFile $HOME/az104-06-vms-loop-parameters.json
   ```

    >**Catatan**: Tunggu hingga penyebaran selesai sebelum melanjutkan ke langkah berikutnya. Proses ini memerlukan waktu sekitar 5 menit.

    >**Catatan**: Jika mendapatkan kesalahan yang menyatakan ukuran VM tidak tersedia, mintalah bantuan instruktur Anda dan coba langkah-langkah ini.
    > 1. Klik tombol `{}` di CloudShell Anda, pilih **az104-06-vms-loop-parameters.json** dari bilah sisi kiri dan catat nilai parameter `vmSize`.
    > 1. Periksa lokasi tempat grup sumber daya 'az104-06-rg1' disebarkan. Anda dapat menjalankan `az group show -n az104-06-rg1 --query location` di CloudShell untuk mendapatkannya.
    > 1. Jalankan `az vm list-skus --location <Replace with your location> -o table --query "[? contains(name,'Standard_D2s')].name"` di CloudShell Anda.
    > 1. Ganti nilai parameter `vmSize` dengan salah satu nilai yang dikembalikan oleh perintah yang baru saja Anda jalankan. Jika tidak ada nilai yang dikembalikan, Anda mungkin perlu memilih wilayah yang berbeda untuk disebarkan. Anda juga dapat memilih nama keluarga yang berbeda, seperti "Standard_B1s".
    > 1. Sekarang sebarkan ulang template Anda dengan menjalankan perintah `New-AzResourceGroupDeployment` lagi. Anda dapat menekan tombol atas beberapa kali yang akan membawa ke perintah yang terakhir dijalankan.

1. Dari panel Cloud Shell, jalankan perintah berikut untuk menginstal ekstensi Network Watcher pada VM Azure yang disebarkan pada langkah sebelumnya:

   ```powershell
   $rgName = 'az104-06-rg1'
   $location = (Get-AzResourceGroup -ResourceGroupName $rgName).location
   $vmNames = (Get-AzVM -ResourceGroupName $rgName).Name

   foreach ($vmName in $vmNames) {
     Set-AzVMExtension `
     -ResourceGroupName $rgName `
     -Location $location `
     -VMName $vmName `
     -Name 'networkWatcherAgent' `
     -Publisher 'Microsoft.Azure.NetworkWatcher' `
     -Type 'NetworkWatcherAgentWindows' `
     -TypeHandlerVersion '1.4'
   }
   ```

    >**Catatan**: Tunggu hingga penyebaran selesai sebelum melanjutkan ke langkah berikutnya. Proses ini memerlukan waktu sekitar 5 menit.



1. Tutup panel Cloud Shell.

## Tugas 2: Mengonfigurasi topologi jaringan spoke dan hub

Dalam tugas ini, Anda akan mengonfigurasi peering lokal antara jaringan virtual yang Anda sebarkan di tugas sebelumnya untuk membuat topologi jaringan hub dan spoke.

1. Di portal Microsoft Azure, cari dan pilih **Jaringan virtual**.

1. Tinjauan jaringan virtual yang Anda buat di tugas sebelumnya.

    >**Catatan**: Template yang Anda gunakan untuk penyebaran tiga jaringan virtual memastikan bahwa rentang alamat IP dari tiga jaringan virtual tidak tumpang tindih.

1. Di daftar jaringan virtual, pilih **az104-06-vnet2**.

1. Pada panel **az104-06-vnet2**, pilih **Properti**. 

1. Pada panel Properti **az104-06-vnet2\|** , catat nilai properti **ID Sumber Daya**.

1. Navigasikan kembali ke daftar jaringan virtual dan pilih **az104-06-vnet3**.

1. Pada panel **az104-06-vnet3**, pilih **Properti**. 

1. Pada panel Properti **az104-06-vnet3\|** , catat nilai properti **ID Sumber Daya**.

    >**Catatan**: Anda akan memerlukan nilai properti ResourceID untuk kedua jaringan virtual nanti dalam tugas ini.

    >**Catatan**: Ini adalah solusi yang mengatasi masalah dengan portal Microsoft Azure yang terkadang tidak menampilkan jaringan virtual yang baru diprovisikan saat membuat peering jaringan virtual.

1. Di daftar jaringan virtual, klik **az104-06-vnet01**.

1. Pada panel jaringan virtual **az104-06-vnet01**, di bagian **Pengaturan**, klik **Peering** lalu klik **+ Tambahkan**.

1. Tambahkan peering dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya) dan klik **Tambahkan**:

    | Pengaturan | Nilai |
    | --- | --- |
    | Jaringan virtual ini: Nama tautan peering | **az104-06-vnet01_to_az104-06-vnet2** |
    | Lalu lintas untuk jaringan virtual jarak jauh | **Izinkan (default)** |
    | Lalu lintas yang diteruskan dari jaringan virtual jarak jauh | **Memblokir lalu lintas yang berasal dari luar jaringan virtual ini** |
    | Gateway jaringan virtual | **Tidak ada (default)** |
    | Jaringan virtual jarak jauh: Nama tautan peering | **az104-06-vnet2_to_az104-06-vnet01** |
    | Model penyebaran jaringan virtual | **Manajer sumber daya** |
    | Saya mengetahui ID sumber daya saya | diaktifkan |
    | ID sumber daya | nilai parameter resourceID **az104-06-vnet2** yang Anda catat sebelumnya dalam tugas ini |
    | Lalu lintas untuk jaringan virtual jarak jauh | **Izinkan (default)** |
    | Lalu lintas yang diteruskan dari jaringan virtual jarak jauh | **Izinkan (default)** |
    | Gateway jaringan virtual | **Tidak ada (default)** |

    >**Catatan**: Tunggu hingga operasi selesai.

    >**Catatan**: Langkah ini membuat dua peering lokal - satu dari az104-06-vnet01 ke az104-06-vnet2 dan yang lainnya dari az104-06-vnet2 ke az104-06-vnet01.

    >**Catatan**: **Izinkan lalu lintas yang diteruskan** harus diaktifkan untuk memfasilitasi perutean antara jaringan virtual spoke, yang akan Anda terapkan nanti di lab ini.

1. Pada panel jaringan virtual **az104-06-vnet01**, di bagian **Pengaturan**, klik **Peering** lalu klik **+ Tambahkan**.

1. Tambahkan peering dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya) dan klik **Tambahkan**:

    | Pengaturan | Nilai |
    | --- | --- |
    | Jaringan virtual ini: Nama tautan peering | **az104-06-vnet01_to_az104-06-vnet3** |
    | Lalu lintas untuk jaringan virtual jarak jauh | **Izinkan (default)** |
    | Lalu lintas yang diteruskan dari jaringan virtual jarak jauh | **Memblokir lalu lintas yang berasal dari luar jaringan virtual ini** |
    | Gateway jaringan virtual | **Tidak ada (default)** |
    | Jaringan virtual jarak jauh: Nama tautan peering | **az104-06-vnet3_to_az104-06-vnet01** |
    | Model penyebaran jaringan virtual | **Manajer sumber daya** |
    | Saya mengetahui ID sumber daya saya | diaktifkan |
    | ID sumber daya | nilai parameter resourceID **az104-06-vnet3** yang Anda catat sebelumnya dalam tugas ini |
    | Lalu lintas untuk jaringan virtual jarak jauh | **Izinkan (default)** |
    | Lalu lintas yang diteruskan dari jaringan virtual jarak jauh | **Izinkan (default)** |
    | Gateway jaringan virtual | **Tidak ada (default)** |

    >**Catatan**: Langkah ini membuat dua peering lokal - satu dari az104-06-vnet01 ke az104-06-vnet3 dan yang lainnya dari az104-06-vnet3 ke az104-06-vnet01. Langkah ini menyelesaikan pengaturan hub dan topologi spoke (dengan dua jaringan virtual spoke).

    >**Catatan**: **Izinkan lalu lintas yang diteruskan** harus diaktifkan untuk memfasilitasi perutean antara jaringan virtual spoke, yang akan Anda terapkan nanti di lab ini.

## Tugas 3: Menguji transitivitas peering jaringan virtual

Dalam tugas ini, Anda akan menguji transitivitas peering jaringan virtual dengan menggunakan Network Watcher.

1. Di portal Microsoft Azure, cari dan pilih **Network Watcher**.

1. Pada panel **Network Watcher**, perluas daftar wilayah Azure dan verifikasi bahwa layanan diaktifkan di wilayah yang Anda gunakan. 

1. Pada panel **Network Watcher**, navigasikan ke **Pemecahan masalah koneksi**.

1. Pada bilah **Network Watcher - Pemecahan masalah koneksi**, lakukan pemeriksaan dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    > **Catatan** : Mungkin perlu waktu beberapa menit untuk mencantumkan grup sumber daya. Jika Anda tidak ingin menunggu, coba ini: hapus Network Watcher, buat Network Watcher baru, lalu coba lagi Pemecahan Masalah Koneksi. 

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | **az104-06-rg1** |
    | Jenis sumber | **Mesin virtual** |
    | Komputer virtual | **az104-06-vm0** |
    | Tujuan | **Tentukan secara manual** |
    | URI, FQDN atau IPv4 | **10.62.0.4** |
    | Protokol | **TCP** |
    | Port Tujuan | **3389** |

    > **Catatan**: **10.62.0.4** mewakili alamat IP privat **az104-06-vm2**

1. Klik **Periksa** dan tunggu hingga hasil pemeriksaan konektivitas ditampilkan. Verifikasi bahwa statusnya **Dapat dijangkau**. Tinjauan jalur jaringan dan perhatikan bahwa koneksinya langsung, tanpa lompatan perantara di antara VM.

    > **Catatan**: Hal ini diharapkan, karena jaringan virtual hub di-peering langsung dengan jaringan virtual spoke pertama.

1. Pada bilah **Network Watcher - Pemecahan masalah koneksi**, lakukan pemeriksaan dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | **az104-06-rg1** |
    | Jenis sumber | **Mesin virtual** |
    | Komputer virtual | **az104-06-vm0** |
    | Tujuan | **Tentukan secara manual** |
    | URI, FQDN atau IPv4 | **10.63.0.4** |
    | Protokol | **TCP** |
    | Port Tujuan | **3389** |

    > **Catatan**: **10.63.0.4** mewakili alamat IP pribadi **az104-06-vm3**

1. Klik **Periksa** dan tunggu hingga hasil pemeriksaan konektivitas ditampilkan. Verifikasi bahwa statusnya **Dapat dijangkau**. Tinjauan jalur jaringan dan perhatikan bahwa koneksinya langsung, tanpa lompatan perantara di antara VM.

    > **Catatan**: Hal ini diharapkan, karena jaringan virtual hub di-peering langsung dengan jaringan virtual spoke kedua.

1. Pada bilah **Network Watcher - Pemecahan masalah koneksi**, lakukan pemeriksaan dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | **az104-06-rg1** |
    | Jenis sumber | **Mesin virtual** |
    | Komputer virtual | **az104-06-vm2** |
    | Tujuan | **Tentukan secara manual** |
    | URI, FQDN atau IPv4 | **10.63.0.4** |
    | Protokol | **TCP** |
    | Port Tujuan | **3389** |

1. Klik **Periksa** dan tunggu hingga hasil pemeriksaan konektivitas ditampilkan. Perhatikan bahwa statusnya **Tidak dapat dijangkau**.

    > **Catatan**: Hal ini diharapkan, karena kedua jaringan virtual spoke tidak di-peering satu sama lain (peering jaringan virtual tidak transitif).

## Tugas 4: Mengonfigurasi perutean di topologi spoke dan hub

Dalam tugas ini, Anda akan mengonfigurasi dan menguji perutean antara dua jaringan virtual spoke dengan mengaktifkan penerusan IP pada antarmuka jaringan mesin virtual **az104-06-vm0**, mengaktifkan perutean dalam sistem operasinya, dan mengonfigurasi pengguna rute yang ditentukan pada jaringan virtual spoke.

1. Di portal Microsoft Azure, cari dan pilih **Mesin virtual**.

1. Pada panel **Mesin virtual**, dalam daftar mesin virtual, klik **az104-06-vm0**.

1. Pada panel mesin virtual **az104-06-vm0**, di bagian **Pengaturan**, klik **Jaringan**.

1. Klik tautan **az104-06-nic0** di samping label **Antarmuka jaringan**, lalu, pada panel antarmuka jaringan **az104-06-nic0**, di **Pengaturan**, klik **Konfigurasi IP**.

1. Atur **Penerusan IP** ke **Diaktifkan** dan simpan perubahannya.

   > **Catatan**: Pengaturan ini diperlukan agar **az104-06-vm0** berfungsi sebagai router, yang akan merutekan lalu lintas antara dua jaringan virtual spoke.

   > **Catatan**: Sekarang Anda perlu mengonfigurasi sistem operasi mesin virtual **az104-06-vm0** untuk mendukung perutean.

1. Di portal Microsoft Azure, navigasikan kembali ke panel mesin virtual Azure **az104-06-vm0** dan klik **Ringkasan**.

1. Pada panel **az104-06-vm0**, di bagian **Operasi**, klik **Jalankan perintah**, dan, dalam daftar perintah, klik **RunPowerShellScript**.

1. Pada panel **Jalankan Skrip Perintah**, ketik perintah berikut dan klik **Jalankan** untuk menginstal peran Server Windows Akses Jarak Jauh.

   ```powershell
   Install-WindowsFeature RemoteAccess -IncludeManagementTools
   ```

   > **Catatan**: Tunggu konfirmasi bahwa perintah berhasil diselesaikan.

1. Pada panel **Jalankan Skrip Perintah**, ketik perintah berikut dan klik **Jalankan** untuk menginstal layanan peran Perutean.

   ```powershell
   Install-WindowsFeature -Name Routing -IncludeManagementTools -IncludeAllSubFeature

   Install-WindowsFeature -Name "RSAT-RemoteAccess-Powershell"

   Install-RemoteAccess -VpnType RoutingOnly

   Get-NetAdapter | Set-NetIPInterface -Forwarding Enabled
   ```

   > **Catatan**: Tunggu konfirmasi bahwa perintah berhasil diselesaikan.

   > **Catatan**: Sekarang Anda perlu membuat dan mengonfigurasi rute yang ditentukan pengguna pada jaringan virtual spoke.

1. Di portal Microsoft Azure, cari dan pilih **Tabel rute** dan, pada panel **Tabel rute**, klik **+ Buat**.

1. Buat tabel rute dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | **az104-06-rg1** |
    | Lokasi | nama wilayah Azure tempat Anda membuat jaringan virtual |
    | Nama | **az104-06-rt23** |
    | Merambat rute gateway | **Tidak** |

1. Klik **Tinjauan dan Buat**. Biarkan validasi berjalan, dan klik **Buat** untuk mengirimkan penyebaran Anda.

   > **Catatan**: Tunggu hingga tabel rute dibuat. Ini akan memakan waktu sekitar 3 menit.

1. Klik **Buka sumber daya**.

1. Pada panel tabel rute **az104-06-rt23**, di bagian **Pengaturan**, klik **Rute**, lalu klik **+ Tambahkan**.

1. Tambahkan rute baru dengan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama rute | **az104-06-route-vnet2-to-vnet3** |
    | Tujuan awalan alamat | **Alamat IP** |
    | Alamat IP tujuan/rentang CIDR | **10.63.0.0/20** |
    | Tipe Lompatan Berikutnya | **Appliance virtual** |
    | Alamat lompatan berikutnya | **10.60.0.4** |

1. Klik **Tambahkan**

1. Kembali ke panel tabel rute **az104-06-rt23**, di bagian **Pengaturan**, klik **Subnet**, lalu klik **+ Kaitkan**.

1. Kaitkan tabel rute **az104-06-rt23** dengan subnet berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Jaringan virtual | **az104-06-vnet2** |
    | Subnet | **subnet0** |

1. Klik **Tambahkan**

1. Navigasikan kembali ke panel **Tabel rute** dan klik **+ Buat**.

1. Buat tabel rute dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | **az104-06-rg1** |
    | Wilayah | nama wilayah Azure tempat Anda membuat jaringan virtual |
    | Nama | **az104-06-rt32** |
    | Merambat rute gateway | **Tidak** |

1. Klik Tinjauan dan Buat. Biarkan validasi berjalan, dan tekan Buat untuk mengirimkan penyebaran Anda.

   > **Catatan**: Tunggu hingga tabel rute dibuat. Ini akan memakan waktu sekitar 3 menit.

1. Klik **Buka sumber daya**.

1. Pada panel tabel rute **az104-06-rt32**, di bagian **Pengaturan**, klik **Rute**, lalu klik **+ Tambahkan**.

1. Tambahkan rute baru dengan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama rute | **az104-06-route-vnet3-to-vnet2** |
    | Tujuan awalan alamat | **Alamat IP** |
    | Alamat IP tujuan/rentang CIDR | **10.62.0.0/20** |
    | Tipe Lompatan Berikutnya | **Appliance virtual** |
    | Alamat lompatan berikutnya | **10.60.0.4** |

1. Klik **OK**

1. Kembali ke panel tabel rute **az104-06-rt32**, di bagian **Pengaturan**, klik **Subnet**, lalu klik **+ Kaitkan**.

1. Kaitkan tabel rute **az104-06-rt32** dengan subnet berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Jaringan virtual | **az104-06-vnet3** |
    | Subnet | **subnet0** |

1. Klik **OK**

1. Di portal Microsoft Azure, navigasikan kembali ke panel **Network Watcher - Pemecahan masalah koneksi**.

1. Pada bilah **Network Watcher - Pemecahan masalah koneksi**, lakukan pemeriksaan dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | **az104-06-rg1** |
    | Jenis sumber | **Mesin virtual** |
    | Komputer virtual | **az104-06-vm2** |
    | Tujuan | **Tentukan secara manual** |
    | URI, FQDN atau IPv4 | **10.63.0.4** |
    | Protokol | **TCP** |
    | Port Tujuan | **3389** |

1. Klik **Periksa** dan tunggu hingga hasil pemeriksaan konektivitas ditampilkan. Verifikasi bahwa statusnya **Dapat dijangkau**. Tinjauan jalur jaringan dan perhatikan bahwa lalu lintas dirutekan melalui **10.60.0.4**, ditetapkan ke adaptor jaringan **az104-06-nic0**. Jika statusnya **Tidak dapat dijangkau**, Anda harus berhenti, lalu memulai az104-06-vm0.

    > **Catatan**: Hal ini diharapkan, karena lalu lintas antara jaringan virtual spoke sekarang dirutekan melalui mesin virtual yang terletak di jaringan virtual hub, yang berfungsi sebagai router.

    > **Catatan**: Anda dapat menggunakan **Network Watcher** untuk melihat topologi jaringan.

## Tugas 5: Menerapkan Azure Load Balancer

Dalam tugas ini, Anda akan menerapkan Azure Load Balancer di depan dua mesin virtual Azure di jaringan virtual hub.

1. Di portal Microsoft Azure, cari dan pilih **Load balancer** dan, pada bilah **Load balancer**, klik **+ Buat**.

1. Buat load balancer dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya) lalu klik **Berikutnya : Konfigurasi IP frontend**:

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | **az104-06-rg4** (jika perlu buat) |
    | Nama | **az104-06-lb4** |
    | Wilayah | nama wilayah Azure tempat Anda menyebarkan semua sumber daya lainnya di lab ini |
    | SKU  | **Standard** |
    | Jenis | **Publik** |
    | Tingkat | **Wilayah** |
    
1. Pada tab **Konfigurasi IP frontend**, klik **Tambahkan konfigurasi IP frontend** dan gunakan pengaturan berikut sebelum mengklik **OK** lalu **Tambahkan**. Setelah selesai klik **Berikutnya: Kumpulan backend**. 
     
    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **az104-06-pip4** |
    | Versi IP | IPv4 |
    | Alamat IP publik | **Buat baru** |

1. Pada tab **Kumpulan backend**, klik **Tambahkan kumpulan backend** dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya). Klik **+ Tambahkan** (dua kali) lalu klik **Berikutnya: Aturan masuk**. 

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **az104-06-lb4-be1** |
    | Jaringan virtual | **az104-06-vnet01** |
    | Konfigurasi Kumpulan Backend | **NIC** | 
    | Versi IP | **IPv4** |
    | Klik **Tambahkan** untuk menambahkan mesin virtual |  |
    | az104-06-vm0 | **centang kotak** |
    | az104-06-vm1 | **centang kotak** |


1. Pada tab **Aturan masuk**, klik **Tambahkan aturan penyeimbangan beban**. Tambahkan aturan penyeimbangan beban dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya). Setelah selesai klik **Tambahkan**.

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **az104-06-lb4-lbrule1** |
    | Versi IP | **IPv4** |
    | Alamat IP Frontend | **az104-06-pip4** |
    | Kumpulan Backend | **az104-06-lb4-be1** |    
    | Protokol | **TCP** |
    | Port | **80** |
    | Port Backend | **80** |
    | Pemeriksaan kesehatan | **Buat baru** |
    | Nama | **az104-06-lb4-hp1** |
    | Protokol | **TCP** |
    | Port | **80** |
    | Interval | **5** |
    | Ambang tidak sehat | **2** |
    | Tutup jendela buat pemeriksaan kesehatan | **OK** | 
    | Persistensi sesi | **Tidak ada** |
    | Waktu idle habis (menit) | **4** |
    | Reset TCP | **Nonaktif** |
    | IP Mengambang | **Nonaktif** |
    | Terjemahan alamat jaringan sumber keluar (SNAT) | **Disarankan** |

1. Saat luang, tinjau tab lainnya, lalu klik **Tinjau dan buat**. Pastikan tidak ada kesalahan validasi, lalu klik **Buat**. 

1. Tunggu hingga penyeimbang beban diterapkan, lalu klik **Buka sumber daya**.  

1. Pilih **Konfigurasi IP frontend** dari halaman sumber daya Load Balancer. Salin alamat IP.

1. Buka tab browser lain dan arahkan ke alamat IP. Pastikan jendela browser menampilkan pesan **Halo Dunia dari az104-06-vm0** atau **Halo Dunia dari az104-06-vm1**.

1. Refresh jendela untuk memverifikasi perubahan pesan ke mesin virtual lainnya. Hal ini menunjukkan load balancer berputar melalui mesin virtual.

    > **Catatan**: Anda mungkin perlu merefresh lebih dari sekali atau membuka jendela browser baru dalam mode InPrivate.

## Tugas 6: Menerapkan Azure Application Gateway

Dalam tugas ini, Anda akan menerapkan Azure Application Gateway di depan dua mesin virtual Azure di jaringan virtual spoke.

1. Di portal Microsoft Azure, cari dan pilih **Jaringan virtual**.

1. Pada panel **Jaringan virtual**, di daftar jaringan virtual, klik **az104-06-vnet01**.

1. Pada panel jaringan virtual **az104-06-vnet01**, di bagian **Pengaturan**, klik **Subnet**, lalu klik **+ Subnet**.

1. Tambahkan subnet dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **subnet-appgw** |
    | Rentang alamat subnet | **10.60.3.224/27** |

1. Klik **Simpan**

    > **Catatan**: Subnet ini akan digunakan oleh instans Azure Application Gateway, yang akan Anda sebarkan nanti dalam tugas ini. Application Gateway memerlukan subnet khusus dengan ukuran /27 atau lebih besar.

1. Di portal Microsoft Azure, cari dan pilih **Application Gateway** dan, pada panel **Application Gateway**, klik **+ Buat**.

1. Pada tab **Dasar**, tentukan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | **az104-06-rg5** (buat baru) |
    | Nama gateway aplikasi | **az104-06-appgw5** |
    | Wilayah | nama wilayah Azure tempat Anda menyebarkan semua sumber daya lainnya di lab ini |
    | Tingkat | **Standard V2** |
    | Aktifkan penskalaan otomatis | **Tidak** |
    | Jumlah instans | **2** |
    | Zona ketersediaan | **Tidak ada** |
    | HTTP2 | **Nonaktif** |
    | Jaringan virtual | **az104-06-vnet01** |
    | Subnet | **subnet-appgw (10.60.3.224/27)** |

1. Klik **Berikutnya: Frontend >** dan tentukan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya). Setelah selesai, klik **OKE**. 

    | Pengaturan | Nilai |
    | --- | --- |
    | Jenis alamat IP frontend | **Publik** |
    | Alamat IP publik| **Tambah** yang baru | 
    | Nama | **az104-06-pip5** |
    | Zona ketersediaan | **Tidak ada** |

1. Klik **Berikutnya: Backend >** lalu **Tambahkan kumpulan backend**. Tentukan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya). Setelah selesai klik **Tambahkan**.

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **az104-06-appgw5-be1** |
    | Menambahkan kumpulan backend tanpa target | **Tidak** |
    | Alamat IP atau FQDN | **10.62.0.4** | 
    | Alamat IP atau FQDN | **10.63.0.4** |

    > **Catatan**: Target mewakili alamat IP privat mesin virtual di jaringan virtual spoke **az104-06-vm2** dan **az104-06-vm3**.

1. Klik **Berikutnya: Konfigurasi >** lalu **+ Tambahkan aturan perutean**. Tentukan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama aturan | **az104-06-appgw5-rl1** |
    | Prioritas | **10** |
    | Nama listener | **az104-06-appgw5-rl1l1** |
    | IP Frontend | **Publik** |
    | Protokol | **HTTP** |
    | Port | **80** |
    | Tipe listener | **Dasar** |
    | URL halaman kesalahan | **Tidak** |

1. Beralih ke tab **Target backend** dan tentukan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya). Setelah selesai klik **Tambahkan** (dua kali).  

    | Pengaturan | Nilai |
    | --- | --- |
    | Jenis target | **Kumpulan backend** |
    | Target ujung belakang | **az104-06-appgw5-be1** |
    | Pengaturan backend | **Tambah** yang baru |
    | Nama pengaturan backend | **az104-06-appgw5-http1** |
    | Protokol backend | **HTTP** |
    | Port Backend | **80** |
    | Pengaturan tambahan | **biarkan default** |
    | Nama Host | **biarkan default** |

1. Klik **Berikutnya: Tag >** , diikuti dengan **Berikutnya: Tinjauan + buat >** lalu klik **Buat**.

    > **Catatan**: Tunggu hingga instans Application Gateway dibuat. Proses ini mungkin perlu waktu sekitar 8 menit.

1. Di portal Microsoft Azure, cari dan pilih **Application Gateway** dan, pada panel **Application Gateway**, klik **az104-06-appgw5**.

1. Pada blade Application Gateway **az104-06-appgw5**, salin nilai **alamat IP publik Frontend**.

1. Buka jendela browser lain dan navigasikan ke alamat IP yang Anda identifikasi di langkah sebelumnya.

1. Pastikan jendela browser menampilkan pesan **Halo Dunia dari az104-06-vm2** atau **Halo Dunia dari az104-06-vm3**.

1. Refresh jendela untuk memverifikasi perubahan pesan ke mesin virtual lainnya. 

    > **Catatan**: Anda mungkin perlu merefresh lebih dari sekali atau membuka jendela browser baru dalam mode InPrivate.

    > **Catatan**: Menargetkan mesin virtual pada beberapa jaringan virtual bukanlah konfigurasi umum, tetapi hal ini dimaksudkan untuk menggambarkan poin bahwa Application Gateway mampu menargetkan mesin virtual pada beberapa jaringan virtual (serta titik akhir di wilayah Azure lain atau bahkan di luar Azure), tidak seperti Azure Load Balancer, yang memuat keseimbangan di seluruh mesin virtual di jaringan virtual yang sama.

## Membersihkan sumber daya

>**Catatan**: Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak digunakan akan memastikan bahwa Anda tidak akan melihat biaya yang tidak diharapkan.

>**Catatan**:  Jangan khawatir jika sumber daya lab tidak dapat segera dihapus. Terkadang sumber daya memiliki dependensi dan membutuhkan waktu lebih lama untuk dihapus. Ini adalah tugas Administrator yang umum untuk memantau penggunaan sumber daya, jadi tinjauan sumber daya Anda secara berkala di Portal untuk melihat bagaimana pembersihannya. 

1. Di portal Microsoft Azure, buka sesi **PowerShell** dalam panel **Cloud Shell**.

1. Buat daftar semua grup sumber daya yang dibuat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-06*'
   ```

1. Hapus semua grup sumber daya yang Anda buat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-06*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Catatan**: Perintah dijalankan secara asinkron (sebagaimana yang ditentukan oleh parameter -AsJob), jadi saat Anda akan dapat menjalankan perintah PowerShell lain langsung setelahnya dalam sesi PowerShell yang sama, proses ini akan memakan waktu beberapa menit sebelum grup sumber daya benar-benar dihapus.

## Tinjau

Di lab ini, Anda telah:

+ Memprovisikan lingkungan lab
+ Mengonfigurasi topologi jaringan hub dan spoke
+ Menguji transitivitas peering jaringan virtual
+ Konfigurasi perutean di topologi hub dan spoke
+ Menerapkan Azure Load Balancer
+ Menerapkan Azure Application Gateway
