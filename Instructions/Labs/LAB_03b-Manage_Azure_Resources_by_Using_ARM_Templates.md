---
lab:
  title: 'Lab 03: Mengelola sumber daya Azure dengan menggunakan Templat Azure Resource Manager'
  module: Administer Azure Resources
---

# Lab 03 - Mengelola sumber daya Azure dengan menggunakan Templat Azure Resource Manager

## Pengenalan lab

Di lab ini, Anda mempelajari cara mengotomatiskan penyebaran sumber daya. Anda mempelajari tentang templat Azure Resource Manager dan templat Bicep. Anda mempelajari tentang berbagai cara untuk menyebarkan templat. 

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur di lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkahnya ditulis menggunakan **US** Timur. 

## Perkiraan waktu: 50 menit

## Simulasi lab interaktif

Ada simulasi lab interaktif yang mungkin berguna bagi Anda untuk topik ini. Simulasi ini memungkinkan Anda mengklik skenario serupa dengan kecepatan Anda sendiri. Ada perbedaan antara simulasi interaktif dan lab ini, tetapi banyak konsep intinya sama. Langganan Azure tidak diperlukan. 

+ [Mengelola sumber daya Azure dengan menggunakan templat](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%205) Azure Resource Manager. Tinjau, buat, dan sebarkan disk terkelola dengan templat.
  
+ [Buat komputer virtual dengan templat](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%209). Sebarkan komputer virtual dengan templat Mulai Cepat.
  
## Skenario lab

Tim Anda ingin melihat cara untuk mengotomatiskan dan menyederhanakan penyebaran sumber daya. Organisasi Anda mencari cara untuk mengurangi overhead administratif, mengurangi kesalahan manusia, dan meningkatkan konsistensi.  

## Diagram arsitektur

![Diagram tugas.](../media/az104-lab03-architecture.png)

## Keterampilan pekerjaan

+ Tugas 1: Buat templat Azure Resource Manager.
+ Tugas 2: Edit templat Azure Resource Manager dan sebarkan ulang templat.
+ Tugas 3: Konfigurasikan Cloud Shell dan sebarkan templat dengan Azure PowerShell.
+ Tugas 4: Sebarkan templat dengan CLI. 
+ Tugas 5: Sebarkan sumber daya dengan menggunakan Azure Bicep.

## Tugas 1: Membuat templat Azure Resource Manager

Dalam tugas ini, kita akan membuat disk terkelola di portal Azure. Disk terkelola adalah penyimpanan yang dirancang untuk digunakan dengan komputer virtual. Setelah disk disebarkan, Anda akan mengekspor templat yang dapat Anda gunakan di penyebaran lain.

1. Masuk ke **portal Azure** - `https://portal.azure.com`.

1. Cari dan pilih `Disks`.

1. Pada halaman Disk, pilih **Buat**.

1. Pada halaman **Buat disk** terkelola, konfigurasikan disk lalu pilih **Ok**. 
    
    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | *langganan Anda* | 
    | Grup Sumber Daya | `az104-rg3` (Jika perlu, pilih **Buat baru**.)
    | Nama disk | `az104-disk1` | 
    | Wilayah | **US Timur** |
    | Zona ketersediaan | **Tidak ada redundansi infrastruktur yang diperlukan** | 
    | Jenis sumber | **Tidak** |
    | Performa | **HDD** standar (ubah ukuran) |
    | Ukuran | **32 Gib** | 

    >**Catatan:** Kami membuat disk terkelola sederhana sehingga Anda dapat berlatih dengan templat. Disk terkelola Azure adalah volume penyimpanan tingkat blok yang dikelola oleh Azure.

1. Klik **Tinjau + Buat** lalu pilih **Buat**.

1. Pantau pemberitahuan (kanan atas) dan setelah penyebaran pilih **Buka sumber daya**. 

1. Di bilah **Automation** , pilih **Ekspor templat**. 

1. Luangkan waktu semenit untuk meninjau **file Templat** dan **Parameter** .

1. Klik **Unduh** dan simpan templat ke drive lokal. Ini membuat file zip terkompresi. 

1. Gunakan File Explorer untuk mengekstrak konten file yang diunduh ke **folder Unduhan** di komputer Anda. Perhatikan ada dua file JSON (templat dan parameter). 

   >**Apakah Anda tahu?**  Anda dapat mengekspor seluruh grup sumber daya atau hanya sumber daya tertentu dalam grup sumber daya tersebut.

## Tugas 2: Edit templat Azure Resource Manager lalu sebarkan ulang templat

Dalam tugas ini, Anda menggunakan templat yang diunduh untuk menyebarkan disk terkelola baru. Tugas ini menguraikan cara mengulangi penyebaran dengan cepat dan mudah. 

1. Di portal Azure, cari dan pilih `Deploy a custom template`.

1. Pada bilah **Penyebaran** kustom, perhatikan ada kemampuan untuk menggunakan **templat** Mulai Cepat. Ada banyak templat bawaan seperti yang ditunjukkan di menu drop-down. 

1. Alih-alih menggunakan Mulai Cepat, pilih **Bangun templat Anda sendiri di editor**.

1. Pada bilah **Edit templat** , klik **Muat file** dan unggah **file template.json** yang Anda unduh ke disk lokal.

1. Di dalam panel editor, buat perubahan ini.

    + Ubah **disks_az104_disk1_name** menjadi `disk_name` (dua tempat untuk diubah)
    + Ubah **az104-disk1** menjadi `az104-disk2` (satu tempat untuk diubah)

1. Perhatikan bahwa ini adalah **disk Standar** . Lokasinya eastus****. Ukuran disk adalah **32GB**.

1. **Simpan** perubahan Anda.

1. Jangan lupa file parameter. Pilih **Edit parameter**, klik **Muat file** dan unggah **parameters.json**. 

1. Buat perubahan ini sehingga cocok dengan file templat.

    Ubah **disks_az104_disk1_name** menjadi **disk_name** (satu tempat untuk diubah)

1. **Simpan** perubahan Anda. 

1. Selesaikan pengaturan penyebaran kustom:

    | Pengaturan | Nilai |
    | --- |--- |
    | Langganan | *langganan Anda* |
    | Grup Sumber Daya | `az104-rg3` |
    | Wilayah | **(US) US Timur)** |
    | Disk_name | `az104-disk2` |

1. Pilih **Ulas + buat**, lalu pilih **Buat**.

1. Pilih **Buka sumber daya**. Pastikan **az104-disk2** dibuat.

1. Pada bilah **Gambaran Umum** , pilih grup sumber daya, **az104-rg3**. Anda sekarang harus memiliki dua disk.
   
1. Di bagian **Pengaturan, klik **Penyebaran****.

    >**Catatan:** Semua detail penyebaran didokumentasikan dalam grup sumber daya. Ini adalah praktik yang baik untuk meninjau beberapa penyebaran berbasis templat pertama untuk memastikan keberhasilan sebelum menggunakan templat untuk operasi skala besar.

1. Pilih penyebaran dan tinjau konten bilah **Input** dan **Templat** .

## Tugas 3: Mengonfigurasi Cloud Shell dan menyebarkan templat dengan Azure PowerShell

Dalam tugas ini, Anda bekerja dengan Azure Cloud Shell dan Azure PowerShell. Azure Cloud Shell adalah terminal interaktif, diautentikasi, dan dapat diakses browser untuk mengelola sumber daya Azure. Ini memberi fleksibilitas untuk memilih pengalaman shell yang paling sesuai dengan cara Anda bekerja, baik Bash atau PowerShell. Dalam tugas ini, Anda menggunakan PowerShell untuk menyebarkan templat. 

1. **Pilih ikon Cloud Shell** di kanan atas Portal Microsoft Azure. Secara bergantian, Anda dapat menavigasi langsung ke `https://shell.azure.com`.

   ![Cuplikan layar ikon cloud shell.](../media/az104-lab03-cloudshell-icon.png)

1. Saat diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**. 

    >**Apakah Anda tahu?**  Jika Anda sebagian besar bekerja dengan sistem Linux, Bash (CLI) terasa lebih akrab. Jika Anda sebagian besar bekerja dengan sistem Windows, Azure PowerShell merasa lebih akrab. 

1. **Pada layar Memulai** pilih **Pasang akun** penyimpanan lalu **saya ingin membuat akun** penyimpanan.  

    >**Catatan:** Untuk lab ini, akun penyimpanan diperlukan. Berikan informasi yang diperlukan. 
    
    | Pengaturan | Nilai |
    |  -- | -- |
    | Langganan | *pilih langganan Anda* |
    | Grup Sumber Daya | **az104-rg3** |
    | Wilayah | *pilih wilayah Anda* | 
    | Akun penyimpanan (Buat baru) | *harus unik secara global, antara 3 dan 24 karakter dan hanya menggunakan angka dan huruf kecil* |
    | Berbagi file (Buat baru) | `fs-cloudshell` |

1. Setelah selesai, pilih **Berikutnya**. Anda hanya perlu melakukan ini saat pertama kali menggunakan Cloud Shell. Dibutuhkan beberapa menit untuk menyediakan penyimpanan.

1. **Gunakan ikon Unggah/Unduh file** untuk mengunggah file templat dan parameter dari direktori unduhan. Anda harus mengunggah setiap file secara terpisah.

   >**Catatan:** Jika kapan saja Anda diminta untuk Beralih ke **cloud shell** klasik, lakukanlah. 

1. Verifikasi file Anda tersedia di penyimpanan Cloud Shell. 

    ```powershell
    dir
    ```
    >**Catatan**: Jika perlu, Anda dapat menggunakan **cls** untuk menghapus jendela perintah. Anda bisa menggunakan tombol panah untuk memindahkan riwayat perintah.
   
1. **Pilih ikon Editor** (kurung kurawal) dan navigasikan ke file JSON templat.

1. Mengubah. Misalnya, ubah nama disk menjadi **az104-disk3**. Gunakan **Ctrl +S** untuk menyimpan perubahan Anda. 

    >**Catatan**: Anda dapat menargetkan penyebaran templat anda ke grup sumber daya, langganan, grup manajemen, atau penyewa. Tergantung pada lingkup penyebaran, Anda menggunakan perintah yang berbeda.

1. Untuk menyebarkan ke grup sumber daya, gunakan **New-AzResourceGroupDeployment**.

    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName az104-rg3 -TemplateFile template.json -TemplateParameterFile parameters.json
    ```
1. Pastikan perintah selesai dan ProvisioningState **Berhasil**.

1. Konfirmasikan bahwa disk telah dibuat.

   ```powershell
   Get-AzDisk
   ```
   
## Tugas 4: Menyebarkan templat dengan CLI 

1. Lanjutkan di **Cloud Shell** pilih **Bash**. **Konfirmasikan** pilihan Anda.

1. Verifikasi file Anda tersedia di penyimpanan Cloud Shell. Jika Anda menyelesaikan tugas sebelumnya, file templat Anda harus tersedia. 

    ```sh
    ls
    ```

1. **Pilih ikon Editor** (kurung kurawal) dan navigasikan ke file JSON templat.

1. Mengubah. Misalnya, ubah nama disk menjadi **az104-disk4**. Gunakan **Ctrl +S** untuk menyimpan perubahan Anda. 

    >**Catatan**: Anda dapat menargetkan penyebaran templat anda ke grup sumber daya, langganan, grup manajemen, atau penyewa. Tergantung pada lingkup penyebaran, Anda menggunakan perintah yang berbeda.

1. Untuk menyebarkan ke grup sumber daya, gunakan **buat grup penyebaran az**.

    ```sh
    az deployment group create --resource-group az104-rg3 --template-file template.json --parameters parameters.json
    ```
    
1. Pastikan perintah selesai dan ProvisioningState **Berhasil**.

1. Konfirmasikan bahwa disk telah dibuat.

     ```sh
     az disk list --output table
     ```
   
## Tugas 5: Menyebarkan sumber daya dengan menggunakan Azure Bicep

Dalam tugas ini, Anda akan menggunakan file Bicep untuk menyebarkan disk terkelola. Bicep adalah alat otomatisasi deklaratif yang dibangun pada templat ARM.

1. Lanjutkan bekerja di **Cloud Shell** dalam **sesi Bash** .

1. Temukan dan unduh **file \Allfiles\Lab03\azuredeploydisk.bicep** .

1. **Unggah** file bicep ke Cloud Shell. 

1. **Pilih ikon Editor** (kurung kurawal) dan navigasikan ke file.

1. Luangkan waktu satu menit untuk membaca file templat bicep. Perhatikan bagaimana sumber daya disk ditentukan. 
   
1. Lakukan perubahan berikut:

    + **Ubah nilai managedDiskName** menjadi `Disk4`.
    + **Ubah nilai nama** sku menjadi `StandardSSD_LRS`.
    + Ubah nilai **diskSizeinGiB** menjadi `32`.

1. Gunakan **Ctrl +S** untuk menyimpan perubahan Anda.

1. Sekarang, sebarkan templat.

    ```
    az deployment group create --resource-group az104-rg3 --template-file azuredeploydisk.bicep
    ```

1. Konfirmasikan bahwa disk telah dibuat.

    ```sh
    az disk list --output table
    ```

    >**Catatan:** Anda telah berhasil menyebarkan lima disk terkelola, masing-masing dengan cara yang berbeda. Kerja bagus!

## Membersihkan sumber daya Anda

Jika Anda bekerja dengan **langganan** Anda sendiri membutuhkan waktu satu menit untuk menghapus sumber daya lab. Ini akan memastikan sumber daya dibebankan dan biaya diminimalkan. Cara term mudah untuk menghapus sumber daya lab adalah dengan menghapus grup sumber daya lab. 

+ Di portal Azure, pilih grup sumber daya, pilih **Hapus grup** sumber daya, **Masukkan nama** grup sumber daya, lalu klik **Hapus**.
+ Menggunakan Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Menggunakan CLI, `az group delete --name resourceGroupName`.

## Perluas pembelajaran Anda dengan Copilot

Copilot dapat membantu Anda mempelajari cara menggunakan alat pembuatan skrip Azure. Salinan juga dapat membantu di area yang tidak tercakup dalam lab atau di mana Anda memerlukan informasi lebih lanjut. Buka browser Edge dan pilih Copilot (kanan atas) atau navigasi ke *copilot.microsoft.com*. Luangkan beberapa menit untuk mencoba perintah ini.

+ Apa format file templat Azure Resource Manager? Jelaskan setiap komponen dengan contoh. 
+ Bagaimana cara menggunakan templat Azure Resource Manager yang sudah ada?
+ Membandingkan dan membedakan templat Azure Resource Manager dan templat Azure Bicep. 


## Pelajari lebih lanjut dengan pelatihan mandiri

+ [Sebarkan infrastruktur Azure dengan menggunakan templat](https://learn.microsoft.com/training/modules/create-azure-resource-manager-template-vs-code/) JSON ARM. Menulis templat JSON Azure Resource Manager (template ARM) dengan menggunakan Visual Studio Code untuk menyebarkan infrastruktur Anda ke Azure secara konsisten dan andal.
+ [Tinjau fitur dan alat untuk Azure Cloud Shell](https://learn.microsoft.com/training/modules/review-features-tools-for-azure-cloud-shell/). Fitur dan alat Cloud Shell. 
+ [Mengelola sumber daya Azure dengan Windows PowerShell](https://learn.microsoft.com/training/modules/manage-azure-resources-windows-powershell/). Modul ini menjelaskan cara untuk menginstal modul yang diperlukan manajemen layanan cloud dan menggunakan perintah PowerShell untuk melakukan tugas administratif sederhana pada sumber daya cloud, seperti mesin virtual Azure, langganan Azure, dan akun penyimpanan Azure.
+ [Pengantar Bash](https://learn.microsoft.com/training/modules/bash-introduction/). Gunakan Bash untuk mengelola infrastruktur TI.
+ [Buat templat](https://learn.microsoft.com/training/modules/build-first-bicep-template/) Bicep pertama Anda. Tentukan sumber daya Azure dalam templat Bicep. Tingkatkan konsistensi dan keandalan penyebaran, kurangi upaya manual yang diperlukan, dan skalakan penyebaran di seluruh lingkungan. Templat Anda akan fleksibel dan dapat digunakan kembali dengan menggunakan parameter, variabel, ekspresi, dan modul.

## Poin penting

Selamat atas penyelesaian lab. Berikut adalah takeaway utama untuk lab ini. 

+ Templat Azure Resource Manager memungkinkan Anda menyebarkan, mengelola, dan memantau semua sumber daya untuk solusi Anda sebagai grup, daripada menangani sumber daya ini satu per satu.
+ Templat Azure Resource Manager adalah file JavaScript Object Notation (JSON) yang memungkinkan Anda mengelola infrastruktur secara deklaratif daripada dengan skrip.
+ Daripada meneruskan parameter sebagai nilai sebaris dalam templat Anda, Anda dapat menggunakan file JSON terpisah yang berisi nilai parameter.
+ Templat Azure Resource Manager dapat disebarkan dengan berbagai cara termasuk portal Azure, Azure PowerShell, dan CLI.
+ Bicep adalah alternatif untuk templat Azure Resource Manager. Bicep menggunakan sintaksis deklaratif untuk menyebarkan sumber daya Azure.
+ Bicep menyediakan sintaksis ringkas, keamanan jenis yang andal, dan dukungan untuk penggunaan kembali kode. Bicep menawarkan pengalaman penulisan kelas satu untuk solusi infrastruktur sebagai kode di Azure.


