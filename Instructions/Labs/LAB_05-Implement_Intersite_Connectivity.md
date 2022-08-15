---
lab:
  title: 05 - Menerapkan Konektivitas Antarsitus
  module: Module 05 - Intersite Connectivity
ms.openlocfilehash: 6254f1b47aacdb2b0e01f090ca182feacba5e076
ms.sourcegitcommit: be14e4ff5bc638e8aee13ec4b8be29525d404028
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 05/11/2022
ms.locfileid: "145198215"
---
# <a name="lab-05---implement-intersite-connectivity"></a>Lab 05 - Menerapkan Konektivitas Antarsitus
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Pusat data Contoso terletak di kantor Boston, New York, dan Seattle yang tersambung melalui tautan jaringan area luas mesh, dengan konektivitas penuh di antaranya. Anda perlu menerapkan lingkungan lab yang akan mencerminkan topologi jaringan lokal Contoso dan memverifikasi fungsinya.

## <a name="objectives"></a>Tujuan

Di lab ini Anda akan:

+ Tugas 1: Menyediakan lingkungan lab
+ Tugas 2: Mengonfigurasi peering jaringan virtual lokal dan global
+ Tugas 3: Menguji konektivitas antar situs

## <a name="estimated-timing-30-minutes"></a>Perkiraan waktu: 30 menit

## <a name="architecture-diagram"></a>Diagram arsitektur

![gambar](../media/lab05.png)

### <a name="instructions"></a>Instruksi

#### <a name="task-1-provision-the-lab-environment"></a>Tugas 1: Menyediakan lingkungan lab

Dalam tugas ini, Anda akan menyebarkan tiga mesin virtual, masing-masing ke dalam jaringan virtual terpisah, dengan dua di antaranya di wilayah Azure yang sama dan yang ketiga di wilayah Azure lainnya.

1. Masuk ke [portal Microsoft Azure](https://portal.azure.com).

1. Di portal Microsoft Azure, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**.

    >**Catatan**: Jika ini pertama kalinya Anda memulai **Cloud Shell** dan Anda melihat pesan **Anda tidak memiliki penyimpanan yang terpasang**, pilih langganan yang Anda gunakan di lab ini, dan klik **Buat penyimpanan**.

1. Di toolbar panel Cloud Shell, klik ikon **Unggah/Unduh file**, di menu tarik-turun, klik **Unggah** dan unggah file **\\Semua file \\Lab\\05\\az104-05-vnetvm-loop-template.json** dan **\\Semua file\\Lab\\05\\az104-05-vnetvm-loop -parameters.json** ke dalam direktori beranda Cloud Shell.

1. Edit file **Parameter** yang baru saja Anda unggah dan ubah kata sandinya. Jika Anda memerlukan bantuan untuk mengedit file di Shell, mintalah bantuan instruktur Anda. Untuk praktik terbaik, rahasia, seperti kata sandi, harus disimpan lebih aman di Key Vault. 

1. Dari panel Cloud Shell, jalankan yang berikut ini untuk membuat grup sumber daya yang akan menghosting lingkungan lab. Dua jaringan virtual pertama dan sepasang mesin virtual akan disebarkan di [Azure_region_1]. Jaringan virtual ketiga dan mesin virtual ketiga akan disebarkan dalam grup sumber daya yang sama tetapi [Azure_region_2] lainnya. (ganti tempat penampung [Azure_region_1] dan [Azure_region_2], termasuk kurung siku, dengan nama dua wilayah Azure yang berbeda tempat Anda ingin menyebarkan mesin virtual Azure ini. Contohnya adalah $location1 = 'eastus'. Anda dapat menggunakan Get-AzLocation untuk mencantumkan semua lokasi.):

   ```powershell
   $location1 = 'eastus'

   $location2 = 'westus'

   $rgName = 'az104-05-rg1'

   New-AzResourceGroup -Name $rgName -Location $location1
   ```

   >**Catatan**: Wilayah yang digunakan di atas telah diuji dan diketahui berfungsi saat lab ini terakhir kali ditinjau secara resmi. Jika Anda lebih suka menggunakan lokasi yang berbeda, atau tidak lagi berfungsi, Anda harus mengidentifikasi dua wilayah berbeda tempat mesin virtual Standard D2Sv3 dapat disebarkan.
   >
   >Untuk mengidentifikasi wilayah Azure, dari sesi PowerShell di Cloud Shell, jalankan **(Get-AzLocation).Location**
   >
   >Setelah Anda mengidentifikasi dua wilayah yang ingin Anda gunakan, jalankan perintah di bawah ini di Cloud Shell untuk setiap wilayah untuk mengonfirmasi bahwa Anda dapat menyebarkan mesin virtual D2Sv3 Stan dar
   >
   >```az vm list-skus --location <Replace with your location> -o table --query "[? contains(name,'Standard_D2s')].name" ```
   >
   >Jika perintah tidak mengembalikan hasil, maka Anda perlu memilih wilayah lain. Setelah mengidentifikasi dua wilayah yang sesuai, Anda dapat menyesuaikan wilayah di blok kode di atas.

1. Dari panel Cloud Shell, jalankan perintah berikut untuk membuat tiga jaringan virtual dan menerapkan mesin virtual ke dalamnya dengan menggunakan file template dan parameter yang Anda unggah:

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

#### <a name="task-2-configure-local-and-global-virtual-network-peering"></a>Tugas 2: Mengonfigurasi peering jaringan virtual lokal dan global

Dalam tugas ini, Anda akan mengonfigurasi peering lokal dan global antara jaringan virtual yang Anda sebarkan di tugas sebelumnya.

1. Di portal Microsoft Azure, cari dan pilih **Jaringan virtual**.

1. Tinjauan jaringan virtual yang Anda buat di tugas sebelumnya dan verifikasi bahwa dua jaringan pertama terletak di wilayah Azure yang sama dan yang ketiga di wilayah Azure yang berbeda.

    >**Catatan**: Templat yang Anda gunakan untuk penyebaran tiga jaringan virtual memastikan bahwa rentang alamat IP dari tiga jaringan virtual tidak tumpang tindih.

1. Dalam daftar jaringan virtual, klik **az104-05-vnet0**.

1. Pada panel jaringan virtual **az104-05-vnet0**, di bagian **Pengaturan**, klik **Peering lalu** klik **+ Tambahkan**.

1. Tambahkan peering dengan pengaturan berikut (biarkan orang lain dengan nilai defaultnya) dan klik **Tambahkan**:

    | Pengaturan | Nilai|
    | --- | --- |
    | Jaringan virtual ini: Nama tautan peering | **az104-05-vnet0_to_az104-05-vnet1** |
    | Jaringan virtual ini: Lalu lintas ke jaringan virtual jarak jauh | **Izinkan (default)** |
    | Jaringan virtual ini: Lalu lintas diteruskan dari jaringan virtual jarak jauh | **Memblokir lalu lintas yang berasal dari luar jaringan virtual ini** |
    | Gateway jaringan virtual | **Tidak ada** |
    | Jaringan virtual jarak jauh: Nama tautan peering | **az104-05-vnet1_to_az104-05-vnet0** |
    | Model penyebaran jaringan virtual | **Manajer sumber daya** |
    | Saya mengetahui ID sumber daya saya | tidak dipilih |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Jaringan virtual | **az104-05-vnet1** |
    | Lalu lintas untuk jaringan virtual jarak jauh | **Izinkan (default)** |
    | Lalu lintas yang diteruskan dari jaringan virtual jarak jauh | **Memblokir lalu lintas yang berasal dari luar jaringan virtual ini** |
    | Gateway jaringan virtual | **Tidak ada** |

    >**Catatan**: Langkah ini membuat dua peering lokal - satu dari az104-05-vnet0 hingga az104-05-vnet1 dan yang lainnya dari az104-05-vnet1 hingga az104-05-vnet0.

    >**Catatan**: Jika Anda mengalami masalah dengan antarmuka portal Microsoft Azure yang tidak menampilkan jaringan virtual yang dibuat di tugas sebelumnya, Anda dapat mengonfigurasi peering dengan menjalankan perintah PowerShell berikut dari Cloud Shell:
    
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
    | Jaringan virtual ini: Lalu lintas ke jaringan virtual jarak jauh | **Izinkan (default)** |
    | Jaringan virtual ini: Lalu lintas diteruskan dari jaringan virtual jarak jauh | **Memblokir lalu lintas yang berasal dari luar jaringan virtual ini** |
    | Gateway jaringan virtual | **Tidak ada** |
    | Jaringan virtual jarak jauh: Nama tautan peering | **az104-05-vnet2_to_az104-05-vnet0** |
    | Model penyebaran jaringan virtual | **Manajer sumber daya** |
    | Saya mengetahui ID sumber daya saya | tidak dipilih |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Jaringan virtual | **az104-05-vnet2** |
    | Lalu lintas untuk jaringan virtual jarak jauh | **Izinkan (default)** |
    | Lalu lintas yang diteruskan dari jaringan virtual jarak jauh | **Memblokir lalu lintas yang berasal dari luar jaringan virtual ini** |
    | Gateway jaringan virtual | **Tidak ada** |

    >**Catatan**: Langkah ini membuat dua peering global - satu dari az104-05-vnet0 hingga az104-05-vnet2 dan yang lainnya dari az104-05-vnet2 hingga az104-05-vnet0.

    >**Catatan**: Jika Anda mengalami masalah dengan antarmuka portal Microsoft Azure yang tidak menampilkan jaringan virtual yang dibuat di tugas sebelumnya, Anda dapat mengonfigurasi peering dengan menjalankan perintah PowerShell berikut dari Cloud Shell:
    
   ```powershell
   $rgName = 'az104-05-rg1'

   $vnet0 = Get-AzVirtualNetwork -Name 'az104-05-vnet0' -ResourceGroupName $rgname

   $vnet2 = Get-AzVirtualNetwork -Name 'az104-05-vnet2' -ResourceGroupName $rgname

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet0_to_az104-05-vnet2' -VirtualNetwork $vnet0 -RemoteVirtualNetworkId $vnet2.Id

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet2_to_az104-05-vnet0' -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet0.Id
   ``` 

1. Navigasi kembali ke panel **Jaringan virtual** dan, dalam daftar jaringan virtual, klik **az104-05-vnet1**.

1. Pada panel jaringan virtual **az104-05-vnet1**, di bagian **Pengaturan**, klik **Peering** lalu klik **+ Tambahkan**.

1. Tambahkan peering dengan pengaturan berikut (biarkan orang lain dengan nilai defaultnya) dan klik **Tambahkan**:

    | Pengaturan | Nilai|
    | --- | --- |
    | Jaringan virtual ini: Nama tautan peering | **az104-05-vnet1_to_az104-05-vnet2** |
    | Jaringan virtual ini: Lalu lintas ke jaringan virtual jarak jauh | **Izinkan (default)** |
    | Jaringan virtual ini: Lalu lintas diteruskan dari jaringan virtual jarak jauh | **Memblokir lalu lintas yang berasal dari luar jaringan virtual ini** |
    | Gateway jaringan virtual | **Tidak ada** |
    | Jaringan virtual jarak jauh: Nama tautan peering | **az104-05-vnet2_to_az104-05-vnet1** |
    | Model penyebaran jaringan virtual | **Manajer sumber daya** |
    | Saya mengetahui ID sumber daya saya | tidak dipilih |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Jaringan virtual | **az104-05-vnet2** |
    | Lalu lintas untuk jaringan virtual jarak jauh | **Izinkan (default)** |
    | Lalu lintas yang diteruskan dari jaringan virtual jarak jauh | **Memblokir lalu lintas yang berasal dari luar jaringan virtual ini** |
    | Gateway jaringan virtual | **Tidak ada** |

    >**Catatan**: Langkah ini membuat dua peering global - satu dari az104-05-vnet1 hingga az104-05-vnet2 dan yang lainnya dari az104-05-vnet2 hingga az104-05-vnet1.

    >**Catatan**: Jika Anda mengalami masalah dengan antarmuka portal Microsoft Azure yang tidak menampilkan jaringan virtual yang dibuat di tugas sebelumnya, Anda dapat mengonfigurasi peering dengan menjalankan perintah PowerShell berikut dari Cloud Shell:
    
   ```powershell
   $rgName = 'az104-05-rg1'

   $vnet1 = Get-AzVirtualNetwork -Name 'az104-05-vnet1' -ResourceGroupName $rgname

   $vnet2 = Get-AzVirtualNetwork -Name 'az104-05-vnet2' -ResourceGroupName $rgname

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet1_to_az104-05-vnet2' -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet2_to_az104-05-vnet1' -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id
   ``` 

#### <a name="task-3-test-intersite-connectivity"></a>Tugas 3: Menguji konektivitas antar situs

Dalam tugas ini, Anda akan menguji konektivitas antara mesin virtual pada tiga jaringan virtual yang Anda sambungkan melalui peering lokal dan global di tugas sebelumnya.

1. Di portal Microsoft Azure, cari dan pilih **Mesin virtual.**

1. Dalam daftar mesin virtual, klik **az104-05-vm0**.

1. Pada panel **az104-05-vm0**, klik **Hubungkan**, di menu menurun, klik **RDP**, di panel **Hubungkan dengan RDP**, klik **Unduh File RDP** dan ikuti petunjuk untuk memulai sesi Desktop Jauh.

    >**Catatan**: Langkah ini mengacu pada menghubungkan melalui Desktop Jauh dari komputer Windows. Di Mac, Anda dapat menggunakan Klien Desktop Jauh dari Mac App Store dan di komputer Linux Anda dapat menggunakan perangkat lunak klien RDP sumber terbuka.

    >**Catatan**: Anda dapat mengabaikan permintaan peringatan apa pun saat menghubungkan ke mesin virtual target.

1. Saat diminta, masuk dengan menggunakan nama pengguna **Siswa** dan sandi dari file parameter Anda. 

1. Dalam sesi Desktop Jarak Jauh ke **az104-05-vm0**, klik kanan tombol **Mulai** dan, di menu klik kanan, klik **Windows PowerShell (Admin)** .

1. Di jendela konsol Windows PowerShell, jalankan hal berikut untuk menguji konektivitas ke **az104-05-vm1** (yang memiliki alamat IP privat **10.51.0.4**) melalui port TCP 3389:

   ```powershell
   Test-NetConnection -ComputerName 10.51.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

    >**Catatan**: Pengujian menggunakan TCP 3389 karena port ini diizinkan secara default oleh firewall sistem operasi.

1. Periksa output perintah dan verifikasi bahwa koneksi berhasil.

1. Di jendela konsol Windows PowerShell, jalankan yang berikut ini untuk menguji konektivitas ke **az104-05-vm2** (yang memiliki alamat IP privat **10.52.0.4**):

   ```powershell
   Test-NetConnection -ComputerName 10.52.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

1. Beralih kembali ke portal Azure di komputer lab Anda dan navigasikan kembali ke panel **Mesin virtual**.

1. Dalam daftar mesin virtual, klik **az104-05-vm1**.

1. Pada panel **az104-05-vm1**, klik **Hubungkan**, di menu menurun, klik **RDP**, di panel **Hubungkan dengan RDP**, klik **Unduh File RDP** dan ikuti petunjuk untuk memulai sesi Desktop Jarak Jauh.

    >**Catatan**: Langkah ini mengacu pada menghubungkan melalui Desktop Jauh dari komputer Windows. Di Mac, Anda dapat menggunakan Klien Desktop Jauh dari Mac App Store dan di komputer Linux Anda dapat menggunakan perangkat lunak klien RDP sumber terbuka.

    >**Catatan**: Anda dapat mengabaikan permintaan peringatan apa pun saat menghubungkan ke mesin virtual target.

1. Saat diminta, masuk dengan menggunakan nama pengguna **Siswa** dan sandi dari file parameter Anda. 

1. Dalam sesi Desktop Jauh ke **az104-05-vm1**, klik kanan tombol **Mulai** dan, di menu klik kanan, klik **Windows PowerShell (Admin)** .

1. Di jendela konsol Windows PowerShell, jalankan yang berikut ini untuk menguji konektivitas ke **az104-05-vm2** (yang memiliki alamat IP pribadi **10.52.0.4**) melalui port TCP 3389:

   ```powershell
   Test-NetConnection -ComputerName 10.52.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

    >**Catatan**: Pengujian menggunakan TCP 3389 karena port ini diizinkan secara default oleh firewall sistem operasi.

1. Periksa output perintah dan verifikasi bahwa koneksi berhasil.

#### <a name="clean-up-resources"></a>Membersihkan sumber daya

>**Catatan**: Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak digunakan akan memastikan bahwa Anda tidak akan melihat biaya yang tidak diharapkan.

>**Catatan**:  Jangan khawatir jika sumber daya lab tidak dapat segera dihapus. Terkadang sumber daya memiliki dependensi dan membutuhkan waktu lebih lama untuk dihapus. Ini adalah tugas Administrator yang umum untuk memantau penggunaan sumber daya, jadi tinjau sumber daya Anda secara berkala di Portal untuk melihat bagaimana pembersihannya. 

1. Di portal Microsoft Azure, buka sesi **PowerShell** dalam panel **Cloud Shell**.

1. Buat daftar semua grup sumber daya yang dibuat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-05*'
   ```

1. Hapus semua grup sumber daya yang Anda buat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-05*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Catatan**: Perintah dijalankan secara asinkron (sebagaimana yang ditentukan oleh parameter -AsJob), jadi saat Anda akan dapat menjalankan perintah PowerShell lain langsung setelahnya dalam sesi PowerShell yang sama, proses ini akan memakan waktu beberapa menit sebelum grup sumber daya benar-benar dihapus.

#### <a name="review"></a>Tinjauan

Di lab ini, Anda telah:

+ Menyediakan lingkungan lab
+ Mengonfigurasi peering jaringan virtual lokal dan global
+ Menguji konektivitas antar situs
