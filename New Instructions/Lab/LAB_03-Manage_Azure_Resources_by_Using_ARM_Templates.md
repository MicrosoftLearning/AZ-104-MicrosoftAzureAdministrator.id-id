---
lab:
  title: 'Lab 03: Mengelola sumber daya Azure dengan menggunakan Templat Azure Resource Manager'
  module: Administer Azure Resources
---

# Lab 03 - Mengelola sumber daya Azure dengan menggunakan Templat Azure Resource Manager

## Pengenalan lab

Di lab ini, Anda mempelajari cara menentukan infrastruktur sumber daya Menggunakan templat Azure Resource Manager. Templat memastikan konsistensi dan memungkinkan Anda membuat beberapa sumber daya sekaligus. Anda belajar menyebarkan templat dengan portal Azure, Azure PowerShell, atau CLI. 

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur di lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkahnya ditulis menggunakan **US** Timur. 

## Perkiraan waktu: 40 menit

## Simulasi lab interaktif

Ada simulasi lab interaktif yang mungkin berguna bagi Anda untuk topik ini. Simulasi ini memungkinkan Anda mengklik skenario serupa dengan kecepatan Anda sendiri. Ada perbedaan antara simulasi interaktif dan lab ini, tetapi banyak konsep intinya sama. Langganan Azure tidak diperlukan. 

+ [Buat komputer virtual dengan templat](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%209). Sebarkan komputer virtual dengan templat Mulai Cepat.
  
+ [Mengelola sumber daya Azure dengan menggunakan templat](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%205) Azure Resource Manager. Tinjau, buat, dan sebarkan disk terkelola dengan templat.

## Skenario laboratorium

Tim Anda telah menjelajahi kemampuan administratif Azure dasar seperti menyediakan sumber daya dan mengaturnya berdasarkan grup sumber daya. Selanjutnya, tim Anda ingin melihat cara untuk mengotomatiskan dan menyederhanakan penyebaran. Organisasi sering mencari otomatisasi untuk mengurangi overhead administratif, mengurangi kesalahan manusia atau meningkatkan konsistensi, dan sebagai cara untuk memungkinkan administrator mengerjakan tugas yang lebih kompleks atau kreatif.

## Diagram arsitektur

![Diagram tugas.](../media/az104-lab03b-architecture-diagram.png)

## Tugas

+ Tugas 1: Buat templat Azure Resource Manager untuk penyebaran disk terkelola Azure.
+ Tugas 2: Edit templat Azure Resource Manager lalu buat disk terkelola Azure dengan menggunakan templat.
+ Tugas 3: Tinjau penyebaran berbasis templat Azure Resource Manager dari disk terkelola.
+ Tugas 4: Sebarkan disk terkelola dengan menggunakan Azure Bicep.
+ Tugas 5: Sebarkan templat dengan Azure PowerShell (opsi 1).
+ Tugas 5: Sebarkan templat dengan CLI (opsi 2). 


## Tugas 1: Membuat templat Azure Resource Manager untuk penyebaran disk terkelola Azure

Dalam tugas ini, Anda menggunakan portal Azure untuk menghasilkan templat Azure Resource Manager. Anda kemudian dapat mengunduh templat untuk digunakan dalam penyebaran mendatang. Organisasi yang berencana untuk menyebarkan ratusan atau ribuan disk dapat memanfaatkan satu atau beberapa templat untuk membantu mengotomatiskan penyebaran. 

1. Masuk ke **portal Azure** - `https://portal.azure.com`.

1. Cari dan pilih `Disks`.

1. Pada halaman Disk, pilih **Buat**.

1. Pada halaman Buat disk terkelola, gunakan informasi berikut untuk membuat disk.
    
    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | *langganan Anda* | 
    | Grup Sumber Daya | `az104-rg3` (Jika perlu, pilih **Buat baru**.)
    | Nama disk | `az104-disk1` | 
    | Wilayah | **US Timur** |
    | Zona ketersediaan | **Tidak ada redundansi infrastruktur yang diperlukan** | 
    | Jenis sumber | **Tidak** |
    | Ukuran | **32 Gb** | 
    | Performa | **HDD Standar** |

1. Klik **Tinjau + Buat** *sekali*. Jangan **** sebarkan sumber daya.

1. Setelah validasi, klik **Unduh templat untuk otomatisasi** (bagian bawah halaman).

1. Tinjau informasi yang diperlihatkan dalam templat. Tinjau tab **Templat** dan **Parameter** .

1. Klik **Unduh** dan simpan templat ke komputer Anda.

1. Ekstrak konten file yang diunduh ke **folder Unduhan** di komputer Anda. Perhatikan ada dua file JSON (templat dan parameter). 

1. Tutup semua jendela **File Explorer**.

1. Di portal Azure, batalkan penyebaran disk terkelola.

## Tugas 2: Edit templat Azure Resource Manager lalu buat disk terkelola Azure dengan menggunakan templat

Dalam tugas ini, Anda menggunakan templat yang Anda buat untuk menyebarkan disk terkelola baru. Tugas ini menguraikan proses umum memiliki penyebaran berbasis templat sehingga Anda dapat dengan cepat dan mudah mengulangi penyebaran. Jika Anda perlu mengubah parameter atau dua, Anda dapat dengan mudah memodifikasi templat di masa mendatang.

1. Di portal Azure, cari dan pilih `Deploy a custom template`.

1. Pada bilah **Penyebaran** kustom, perhatikan ada kemampuan untuk menggunakan **templat** Mulai Cepat. Ada banyak templat bawaan. Memilih templat apa pun akan memberikan deskripsi singkat.

1. Alih-alih menggunakan Mulai Cepat, pilih **Bangun templat Anda sendiri di editor**.

1. Pada panel **Edit template**, klik **Muat file** dan unggah file **template.json** yang Anda unduh di tugas sebelumnya.

1. Di dalam panel editor, hapus baris berikut. Pastikan untuk menghapus tanda kurung siku penutup dan koma. 

   ```
    "sourceResourceId": {
            "type": "string"
    },
   ```
   
   ```
      "hyperVGeneration": {
       "defaultValue": "V1",
       "type": "String"
   },      
   ```

    >**Catatan**: Parameter ini dihapus karena tidak berlaku untuk penyebaran saat ini. Parameter SourceResourceId, sourceUri, osType, dan hyperVGeneration berlaku untuk membuat disk Azure dari file VHD yang ada.

1. **Simpan** perubahan.

1. Kembali ke bilah **Penyebaran kustom**, klik **Edit parameter**. 

1. Pada panel **Edit parameter**, klik **Muat file** dan unggah file **parameters.json** yang Anda unduh di tugas sebelumnya, dan **Simpan** perubahan.

1. Kembali ke panel **Penerapan khusus**, tentukan setelan berikut:

    | Pengaturan | Nilai |
    | --- |--- |
    | Langganan | *langganan Anda* |
    | Grup Sumber Daya | `az104-rg3` |
    | Wilayah | **US Timur** |
    | Nama Disk | `az104-disk1` |
    | Lokasi | nilai parameter lokasi yang Anda catat di tugas sebelumnya |
    | SKU | **Standar_LRS** |
    | Ukuran Disk Gb | **32** |
    | Buat Opsi | **kosong** |
    | Jenis Set Enkripsi Disk | **EncryptionAtRestWithPlatformKey** |
    | Mode Autentikasi Akses Data | Tidak |
    | Azure Policy Akses Jaringan | **IzinkanSemua** |
    | Akses Jaringan Publik | Nonaktif |

1. Pilih **Ulas + buat**, lalu pilih **Buat**.

1. Verifikasi bahwa penerapan berhasil diselesaikan.

## Tugas 3: Tinjau penyebaran berbasis templat Azure Resource Manager dari disk terkelola

Dalam tugas ini, Anda memverifikasi bahwa penyebaran telah berhasil diselesaikan. Semua penyebaran sebelumnya didokumentasikan dalam grup sumber daya tempat penyebaran ditargetkan. Tinjauan ini menunjukkan detail sekeliling waktu dan lamanya penyebaran, yang dapat membantu saat pemecahan masalah. Sering kali merupakan praktik yang baik untuk meninjau beberapa penyebaran berbasis templat pertama untuk memastikan keberhasilan sebelum menggunakan templat untuk operasi skala besar.

1. Di portal Microsoft Azure, cari dan pilih **Grup sumber daya**. 

1. Dalam daftar grup sumber daya, klik **az104-rg3**.

1. Pada bilah **grup sumber daya az104-rg3**, di bagian **Pengaturan**, klik **Penyebaran**.

1. Dari bilah **az104-rg3 - Deployments** , klik entri pertama dalam daftar penyebaran dan tinjau konten **bilah Input** dan **Templat** .

1. Di portal Microsoft Azure, cari dan pilih **Disks**.

1. Perhatikan bahwa disk terkelola Anda telah dibuat.

    >**Catatan:** Anda juga dapat menyebarkan templat dari baris perintah. Tugas 4, opsi 1, memperlihatkan cara menggunakan PowerShell. Tugas 5, opsi 2, memperlihatkan cara menggunakan CLI.

## Tugas 3: Menyebarkan sumber daya dengan menggunakan Azure Bicep

Dalam tugas ini, Anda akan menggunakan file Bicep untuk menyebarkan akun penyimpanan ke grup sumber daya Anda. Bicep adalah alat otomatisasi deklaratif yang dibangun di atas templat ARM, tetapi lebih mudah dibaca dan dikerjakan.

1. Buka sesi Cloud Shell **Bash** . Jika perlu, gunakan **tautan Tingkat Lanjut** untuk mengonfigurasi penyimpanan. 

1. Pilih **ikon Unggah/Unduh File** di bilah menu Cloud Shell. Ini diwakili oleh ikon dokumen dengan panah atas dan bawah.

1. Pilih **Unggah**. Temukan direktori \Allfiles\Lab03 dan pilih file **templat Bicep azuredeploy.bicep**.

1. Untuk memverifikasi bahwa file telah diunggah, pilih ikon tanda kurung ganda di menu Cloud Shell untuk membuka editor bawaan.

1. File harus tercantum di pohon navigasi di sebelah kiri. Pilih untuk memverifikasi kontennya.

   ![Cuplikan layar file bicep di editor cloud shell.](../media/az104-lab03d-editor.png)

1. Luangkan waktu satu menit untuk membaca file templat bicep. Perhatikan bagaimana sumber daya penyimpanan (stg) ditentukan. Perhatikan bagaimana parameter dan nilai yang diizinkan dikonfigurasi.
   
1. Untuk menyebarkan file Bicep ke **grup sumber daya az104-rg3** , jalankan hal berikut:

   ```sh
   az deployment group create --resource-group az104-rg3 --template-file azuredeploy.bicep
   ```

1. Saat dimintai nilai string penyimpanan, masukkan `az104`. File templat akan menggunakan string Anda dengan string yang dihasilkan secara acak untuk membuat nama penyebaran yang unik.

1. Tutup Cloud Shell dan kembali ke Portal Microsoft Azure lengkap. 

1. Cari dan pilih **Akun** Penyimpanan. Verifikasi bahwa akun penyimpanan bernama az104** telah dibuat di **grup sumber daya az104-rg3**.**

## Tugas 5. Sebarkan templat dengan Azure PowerShell (opsi 1).

1. Buka Cloud Shell dan pilih **PowerShell**.

1. Jika perlu, gunakan **pengaturan Tingkat Lanjut** untuk membuat penyimpanan disk untuk Cloud Shell.

1. Di Cloud Shell, gunakan **ikon Unggah** untuk mengunggah file templat dan parameter. Anda harus mengunggah setiap file secara terpisah.

1. Verifikasi file Anda tersedia di penyimpanan Cloud Shell.

    ```powershell
    dir
    ```

1. Di Cloud Shell, pilih **ikon Editor** dan navigasikan ke file JSON parameter.

1. Mengubah. Misalnya, ubah nama disk menjadi **az104-disk2**. 

    >**Catatan**: Anda dapat menargetkan penyebaran templat anda ke grup sumber daya, langganan, grup manajemen, atau penyewa. Tergantung pada lingkup penyebaran, Anda menggunakan perintah yang berbeda.

1. Untuk menyebarkan ke grup sumber daya, gunakan **New-AzResourceGroupDeployment**.

    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName az104-rg3 -TemplateFile template.json -TemplateParameterFile parameters.json
    ```
1. Pastikan perintah selesai dan ProvisioningState **Berhasil**.
   
## Tugas 6: Menyebarkan templat dengan CLI (opsi 2)

1. Buka Cloud Shell dan pilih **Bash**.

1. Jika perlu, gunakan **pengaturan Tingkat Lanjut** untuk membuat penyimpanan disk untuk Cloud Shell.

1. Di Cloud Shell, gunakan **ikon Unggah** untuk mengunggah file templat dan parameter. Anda harus mengunggah setiap file secara terpisah.

1. Verifikasi file Anda tersedia di penyimpanan Cloud Shell.

    ```sh
    dir
    ```

1. Di Cloud Shell, pilih **ikon Editor** dan navigasikan ke file JSON parameter.

1. Mengubah. Misalnya, ubah nama disk menjadi **az104-disk2**. 

    >**Catatan**: Anda dapat menargetkan penyebaran templat anda ke grup sumber daya, langganan, grup manajemen, atau penyewa. Tergantung pada lingkup penyebaran, Anda menggunakan perintah yang berbeda.

1. Untuk menyebarkan ke grup sumber daya, gunakan **buat grup penyebaran az**.

    ```sh
    az deployment group create --resource-group az104-rg3 --template-file template.json --parameters parameters.json
    ```
1. Pastikan perintah selesai dan ProvisioningState **Berhasil**.



## Tinjau titik utama lab

Selamat atas penyelesaian lab. Berikut adalah takeaway utama untuk lab ini. 

+ Templat Azure Resource Manager memungkinkan Anda menyebarkan, mengelola, dan memantau semua sumber daya untuk solusi Anda sebagai grup, daripada menangani sumber daya ini satu per satu.
+ Templat Azure Resource Manager adalah file JavaScript Object Notation (JSON) yang memungkinkan Anda mengelola infrastruktur secara deklaratif daripada dengan skrip.
+ Daripada meneruskan parameter sebagai nilai sebaris dalam templat Anda, Anda dapat menggunakan file JSON terpisah yang berisi nilai parameter.
+ Templat Azure Resource Manager dapat disebarkan dengan berbagai cara termasuk portal Azure, Azure PowerShell, dan CLI.

## Membersihkan sumber daya Anda

Jika Anda bekerja dengan langganan Anda sendiri membutuhkan waktu satu menit untuk menghapus sumber daya lab. Ini akan memastikan sumber daya dibebankan dan biaya diminimalkan. Cara term mudah untuk menghapus sumber daya lab adalah dengan menghapus grup sumber daya lab. 

+ Di portal Azure, pilih grup sumber daya, pilih **Hapus grup** sumber daya, **Masukkan nama** grup sumber daya, lalu klik **Hapus**.

+ Menggunakan Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.

+ Menggunakan CLI, `az group delete --name resourceGroupName`.
