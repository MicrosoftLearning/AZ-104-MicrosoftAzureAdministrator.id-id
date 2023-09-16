---
lab:
  title: 'Lab 03d: Mengelola sumber daya Azure dengan Menggunakan Azure CLI (opsional)'
  module: Administer Azure Resources
---

# Lab 03d - Mengelola sumber daya Azure dengan Menggunakan Azure CLI
# Panduan lab siswa

## Skenario lab

Sekarang, setelah menjelajahi kemampuan administrasi dasar Azure yang terkait dengan provisi dan pengelolaan sumber daya berdasarkan grup sumber daya menggunakan portal Microsoft Azure, templat Azure Resource Manager, dan Azure PowerShell, Anda perlu melakukan tugas yang setara menggunakan Azure CLI. Untuk menghindari menginstal Azure CLI, Anda akan memanfaatkan lingkungan Bash yang tersedia di Azure Cloud Shell.

**Catatan:** Tersedia **[simulasi lab interaktif](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%207)** yang memungkinkan Anda mengklik lab ini sesuai keinginan Anda. Anda mungkin menemukan sedikit perbedaan antara simulasi interaktif dan lab yang dihosting, tetapi konsep dan ide utama yang ditunjukkan sama. 

>**Catatan:** Lab ini mengharuskan Lab 03b selesai.

## Tujuan

Di lab ini Anda akan:

+ Tugas 1: Memulai sesi Bash di Azure Cloud Shell
+ Tugas 2: Membuat grup sumber daya dan disk terkelola Azure dengan menggunakan Azure CLI
+ Tugas 3: Mengonfigurasi disk yang dikelola dengan menggunakan Azure CLI

## Perkiraan waktu: 20 menit

## Diagram arsitektur

![gambar](../media/lab03d.png)

### Petunjuk

## Latihan 1

## Tugas 1: Memulai sesi Bash di Azure Cloud Shell

Dalam tugas ini, Anda akan membuka sesi Bash di Cloud Shell. 

1. Dari portal, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **Bash**. 

    >**Catatan**: Jika ini pertama kalinya Anda memulai **Cloud Shell** dan Anda melihat pesan **Anda tidak memiliki penyimpanan yang terinstal**, pilih langganan yang Anda gunakan di lab ini, dan klik **Buat penyimpanan**. 

1. Jika diminta, klik **Buat penyimpanan**, dan tunggu hingga panel Azure Cloud Shell ditampilkan. 

1. Pastikan **Bash** muncul di menu menurun di sudut kiri atas panel Cloud Shell.

## Tugas 2: Membuat grup sumber daya dan disk terkelola Azure dengan menggunakan Azure CLI

Dalam tugas ini, Anda akan membuat grup sumber daya dan disk yang dikelola Azure dengan menggunakan sesi Azure CLI dalam Cloud Shell.

1. Untuk membuat grup sumber daya di region Azure yang sama dengan grup sumber daya **az104-03c-rg1** yang Anda buat di lab sebelumnya, dari sesi Bash dalam Cloud Shell, jalankan yang berikut:

   ```sh
   LOCATION=$(az group show --name 'az104-03c-rg1' --query location --out tsv)

   RGNAME='az104-03d-rg1'

   az group create --name $RGNAME --location $LOCATION
   ```
1. Untuk mengambil properti grup sumber daya yang baru dibuat, jalankan yang berikut ini:

   ```sh
   az group show --name $RGNAME
   ```
1. Untuk membuat disk terkelola baru dengan karakteristik yang sama seperti yang Anda buat di lab sebelumnya dari modul ini, dari sesi Bash dalam Cloud Shell, jalankan yang berikut:

   ```sh
   DISKNAME='az104-03d-disk1'

   az disk create \
   --resource-group $RGNAME \
   --name $DISKNAME \
   --sku 'Standard_LRS' \
   --size-gb 32
   ```
    >**Catatan**: Saat menggunakan sintaks multi-baris, pastikan bahwa setiap baris diakhiri dengan garis miring terbalik (`\`) tanpa spasi tambahan dan tidak ada spasi awal di awal setiap baris.

1. Untuk mengambil properti dari disk yang baru dibuat, jalankan yang berikut ini:

   ```sh
   az disk show --resource-group $RGNAME --name $DISKNAME
   ```

## Tugas 3: Mengonfigurasi disk yang dikelola dengan menggunakan Azure CLI

Dalam tugas ini, Anda akan mengelola konfigurasi disk terkelola Azure dengan menggunakan sesi Azure CLI dalam Cloud Shell. 

1. Untuk meningkatkan ukuran disk terkelola Azure menjadi **64 GB**, dari sesi Bash dalam Cloud Shell, jalankan yang berikut:

   ```sh
   az disk update --resource-group $RGNAME --name $DISKNAME --size-gb 64
   ```

1. Untuk memverifikasi bahwa perubahan diterapkan, jalankan yang berikut ini:

   ```sh
   az disk show --resource-group $RGNAME --name $DISKNAME --query diskSizeGB
   ```

1. Untuk mengubah SKU kinerja disk ke **Premium_LRS**, dari sesi Bash dalam Cloud Shell, jalankan yang berikut:

   ```sh
   az disk update --resource-group $RGNAME --name $DISKNAME --sku 'Premium_LRS'
   ```

1. Untuk memverifikasi bahwa perubahan diterapkan, jalankan yang berikut ini:

   ```sh
   az disk show --resource-group $RGNAME --name $DISKNAME --query sku
   ```

## Membersihkan sumber daya

 > **Catatan**: Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak digunakan akan memastikan bahwa Anda tidak akan melihat biaya yang tidak diharapkan.

 > **Catatan**:  Jangan khawatir jika sumber daya lab tidak dapat segera dihapus. Terkadang sumber daya memiliki ketergantungan dan membutuhkan waktu lama untuk dihapus. Ini adalah tugas Administrator yang umum untuk memantau penggunaan sumber daya, jadi tinjau sumber daya Anda secara berkala di Portal untuk melihat bagaimana pembersihannya. 

1. Di portal Microsoft Azure, buka sesi shell **Bash** dalam panel **Cloud Shell**.

1. Buat daftar semua grup sumber daya yang dibuat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```sh
   az group list --query "[?starts_with(name,'az104-03')].name" --output tsv
   ```

1. Hapus semua grup sumber daya yang Anda buat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```sh
   az group list --query "[?starts_with(name,'az104-03')].[name]" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
   ```

    >**Catatan**: Perintah dijalankan secara tidak sinkron (seperti yang ditentukan oleh parameter --nowait), jadi sementara Anda akan dapat menjalankan perintah Azure CLI lain segera setelah itu dalam sesi Bash yang sama, itu akan memakan waktu beberapa menit sebelum grup sumber daya benar-benar dihapus.

## Tinjau

Di lab ini, Anda telah:

- Memulai sesi Bash di Azure Cloud Shell
- Membuat grup sumber daya dan disk terkelola Azure dengan menggunakan Azure CLI
- Mengonfigurasi disk terkelola dengan menggunakan Azure CLI
