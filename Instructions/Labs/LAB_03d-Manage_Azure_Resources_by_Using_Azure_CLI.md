---
lab:
  title: 03d - Mengelola sumber daya Azure dengan Menggunakan Azure CLI
  module: Module 03 - Azure Administration
ms.openlocfilehash: e673423e49d49629c72f1b28a234d82eb776190f
ms.sourcegitcommit: c360d3abaa6e09814f051b2568340e80d0d0e953
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 02/09/2022
ms.locfileid: "145198196"
---
# <a name="lab-03d---manage-azure-resources-by-using-azure-cli"></a>Lab 03d - Mengelola sumber daya Azure dengan Menggunakan Azure CLI
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Sekarang, setelah menjelajahi kemampuan administrasi dasar Azure yang terkait dengan provisi dan pengelolaan sumber daya berdasarkan grup sumber daya menggunakan portal Microsoft Azure, templat Azure Resource Manager, dan Azure PowerShell, Anda perlu melakukan tugas yang setara menggunakan Azure CLI. Untuk menghindari menginstal Azure CLI, Anda akan memanfaatkan lingkungan Bash yang tersedia di Azure Cloud Shell.

## <a name="objectives"></a>Tujuan

Di lab ini Anda akan:

+ Tugas 1: Mulai sesi Bash di Azure Cloud Shell
+ Tugas 2: Buat grup sumber daya dan disk terkelola Azure dengan menggunakan Azure CLI
+ Tugas 3: Konfigurasikan disk yang dikelola dengan menggunakan Azure CLI

## <a name="estimated-timing-20-minutes"></a>Perkiraan waktu: 20 menit

## <a name="instructions"></a>Instruksi

### <a name="exercise-1"></a>Latihan 1

#### <a name="task-1-start-a-bash-session-in-azure-cloud-shell"></a>Tugas 1: Mulai sesi Bash di Azure Cloud Shell

Dalam tugas ini, Anda akan membuka sesi Bash di Cloud Shell. 

1. Dari portal, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **Bash**. 

    >**Catatan**: Jika ini pertama kalinya Anda memulai **Cloud Shell** dan Anda melihat pesan **Anda tidak memiliki penyimpanan yang terpasang**, pilih langganan yang Anda gunakan di lab ini, dan klik **Buat penyimpanan**. 

1. Jika diminta, klik **Buat penyimpanan**, dan tunggu hingga panel Azure Cloud Shell ditampilkan. 

1. Pastikan **Bash** muncul di menu tarik-turun di sudut kiri atas panel Cloud Shell.

#### <a name="task-2-create-a-resource-group-and-an-azure-managed-disk-by-using-azure-cli"></a>Tugas 2: Buat grup sumber daya dan disk terkelola Azure dengan menggunakan Azure CLI

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

#### <a name="task-3-configure-the-managed-disk-by-using-azure-cli"></a>Tugas 3: Konfigurasikan disk yang dikelola dengan menggunakan Azure CLI

Dalam tugas ini, Anda akan mengelola konfigurasi disk terkelola Azure dengan menggunakan sesi Azure CLI dalam Cloud Shell. 

1. Untuk meningkatkan ukuran disk terkelola Azure menjadi **64 GB**, dari sesi Bash dalam Cloud Shell, jalankan yang berikut:

   ```sh
   az disk update --resource-group $RGNAME --name $DISKNAME --size-gb 64
   ```

1. Untuk memverifikasi bahwa perubahan diterapkan, jalankan yang berikut ini:

   ```sh
   az disk show --resource-group $RGNAME --name $DISKNAME --query diskSizeGb
   ```

1. Untuk mengubah SKU kinerja disk ke **Premium_LRS**, dari sesi Bash dalam Cloud Shell, jalankan yang berikut:

   ```sh
   az disk update --resource-group $RGNAME --name $DISKNAME --sku 'Premium_LRS'
   ```

1. Untuk memverifikasi bahwa perubahan diterapkan, jalankan yang berikut ini:

   ```sh
   az disk show --resource-group $RGNAME --name $DISKNAME --query sku
   ```

#### <a name="clean-up-resources"></a>Membersihkan sumber daya

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

#### <a name="review"></a>Tinjau

Di lab ini, Anda telah:

- Memulai sesi Bash di Azure Cloud Shell
- Membuat grup sumber daya dan disk terkelola Azure dengan menggunakan Azure CLI
- Mengonfigurasi disk terkelola dengan menggunakan Azure CLI
