---
lab:
  title: 09b - Menerapkan Azure Container Instances
  module: Module 09 - Serverless Computing
ms.openlocfilehash: 603b8b0b4777e3879c00f95771e519a5843ccbac
ms.sourcegitcommit: c360d3abaa6e09814f051b2568340e80d0d0e953
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 02/09/2022
ms.locfileid: "145198177"
---
# <a name="lab-09b---implement-azure-container-instances"></a>Lab 09b - Menerapkan Azure Container Instances
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Contoso ingin menemukan platform baru untuk beban kerja virtualnya. Anda mengidentifikasi sejumlah gambar kontainer yang dapat dimanfaatkan untuk mencapai tujuan ini. Karena Anda ingin meminimalkan manajemen kontainer, Anda berencana untuk mengevaluasi penggunaan Azure Container Instances untuk penyebaran gambar Docker.

## <a name="objectives"></a>Tujuan

Di lab ini Anda akan:

- Tugas 1: Terapkan gambar Docker dengan menggunakan Azure Container Instance
- Tugas 2: Tinjauan fungsionalitas Instans Kontainer Azure

## <a name="estimated-timing-20-minutes"></a>Perkiraan waktu: 20 menit

## <a name="architecture-diagram"></a>Diagram arsitektur

![gambar](../media/lab09b.png)

## <a name="instructions"></a>Instruksi

### <a name="exercise-1"></a>Latihan 1

#### <a name="task-1-deploy-a-docker-image-by-using-the-azure-container-instance"></a>Tugas 1: Terapkan gambar Docker dengan menggunakan Azure Container Instance

Dalam tugas ini, Anda akan membuat instans kontainer baru untuk aplikasi web.

1. Masuk ke [portal Microsoft Azure](https://portal.azure.com).

1. Di portal Azure, jelajahi **Instans kontainer** lalu, pada bilah **Instans kontainer**, klik **+ Buat**.

1. Pada tab **Dasar** pada bilah **Buat instans penampung**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | ---- | ---- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | nama grup sumber daya baru **az104-09b-rg1** |
    | Nama kontainer | **az104-9b-c1** |
    | Wilayah | nama wilayah tempat Anda dapat menyediakan instans kontainer Azure |
    | Sumber Gambar | **Gambar memulai cepat** |
    | Gambar | **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)** |

1. Klik **Berikutnya: Jaringan >** dan, pada tab **Jaringan** bilah **Buat instans penampung**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Label nama DNS | nama host DNS yang valid dan unik secara global |

    >**Catatan**: Kontainer Anda akan dapat dijangkau publik di dns-name-label.region.azurecontainer.io. Jika Anda menerima pesan error **DNS name label not available**, tentukan nilai yang berbeda.

1. Klik **Berikutnya: Lanjutan >** , tinjau pengaturan pada tab **Lanjutan** pada bilah **Buat instans penampung** tanpa membuat perubahan apa pun, klik **Tinjau + Buat**, pastikan validasi lulus dan klik **Buat**.

    >**Catatan**: Tunggu hingga penyebaran selesai. Ini akan memakan waktu sekitar 3 menit.

    >**Catatan**: Sambil menunggu, Anda mungkin tertarik untuk melihat [kode di balik contoh aplikasi](https://github.com/Azure-Samples/aci-helloworld). Untuk melihatnya, jelajahi \\folder aplikasi.

#### <a name="task-2-review-the-functionality-of-the-azure-container-instance"></a>Tugas 2: Tinjauan fungsionalitas Instans Kontainer Azure

Dalam tugas ini, Anda akan meninjau penyebaran instans kontainer.

1. Pada bilah penyebaran, klik tautan **Buka sumber daya**.

1. Pada bilah **Ringkasan** instans penampung, verifikasi bahwa **Status** dilaporkan sebagai **Berjalan**.

1. Salin nilai instans kontainer **FQDN**, buka tab browser baru, dan navigasikan ke URL yang sesuai.

1. Pastikan halaman **Selamat datang di Azure Container Instance** ditampilkan.

1. Tutup tab browser baru, kembali ke portal Azure, di bagian **Pengaturan** bilah instans kontainer, klik **Kontainer**, lalu klik **Log**.

1. Verifikasi bahwa Anda melihat entri log yang mewakili permintaan HTTP GET yang dihasilkan dengan menampilkan aplikasi di browser.

#### <a name="clean-up-resources"></a>Membersihkan sumber daya

>**Catatan**: Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak digunakan akan memastikan bahwa Anda tidak akan melihat biaya yang tidak diharapkan.

>**Catatan**:  Jangan khawatir jika sumber daya lab tidak dapat segera dihapus. Terkadang sumber daya memiliki ketergantungan dan membutuhkan waktu lama untuk dihapus. Ini adalah tugas Administrator yang umum untuk memantau penggunaan sumber daya, jadi tinjau sumber daya Anda secara berkala di Portal untuk melihat bagaimana pembersihannya. 

1. Di portal Microsoft Azure, buka sesi **PowerShell** dalam panel **Cloud Shell**.

1. Buat daftar semua grup sumber daya yang dibuat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-09b*'
   ```

1. Hapus semua grup sumber daya yang Anda buat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-09b*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Catatan**: Perintah dijalankan secara asinkron (sebagaimana yang ditentukan oleh parameter -AsJob), jadi saat Anda akan dapat menjalankan perintah PowerShell lain langsung setelahnya dalam sesi PowerShell yang sama, proses ini akan memakan waktu beberapa menit sebelum grup sumber daya benar-benar dihapus.

#### <a name="review"></a>Tinjauan

Di lab ini, Anda telah:

- Menyebarkan gambar Docker dengan menggunakan Azure Container Instance
- Meninjau fungsionalitas Instans Kontainer Azure
