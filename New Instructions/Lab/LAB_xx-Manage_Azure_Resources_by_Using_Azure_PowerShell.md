---
lab:
  title: 'Lab 03c: Mengelola sumber daya Azure dengan Menggunakan Azure PowerShell (opsional)'
  module: Administer Azure Resources
---

# Lab 03c - Kelola sumber daya Azure dengan Menggunakan Azure PowerShell
# Panduan lab siswa

## Skenario laboratorium

Anda dan tim Anda telah menjelajahi kemampuan administratif Azure dasar di portal Azure seperti menyediakan sumber daya dan mengaturnya berdasarkan grup sumber daya. Anda juga menjelajahi templat Azure Resource Manager. Selanjutnya, Anda ingin mencoba tugas yang setara dengan menggunakan Azure PowerShell. Di lab ini, Anda akan memanfaatkan lingkungan PowerShell yang tersedia di Azure Cloud Shell.

**Catatan:** Tersedia **[simulasi lab interaktif](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%206)** yang memungkinkan Anda mengklik lab ini sesuai keinginan Anda. Anda mungkin menemukan sedikit perbedaan antara simulasi interaktif dan lab yang dihosting, tetapi konsep dan ide utama yang ditunjukkan sama. 

## Tujuan

Di lab ini Anda akan:

+ Tugas 1: Memulai sesi PowerShell di Azure Cloud Shell
+ Tugas 2: Membuat grup sumber daya dan disk terkelola Azure dengan menggunakan Azure PowerShell
+ Tugas 3: Mengonfigurasi disk terkelola dengan menggunakan Azure PowerShell

## Perkiraan waktu: 20 menit

## Diagram arsitektur

![Diagram arsitektur.](../media/az104-lab03c-architecture-diagram.png)

### Petunjuk

## Latihan 1

## Tugas 1: Memulai sesi PowerShell di Azure Cloud Shell

Dalam tugas ini, Anda akan membuka sesi PowerShell di Cloud Shell. Cloud Shell bawaan ke portal Azure dan memungkinkan Anda menjalankan perintah PowerShell atau Azure CLI langsung di portal tanpa perlu menginstal apa pun. Akun penyimpanan menyimpan riwayat perintah dan menyediakan lokasi untuk menyimpan skrip.

1. Di portal, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure. Secara bergantian, navigasikan langsung ke `https://shell.azure.com`.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**. 

    >**Catatan**: Jika ini adalah pertama kalinya Anda memulai **Cloud Shell** dan Anda disajikan dengan **pesan Anda tidak memiliki penyimpanan yang dipasang** , pilih langganan yang Anda gunakan di lab ini, dan klik **Buat penyimpanan**. 

1. Jika diminta, klik **Buat penyimpanan**, dan tunggu hingga panel Azure Cloud Shell ditampilkan. 

1. Pastikan **PowerShell** muncul di menu drop-down di sudut kiri atas panel Cloud Shell.

## Tugas 2: Membuat grup sumber daya dan disk terkelola Azure dengan menggunakan Azure PowerShell

Dalam tugas ini, Anda akan membuat grup sumber daya dan disk terkelola Azure dengan menggunakan sesi Azure PowerShell dalam Cloud Shell. Tugas ini dimaksudkan untuk menunjukkan bagaimana Cloud Shell dan Azure PowerShell dapat digunakan untuk membuat skrip tugas umum. Anda kemudian dapat memicu skrip dengan menggunakan Azure Automation, Logic Apps, atau alat otomatisasi lainnya.

1. PowerShell menggunakan *format Kata Benda Kata Kerja**-* untuk cmdlet. Misalnya, cmdlet untuk membuat grup sumber daya baru adalah **New-AzResourceGroup**. Untuk melihat cara menggunakan cmdlet, jalankan perintah berikut:

   ```powershell
   Get-Help New-AzResourceGroup
   ```


1. Untuk membuat grup sumber daya dari sesi PowerShell dalam Cloud Shell, jalankan perintah berikut. Perhatikan bahwa perintah yang dimulai dengan tanda dolar ($) membuat variabel yang dapat Anda gunakan dalam perintah selanjutnya.

   ```powershell
   $location = 'eastus'

   $rgName = 'az104-rg1'

   New-AzResourceGroup -Name $rgName -Location $location
   ```
   >**Catatan**: Jika **az104-rg1** sudah ada, gunakan nama yang berbeda dalam **parameter $rgName** . 

   ![Cuplikan layar buat grup sumber daya. ](../media/az104-lab03c-createrg.png)

1. Untuk mengambil properti grup sumber daya yang baru dibuat, jalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name $rgName
   ```

1. Mirip dengan membuat grup sumber daya baru, untuk membuat disk baru, gunakan **cmdlet New-AzDisk** . Untuk melihat cara menggunakan cmdlet, jalankan perintah berikut:

   ```powershell
   Get-Help New-AzDisk
   ```

1. Untuk membuat disk terkelola baru bernama **az104-disk1**, jalankan perintah berikut:

   ```powershell
   $diskConfig = New-AzDiskConfig `
    -Location $location `
    -CreateOption Empty `
    -DiskSizeGB 32 `
    -SkuName Standard_LRS

   $diskName = 'az104-disk1'

   New-AzDisk `
    -ResourceGroupName $rgName `
    -DiskName $diskName `
    -Disk $diskConfig
   ```

   ![Cuplikan layar buat disk. ](../media/az104-lab03c-createdisk.png)

1. Untuk mengambil properti disk yang baru dibuat, jalankan perintah berikut:

   ```powershell
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

## Tugas 3: Mengonfigurasi disk terkelola dengan menggunakan Azure PowerShell

Dalam tugas ini, Anda akan mengonfigurasi disk terkelola Azure dengan menggunakan sesi Azure PowerShell dalam Cloud Shell. Tugas ini dimaksudkan untuk menunjukkan bagaimana Cloud Shell dan Azure PowerShell dapat digunakan untuk membuat skrip tugas umum.

>**Apakah Anda tahu?**  Anda juga dapat menjalankan skrip dengan menggunakan Azure Automation, Logic Apps, atau alat otomatisasi lainnya.

1. Untuk meningkatkan ukuran disk terkelola Azure menjadi **64 GB**, dari sesi PowerShell dalam Cloud Shell, jalankan berikut ini:

   ```powershell
   New-AzDiskUpdateConfig -DiskSizeGB 64 | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. Untuk memverifikasi bahwa perubahan berlaku, jalankan perintah berikut:

   ```powershell
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

1. Untuk memverifikasi SKU saat ini sebagai **Standard_LRS**, jalankan perintah berikut:

   ```powershell
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

   ![Cuplikan layar sku pembaruan.](../media/az104-lab03c-updatesku.png)

1. Untuk mengubah SKU performa disk menjadi **Premium_LRS**, dari sesi PowerShell dalam Cloud Shell, jalankan perintah berikut:

   ```powershell
   New-AzDiskUpdateConfig -SkuName Premium_LRS | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. Untuk memverifikasi bahwa perubahan berlaku, jalankan perintah berikut:

   ```powershell
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

   ![Cuplikan layar pembaruan akhir sku.](../media/az104-lab03c-updatesku2.png)

>**Apakah Anda melihat?** Cmdlet PowerShell menggunakan kata kerja umum seperti **Dapatkan**, **Baru**, dan **Perbarui**. Cmdlet yang dimulai dengan  **Get** biasanya mengambil informasi tentang konfigurasi Anda. Cmdlet yang dimulai dengan **Baru** biasanya membuat sumber daya baru. Cmdlet yang dimulai dengan **Pembaruan** biasanya mengonfigurasi sumber daya yang ada.

## Tinjau

Selamat! Anda telah berhasil memulai sesi Azure Cloud Shell, membuat grup sumber daya dengan menggunakan PowerShell, membuat disk terkelola dengan menggunakan PowerShell, dan mengonfigurasi disk terkelola dengan menggunakan PowerShell.
