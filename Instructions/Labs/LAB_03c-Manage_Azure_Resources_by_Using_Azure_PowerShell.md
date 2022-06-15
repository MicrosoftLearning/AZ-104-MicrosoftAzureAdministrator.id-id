---
lab:
  title: 03c - Kelola sumber daya Azure dengan Menggunakan Azure PowerShell
  module: Module 03 - Azure Administration
ms.openlocfilehash: 4210a06af5b873e1031e2224239dd8738e97f23d
ms.sourcegitcommit: a8c7d995806dcf8eaad35b204e87bde178f28443
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 02/08/2022
ms.locfileid: "145198181"
---
# <a name="lab-03c---manage-azure-resources-by-using-azure-powershell"></a>Lab 03c - Kelola sumber daya Azure dengan Menggunakan Azure PowerShell
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Sekarang setelah Anda menjelajahi kemampuan administrasi Azure dasar yang terkait dengan penyediaan sumber daya dan mengaturnya berdasarkan grup sumber daya menggunakan portal Azure dan templat Azure Resource Manager, Anda perlu melakukan tugas yang setara dengan menggunakan Azure PowerShell. Untuk menghindari penginstalan modul Azure PowerShell, Anda akan memanfaatkan lingkungan PowerShell yang tersedia di Azure Cloud Shell.

## <a name="objectives"></a>Tujuan

Di lab ini Anda akan:

+ Tugas 1: Mulai sesi PowerShell di Azure Cloud Shell
+ Tugas 2: Buat grup sumber daya dan disk terkelola Azure dengan menggunakan Azure PowerShell
+ Tugas 3: Konfigurasikan disk terkelola dengan menggunakan Azure PowerShell

## <a name="estimated-timing-20-minutes"></a>Perkiraan waktu: 20 menit

## <a name="architecture-diagram"></a>Diagram arsitektur

![gambar](../media/lab03c.png)

## <a name="instructions"></a>Instruksi

> **Catatan**:  Selalu buat kata sandi aman Anda sendiri untuk mesin virtual atau akun pengguna apa pun yang Anda buat. Jika mesin virtual dibuat untuk Anda, gunakan **Setel ulang kata sandi** di Portal untuk memperbarui kata sandi. 

### <a name="exercise-1"></a>Latihan 1

#### <a name="task-1-start-a-powershell-session-in-azure-cloud-shell"></a>Tugas 1: Mulai sesi PowerShell di Azure Cloud Shell

Dalam tugas ini, Anda akan membuka sesi PowerShell di Cloud Shell. 

1. Di portal, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**. 

    >**Catatan**: Jika ini pertama kalinya Anda memulai **Cloud Shell** dan Anda melihat pesan **Anda tidak memiliki penyimpanan yang terpasang**, pilih langganan yang Anda gunakan di lab ini, dan klik **Buat penyimpanan**. 

1. Jika diminta, klik **Buat penyimpanan**, dan tunggu hingga panel Azure Cloud Shell ditampilkan. 

1. Pastikan **PowerShell** muncul di menu drop-down di sudut kiri atas panel Cloud Shell.

#### <a name="task-2-create-a-resource-group-and-an-azure-managed-disk-by-using-azure-powershell"></a>Tugas 2: Buat grup sumber daya dan disk terkelola Azure dengan menggunakan Azure PowerShell

Dalam tugas ini, Anda akan membuat grup sumber daya dan disk yang dikelola Azure dengan menggunakan sesi Azure PowerShell dalam Cloud Shell

1. Untuk membuat grup sumber daya di region Azure yang sama dengan grup sumber daya **az104-03b-rg1** yang Anda buat di lab sebelumnya, dari sesi PowerShell dalam Cloud Shell, jalankan berikut ini:

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

#### <a name="task-3-configure-the-managed-disk-by-using-azure-powershell"></a>Tugas 3: Konfigurasikan disk terkelola dengan menggunakan Azure PowerShell

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

#### <a name="clean-up-resources"></a>Membersihkan sumber daya

   >**Catatan**: Jangan hapus sumber daya yang Anda terapkan di lab ini. Anda akan merujuknya di lab berikutnya dari modul ini.

#### <a name="review"></a>Tinjau

Di lab ini, Anda telah:

- Memulai sesi PowerShell di Azure Cloud Shell
- Membuat grup sumber daya dan disk terkelola Azure dengan menggunakan Azure PowerShell
- Mengonfigurasi disk terkelola dengan menggunakan Azure PowerShell
