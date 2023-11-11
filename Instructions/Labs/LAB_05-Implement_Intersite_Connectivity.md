---
lab:
  title: 'Lab 05: Menerapkan intersite Koneksi ivity'
  module: Administer Intersite Connectivity
---

# Lab 05 - Menerapkan Konektivitas Antar Situs
# Panduan lab siswa

## Skenario laboratorium

Pusat data Contoso terletak di kantor Boston, New York, dan Seattle yang tersambung melalui tautan jaringan area luas mesh, dengan konektivitas penuh di antaranya. Anda perlu menerapkan lingkungan lab yang akan mencerminkan topologi jaringan lokal Contoso dan memverifikasi fungsinya.

**Catatan:** Tersedia **[simulasi lab interaktif](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%209)** yang memungkinkan Anda mengklik lab ini sesuai keinginan Anda. Anda mungkin menemukan sedikit perbedaan antara simulasi interaktif dan lab yang dihosting, tetapi konsep dan ide utama yang ditunjukkan sama. 

## Tujuan

Di lab ini Anda akan:

+ Tugas 1: Memprovisikan lingkungan lab
+ Tugas 2: Mengonfigurasi peering jaringan virtual lokal dan global
+ Tugas 3: Menguji konektivitas antarsitus

## Perkiraan waktu: 30 menit

## Diagram arsitektur

![gambar](../media/lab05.png)

### Petunjuk

## Latihan 1

## Tugas 1: Memprovisikan lingkungan lab

Dalam tugas ini, Anda akan menyebarkan tiga mesin virtual, masing-masing ke dalam jaringan virtual terpisah, dengan dua di antaranya di wilayah Azure yang sama dan yang ketiga di wilayah Azure lainnya.

1. Masuk ke [portal Azure](https://portal.azure.com).

1. Di portal Microsoft Azure, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**.

    >**Catatan**: Jika ini adalah pertama kalinya Anda memulai **Cloud Shell** dan Anda disajikan dengan **pesan Anda tidak memiliki penyimpanan yang dipasang** , pilih langganan yang Anda gunakan di lab ini, dan klik **Buat penyimpanan**.

1. Di toolbar panel Cloud Shell, klik ikon **Unggah/Unduh file**, di menu tarik-turun, klik **Unggah** dan unggah file **\\Semua file \\Lab\\05\\az104-05-vnetvm-loop-template.json** dan **\\Semua file\\Lab\\05\\az104-05-vnetvm-loop -parameters.json** ke dalam direktori beranda Cloud Shell. 

1. Dari panel Cloud Shell, jalankan yang berikut ini untuk membuat grup sumber daya yang akan menghosting lingkungan lab. Dua jaringan virtual pertama dan sepasang mesin virtual akan disebarkan di [Azure_region_1]. Jaringan virtual ketiga dan mesin virtual ketiga akan disebarkan dalam grup sumber daya yang sama tetapi [Azure_region_2] lainnya. (ganti tempat penampung [Azure_region_1] dan [Azure_region_2], termasuk kurung siku, dengan nama dua wilayah Azure yang berbeda tempat Anda ingin menyebarkan mesin virtual Azure ini. Contohnya adalah $location1 = 'eastus'. Anda dapat menggunakan Get-AzLocation untuk mencantumkan semua lokasi.):

   ```powershell
   $location1 = 'eastus'

   $location2 = 'westus'

   $rgName = 'az104-05-rg1'

   New-AzResourceGroup -Name $rgName -Location $location1
   ```

   >**Catatan**: Wilayah yang digunakan di atas diuji dan diketahui berfungsi ketika lab ini terakhir kali ditinjau secara resmi. Jika Anda lebih suka menggunakan lokasi yang berbeda, atau tidak lagi berfungsi, Anda harus mengidentifikasi dua wilayah berbeda tempat mesin virtual Standard D2Sv3 dapat disebarkan.
   >
   >Untuk mengidentifikasi wilayah Azure, dari sesi PowerShell di Cloud Shell, jalankan **(Get-AzLocation).Location**
   >
   >Setelah Anda mengidentifikasi dua wilayah yang ingin Anda gunakan, jalankan perintah di bawah ini di Cloud Shell untuk setiap wilayah untuk mengonfirmasi bahwa Anda dapat menyebarkan mesin virtual D2Sv3 Stan dar
   >
   >```az vm list-skus --location <Replace with your location> -o table --query "[? contains(name,'Standard_D2s')].name" ```
   >
   >Jika perintah tidak mengembalikan hasil, maka Anda perlu memilih wilayah lain. Setelah mengidentifikasi dua wilayah yang sesuai, Anda dapat menyesuaikan wilayah di blok kode di atas.

1. Dari panel Cloud Shell, jalankan perintah berikut untuk membuat tiga jaringan virtual dan menerapkan mesin virtual ke dalamnya dengan menggunakan file template dan parameter yang Anda unggah:
    
    >**Catatan**: Anda akan diminta untuk memberikan kata sandi Admin.

   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-05-vnetvm-loop-template.json `
      -TemplateParameterFile $HOME/az104-05-vnetvm-loop-parameters.json `
      -location1 $location1 `
      -location2 $location2
   ```

    >**Catatan**: Tunggu hingga penyebaran selesai sebelum melanjutkan ke langkah berikutnya. Proses ini memerlukan waktu sekitar 2 menit.

1. Tutup panel Cloud Shell.

## Tugas 2: Mengonfigurasi peering jaringan virtual lokal dan global

Dalam tugas ini, Anda akan mengonfigurasi peering lokal dan global antara jaringan virtual yang Anda sebarkan di tugas sebelumnya.

1. Di portal Microsoft Azure, cari dan pilih **Jaringan virtual**.

1. Tinjau jaringan virtual yang Anda buat di tugas sebelumnya dan verifikasi bahwa dua jaringan pertama terletak di wilayah Azure yang sama dan yang ketiga di wilayah Azure yang berbeda.

    >**Catatan**: Templat yang Anda gunakan untuk penyebaran tiga jaringan virtual memastikan bahwa rentang alamat IP dari tiga jaringan virtual tidak tumpang tindih.

1. Dalam daftar jaringan virtual, klik **az104-05-vnet0**.

1. Pada panel jaringan virtual **az104-05-vnet0**, di bagian **Pengaturan**, klik **Peering lalu** klik **+ Tambahkan**.

1. Tambahkan peering dengan pengaturan berikut (biarkan orang lain dengan nilai defaultnya) dan klik **Tambahkan**:

    | Pengaturan | Nilai|
    | --- | --- |
    | Jaringan virtual ini: Nama tautan peering | **az104-05-vnet0_to_az104-05-vnet1** |
    | Pengaturan untuk mengizinkan akses, lalu lintas yang diteruskan, dan gateway | **Pastikan semua kotak dicentang** |
    | Jaringan virtual jarak jauh: Nama tautan peering | **az104-05-vnet1_to_az104-05-vnet0** |
    | Model penyebaran jaringan virtual | **Manajer sumber daya** |
    | Saya mengetahui ID sumber daya saya | tidak dipilih |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Jaringan virtual | **az104-05-vnet1** |
    | Perbolehkan akses ke jaringan virtual saat ini |  **Pastikan kotak dicentang (default)** |
    | Pengaturan untuk mengizinkan akses, lalu lintas yang diteruskan, dan gateway | **Pastikan semua kotak dicentang** |

    >**Catatan**: Langkah ini menetapkan dua peering lokal - satu dari az104-05-vnet0 ke az104-05-vnet1 dan yang lainnya dari az104-05-vnet1 ke az104-05-vnet0.

    >**Catatan**: Jika Anda mengalami masalah dengan antarmuka portal Azure yang tidak menampilkan jaringan virtual yang dibuat di tugas sebelumnya, Anda dapat mengonfigurasi peering dengan menjalankan perintah PowerShell berikut dari Cloud Shell:
    
   ```powershell
   $rgName = 'az104-05-rg1'

   $vnet0 = Get-AzVirtualNetwork -Name 'az104-05-vnet0' -ResourceGroupName $rgname

   $vnet1 = Get-AzVirtualNetwork -Name 'az104-05-vnet1' -ResourceGroupName $rgname

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet0_to_az104-05-vnet1' -VirtualNetwork $vnet0 -RemoteVirtualNetworkId $vnet1.Id

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet1_to_az104-05-vnet0' -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet0.Id
   ``` 

1. Pada panel jaringan virtual **az104-05-vnet0**, di bagian **Pengaturan**, klik **Peering lalu** klik **+ Tambahkan**.

1. Tambahkan peering dengan pengaturan berikut (biarkan orang lain dengan nilai defaultnya) dan klik **Tambahkan**:

    | Pengaturan | Nilai|
    | --- | --- |
    | Jaringan virtual ini: Nama tautan peering | **az104-05-vnet0_to_az104-05-vnet2** |
    | Izinkan akses ke jaringan virtual jarak jauh |**Pastikan kotak dicentang (default)** |
    | Jaringan virtual jarak jauh: Nama tautan peering | **az104-05-vnet2_to_az104-05-vnet0** |
    | Model penyebaran jaringan virtual | **Manajer sumber daya** |
    | Saya mengetahui ID sumber daya saya | tidak dipilih |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Jaringan virtual | **az104-05-vnet2** |
    | Perbolehkan akses ke jaringan virtual saat ini |**Pastikan kotak dicentang (default)** |

    >**Catatan**: Langkah ini menetapkan dua peering global - satu dari az104-05-vnet0 ke az104-05-vnet2 dan yang lainnya dari az104-05-vnet2 ke az104-05-vnet0.

    >**Catatan**: Jika Anda mengalami masalah dengan antarmuka portal Azure yang tidak menampilkan jaringan virtual yang dibuat di tugas sebelumnya, Anda dapat mengonfigurasi peering dengan menjalankan perintah PowerShell berikut dari Cloud Shell:
    
   ```powershell
   $rgName = 'az104-05-rg1'

   $vnet0 = Get-AzVirtualNetwork -Name 'az104-05-vnet0' -ResourceGroupName $rgname

   $vnet2 = Get-AzVirtualNetwork -Name 'az104-05-vnet2' -ResourceGroupName $rgname

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet0_to_az104-05-vnet2' -VirtualNetwork $vnet0 -RemoteVirtualNetworkId $vnet2.Id

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet2_to_az104-05-vnet0' -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet0.Id
   ``` 

1. Navigasi kembali ke panel **Jaringan virtual** dan, dalam daftar jaringan virtual, klik **az104-05-vnet1**.

1. Pada panel jaringan virtual **az104-05-vnet1**, di bagian **Setelan**, klik **Peering** lalu klik **+ Tambahkan**.

1. Tambahkan peering dengan pengaturan berikut (biarkan orang lain dengan nilai defaultnya) dan klik **Tambahkan**:

    | Pengaturan | Nilai|
    | --- | --- |
    | Jaringan virtual ini: Nama tautan peering | **az104-05-vnet1_to_az104-05-vnet2** |
    | Izinkan akses ke jaringan virtual jarak jauh | **Pastikan kotak dicentang (default)** |
    | Jaringan virtual jarak jauh: Nama tautan peering | **az104-05-vnet2_to_az104-05-vnet1** |
    | Model penyebaran jaringan virtual | **Manajer sumber daya** |
    | Saya mengetahui ID sumber daya saya | tidak dipilih |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Jaringan virtual | **az104-05-vnet2** |
    | Perbolehkan akses ke jaringan virtual saat ini | **Pastikan kotak dicentang (default)** |

    >**Catatan**: Langkah ini menetapkan dua peering global - satu dari az104-05-vnet1 ke az104-05-vnet2 dan yang lainnya dari az104-05-vnet2 ke az104-05-vnet1.

    >**Catatan**: Jika Anda mengalami masalah dengan antarmuka portal Azure yang tidak menampilkan jaringan virtual yang dibuat di tugas sebelumnya, Anda dapat mengonfigurasi peering dengan menjalankan perintah PowerShell berikut dari Cloud Shell:
    
   ```powershell
   $rgName = 'az104-05-rg1'

   $vnet1 = Get-AzVirtualNetwork -Name 'az104-05-vnet1' -ResourceGroupName $rgname

   $vnet2 = Get-AzVirtualNetwork -Name 'az104-05-vnet2' -ResourceGroupName $rgname

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet1_to_az104-05-vnet2' -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet2_to_az104-05-vnet1' -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id
   ``` 

## Tugas 3: Menguji konektivitas antarsitus

Dalam tugas ini, Anda akan menguji konektivitas antara mesin virtual pada tiga jaringan virtual yang Anda sambungkan melalui peering lokal dan global di tugas sebelumnya.

1. Di portal Microsoft Azure, cari dan pilih **Komputer virtual.**

1. Dalam daftar mesin virtual, klik **az104-05-vm0**.

1. Pada panel **az104-05-vm0**, klik **Hubungkan**, di menu drop-down, klik **RDP**, di panel **Hubungkan dengan RDP**, klik **Unduh File RDP** dan ikuti petunjuk untuk memulai sesi Desktop Jauh.

    >**Catatan**: Langkah ini mengacu pada menyambungkan melalui Desktop Jauh dari komputer Windows. Di Mac, Anda dapat menggunakan Klien Desktop Jauh dari Mac App Store dan di komputer Linux Anda dapat menggunakan perangkat lunak klien RDP sumber terbuka.

    >**Catatan**: Anda dapat mengabaikan perintah peringatan saat menyambungkan ke komputer virtual target.

1. Saat diminta, masuk dengan menggunakan **Nama pengguna siswa** dan kata sandi yang Anda konfigurasi saat menyebarkan komputer virtual Anda melalui CloudShell. 

1. Dalam sesi Desktop Jarak Jauh ke **az104-05-vm0**, klik kanan tombol **Mulai** dan, di menu klik kanan, klik **Windows PowerShell (Admin)**.

1. Di jendela konsol Windows PowerShell, jalankan hal berikut untuk menguji konektivitas ke **az104-05-vm1** (yang memiliki alamat IP privat **10.51.0.4**) melalui port TCP 3389:

   ```powershell
   Test-NetConnection -ComputerName 10.51.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

    >**Catatan**: Pengujian menggunakan TCP 3389 karena ini adalah port ini diizinkan secara default oleh firewall sistem operasi.

1. Periksa output perintah dan verifikasi bahwa koneksi berhasil.

1. Di jendela konsol Windows PowerShell, jalankan yang berikut ini untuk menguji konektivitas ke **az104-05-vm2** (yang memiliki alamat IP privat **10.52.0.4**):

   ```powershell
   Test-NetConnection -ComputerName 10.52.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

1. Beralih kembali ke portal Azure di komputer lab Anda dan navigasikan kembali ke panel **Mesin virtual**.

1. Dalam daftar mesin virtual, klik **az104-05-vm1**.

1. Pada panel **az104-05-vm1**, klik **Hubungkan**, di menu drop-down, klik **RDP**, di panel **Hubungkan dengan RDP**, klik **Unduh File RDP** dan ikuti petunjuk untuk memulai sesi Desktop Jarak Jauh.

    >**Catatan**: Langkah ini mengacu pada menyambungkan melalui Desktop Jauh dari komputer Windows. Di Mac, Anda dapat menggunakan Klien Desktop Jauh dari Mac App Store dan di komputer Linux Anda dapat menggunakan perangkat lunak klien RDP sumber terbuka.

    >**Catatan**: Anda dapat mengabaikan perintah peringatan saat menyambungkan ke komputer virtual target.

1. Saat diminta, masuk dengan menggunakan nama pengguna **Siswa** dan sandi dari file parameter Anda. 

1. Dalam sesi Desktop Jauh ke **az104-05-vm1**, klik kanan tombol **Mulai** dan, di menu klik kanan, klik **Windows PowerShell (Admin)**.

1. Di jendela konsol Windows PowerShell, jalankan yang berikut ini untuk menguji konektivitas ke **az104-05-vm2** (yang memiliki alamat IP pribadi **10.52.0.4**) melalui port TCP 3389:

   ```powershell
   Test-NetConnection -ComputerName 10.52.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

    >**Catatan**: Pengujian menggunakan TCP 3389 karena ini adalah port ini diizinkan secara default oleh firewall sistem operasi.

1. Periksa output perintah dan verifikasi bahwa koneksi berhasil.

## Membersihkan sumber daya

>**Catatan**: Ingatlah untuk menghapus sumber daya Azure yang baru dibuat yang tidak lagi Anda gunakan. Dengan menghapus sumber daya yang tidak digunakan, Anda tidak akan melihat biaya yang tak terduga.

>**Catatan**: Jangan khawatir jika sumber daya lab tidak dapat segera dihapus. Terkadang sumber daya memiliki dependensi dan membutuhkan waktu lebih lama untuk dihapus. Ini adalah tugas Administrator yang umum untuk memantau penggunaan sumber daya, jadi tinjau sumber daya Anda secara berkala di Portal untuk melihat bagaimana pembersihannya. 

1. Di portal Azure, buka sesi **PowerShell** dalam panel **Cloud Shell**.

1. Buat daftar semua grup sumber daya yang dibuat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-05*'
   ```

1. Hapus semua grup sumber daya yang Anda buat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-05*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Catatan**: Perintah dijalankan secara asinkron (seperti yang ditentukan oleh parameter -AsJob), jadi sementara Anda akan dapat menjalankan perintah PowerShell lain segera setelah itu dalam sesi PowerShell yang sama, akan memakan waktu beberapa menit sebelum grup sumber daya benar-benar dihapus.

## Tinjau

Di lab ini, Anda telah:

+ Memprovisikan lingkungan lab
+ Mengonfigurasi peering jaringan virtual lokal dan global
+ Menguji konektivitas antar situs
