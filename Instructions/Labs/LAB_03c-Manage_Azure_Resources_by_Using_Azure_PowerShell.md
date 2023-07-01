---
lab:
  title: 'Lab 03c: Mengelola sumber daya Azure dengan Menggunakan Azure PowerShell'
  module: Administer Azure Resources
---

# Lab 03c - Mengelola sumber daya Azure dengan Menggunakan Azure PowerShell
# Panduan lab siswa

## Skenario lab

Sekarang setelah Anda menjelajahi kemampuan administrasi Azure dasar yang terkait dengan penyediaan sumber daya dan mengaturnya berdasarkan grup sumber daya menggunakan portal Azure dan templat Azure Resource Manager, Anda perlu melakukan tugas yang setara dengan menggunakan Azure PowerShell. Untuk menghindari penginstalan modul Azure PowerShell, Anda akan memanfaatkan lingkungan PowerShell yang tersedia di Azure Cloud Shell.

**Catatan:** Tersedia **[simulasi lab interaktif](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%206)** yang memungkinkan Anda mengklik lab ini sesuai keinginan Anda. Anda mungkin menemukan sedikit perbedaan antara simulasi interaktif dan lab yang dihosting, tetapi konsep dan ide utama yang ditunjukkan sama. 

## Tujuan

Di lab ini Anda akan:

+ Tugas 1: Memulai sesi PowerShell di Azure Cloud Shell
+ Tugas 2: Membuat grup sumber daya dan disk terkelola Azure dengan menggunakan Azure PowerShell
+ Tugas 3: Mengonfigurasi disk terkelola dengan menggunakan Azure PowerShell

## Perkiraan waktu: 20 menit

## Diagram arsitektur

![gambar](../media/lab03c.png)

### Instruksi

> **Catatan**:  Selalu buat kata sandi aman Anda sendiri untuk mesin virtual atau akun pengguna apa pun yang Anda buat. Jika mesin virtual dibuat untuk Anda, gunakan **Atur ulang kata sandi** di Portal untuk memperbarui kata sandi. 

## Latihan 1

## Tugas 1: Memulai sesi PowerShell di Azure Cloud Shell

Dalam tugas ini, Anda akan membuka sesi PowerShell di Cloud Shell. 

1. Di portal, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**. 

    >**Catatan**: Jika ini pertama kalinya Anda memulai **Cloud Shell** dan Anda melihat pesan **Anda tidak memiliki penyimpanan yang terinstal**, pilih langganan yang Anda gunakan di lab ini, dan klik **Buat penyimpanan**. 

1. Jika diminta, klik **Buat penyimpanan**, dan tunggu hingga panel Azure Cloud Shell ditampilkan. 

1. Pastikan **PowerShell** muncul di menu drop-down di sudut kiri atas panel Cloud Shell.

## Tugas 2: Membuat grup sumber daya dan disk terkelola Azure dengan menggunakan Azure PowerShell

Dalam tugas ini, Anda akan membuat grup sumber daya dan disk yang dikelola Azure dengan menggunakan sesi Azure PowerShell dalam Cloud Shell

1. Untuk membuat grup sumber daya di wilayah Azure yang sama dengan grup sumber daya **az104-03b-rg1** yang Anda buat di lab sebelumnya, dari sesi PowerShell dalam Cloud Shell, jalankan berikut ini:

   ```powershell
   $location = (Get-AzResourceGroup -Name az104-03b-rg1).Location

   $rgName = 'az104-03c-rg1'

   New-AzResourceGroup -Name $rgName -Location $location
   ```
1. Untuk mengambil properti grup sumber daya yang baru dibuat, jalankan berikut ini:

   ```powershell
   Get-AzResourceGroup -Name $rgName
   ```
1. Untuk membuat disk terkelola baru dengan karakteristik yang sama seperti yang Anda buat di lab modul ini sebelumnya, jalankan berikut ini:

   ```powershell
   $diskConfig = New-AzDiskConfig `
    -Location $location `
    -CreateOption Empty `
    -DiskSizeGB 32 `
    -Sku Standard_LRS

   $diskName = 'az104-03c-disk1'

   New-AzDisk `
    -ResourceGroupName $rgName `
    -DiskName $diskName `
    -Disk $diskConfig
   ```

1. Untuk mengambil properti dari disk yang baru dibuat, jalankan berikut ini:

   ```powershell
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

## Tugas 3: Mengonfigurasi disk terkelola dengan menggunakan Azure PowerShell

Dalam tugas ini, Anda akan mengelola konfigurasi disk terkelola Azure dengan menggunakan sesi Azure PowerShell dalam Cloud Shell. 

1. Untuk meningkatkan ukuran disk terkelola Azure menjadi **64 GB**, dari sesi PowerShell dalam Cloud Shell, jalankan berikut ini:

   ```powershell
   New-AzDiskUpdateConfig -DiskSizeGB 64 | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. Untuk memverifikasi bahwa perubahan diterapkan, jalankan berikut ini:

   ```powershell
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

1. Untuk memverifikasi SKU saat ini sebagai **Standard_LRS**, jalankan berikut ini:

   ```powershell
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

1. Untuk mengubah SKU kinerja disk ke **Premium_LRS**, dari sesi PowerShell dalam Cloud Shell, jalankan berikut ini:

   ```powershell
   New-AzDiskUpdateConfig -Sku Premium_LRS | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. Untuk memverifikasi bahwa perubahan diterapkan, jalankan berikut ini:

   ```powershell
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

## Membersihkan sumber daya

   >**Catatan**: Jangan hapus sumber daya yang Anda terapkan di lab ini. Anda akan merujuknya di lab berikutnya dari modul ini.

## Tinjau

Di lab ini, Anda telah:

- Memulai sesi PowerShell di Azure Cloud Shell
- Membuat grup sumber daya dan disk terkelola Azure dengan menggunakan Azure PowerShell
- Mengonfigurasi disk terkelola dengan menggunakan Azure PowerShell
