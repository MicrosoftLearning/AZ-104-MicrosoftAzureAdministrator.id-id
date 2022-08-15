---
lab:
  title: 09a - Menerapkan Web Apps
  module: Module 09 - Serverless Computing
ms.openlocfilehash: af243b0cfa2b011dd419516139b5200ba349bcb4
ms.sourcegitcommit: c360d3abaa6e09814f051b2568340e80d0d0e953
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 02/09/2022
ms.locfileid: "145198199"
---
# <a name="lab-09a---implement-web-apps"></a>Lab 09a - Menerapkan Aplikasi Web
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Anda perlu mengevaluasi penggunaan aplikasi Web Azure untuk menghosting situs web Contoso, yang saat ini dihosting di pusat data lokal perusahaan. Situs web berjalan di server Windows menggunakan tumpukan runtime PHP. Anda juga perlu menentukan bagaimana Anda dapat menerapkan praktik DevOps dengan memanfaatkan slot penyebaran aplikasi web Azure.

## <a name="objectives"></a>Tujuan

Di lab ini Anda akan:

+ Tugas 1: Membuat aplikasi web Azure
+ Tugas 2: Menukar slot pentahapan
+ Tugas 3: Mengonfigurasikan pengaturan penyebaran aplikasi web
+ Tugas 4: Menyebarkan kode ke slot pentahapan pementasan
+ Tugas 5: Menukar slot pentahapan
+ Tugas 6: Mengonfigurasi dan menguji penskalaan otomatis aplikasi web Azure

## <a name="estimated-timing-30-minutes"></a>Perkiraan waktu: 30 menit

## <a name="architecture-diagram"></a>Diagram arsitektur

![gambar](../media/lab09a.png)

## <a name="instructions"></a>Instruksi

### <a name="exercise-1"></a>Latihan 1

#### <a name="task-1-create-an-azure-web-app"></a>Tugas 1: Membuat aplikasi web Azure

Dalam tugas ini, Anda akan membuat aplikasi web Azure.

1. Masuk ke [**portal Microsoft Azure**](http://portal.azure.com).

1. Di portal Microsoft Azure, cari dan pilih **Layanan aplikasi**, dan, pada panel **App Services**, klik **+ Buat**.

1. Pada tab **Dasar-dasar** panel **Buat Aplikasi Web**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | ---|
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | nama grup sumber daya baru **az104-09a-rg1** |
    | Nama aplikasi web | nama unik global apa pun |
    | Terbitkan | **Kode** |
    | Tumpukan runtime | **PHP 7.4** |
    | Sistem operasi | **Windows** |
    | Wilayah | nama wilayah Azure tempat Anda dapat memprovisikan aplikasi web Azure |
    | Paket App service | menerima konfigurasi default |

1. Klik **Tinjauan + buat**. Pada tab **Tinjauan + buat** panel **Buat Aplikasi Web**, pastikan validasi lulus dan klik **Buat**.

    >**Catatan**: Tunggu hingga aplikasi web dibuat sebelum Anda melanjutkan ke tugas berikutnya. Ini akan memakan waktu sekitar satu menit.

1. Pada panel penyebaran, klik **Buka sumber daya**.

#### <a name="task-2-create-a-staging-deployment-slot"></a>Tugas 2: Membuat slot penyebaran pentahapan

Dalam tugas ini, Anda akan membuat slot penyebaran pentahapan.

1. Pada panel aplikasi web yang baru disebarkan, klik tautan **URL** untuk menampilkan halaman web default di tab browser baru.

1. Tutup tab browser baru dan, kembali ke portal Microsoft Azure, di bagian **Penyebaran** panel aplikasi web, klik **Slot penyebaran**.

    >**Catatan**: Aplikasi web, pada titik ini, memiliki satu slot penerapan berlabel **PRODUCTION**.

1. Klik **+ Tambahkan slot**, dan tambahkan slot baru dengan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | ---|
    | Nama | **Pentahapan** |
    | Klon pengaturan dari | **Jangan klon pengaturan**|

1. Kembali ke penyebaran **Slot penyebaran** aplikasi web, klik entri yang mewakili slot pentahapan yang baru dibuat.

    >**Catatan**: Ini akan membuka panel yang menampilkan properti slot pentahapan.

1. Tinjauan panel slot pentahapan dan perhatikan bahwa URL-nya berbeda dari yang ditetapkan ke slot produksi.

#### <a name="task-3-configure-web-app-deployment-settings"></a>Tugas 3: Konfigurasikan pengaturan penyebaran aplikasi web

Dalam tugas ini, Anda akan mengonfigurasi pengaturan penyebaran aplikasi web.

1. Pada panel slot penyebaran pentahapan, di bagian **Penyebaran**, klik **Pusat Penyebaran** lalu pilih tab **Pengaturan**.

    >**Catatan:** Pastikan Anda berada di panel slot pentahapan (bukan slot produksi).
    
1. Pada tab **Pengaturan**, di daftar menurun **Sumber**, pilih **Git Lokal** dan klik tombol **Simpan**

1. Pada panel **Pusat Penyebaran**, salin entri **Url Klon Git** ke Notepad.

    >**Catatan:** Anda akan membutuhkan nilai Git Clone Url dalam tugas lab ini selanjutnya.

1. Pada panel **Pusat Penyebaran**, pilih tab **Informasi masuk Git/FTPS Lokal**, di bagian **Cakupan Pengguna**, tentukan pengaturan berikut, dan klik **Simpan**.

    | Pengaturan | Nilai |
    | --- | ---|
    | Nama pengguna | nama unik global apa pun (tidak boleh berisi karakter `@`) |
    | Kata sandi | kata sandi apa pun yang memenuhi persyaratan kompleksitas|

    >**Catatan:** Kata sandi harus setidaknya delapan karakter, dengan dua dari tiga elemen berikut: huruf, angka, dan karakter non-alfanumerik.

    >**Catatan:** Anda akan memerlukan informasi masuk ini dalam tugas lab ini selanjutnya.

#### <a name="task-4-deploy-code-to-the-staging-deployment-slot"></a>Tugas 4: Sebarkan kode ke slot pentahapan pementasan

Dalam tugas ini, Anda akan menyebarkan kode ke slot penyebaran pentahapan.

1. Di portal Microsoft Azure, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**.

    >**Catatan**: Jika ini pertama kalinya Anda memulai **Cloud Shell** dan Anda melihat pesan **Anda tidak memiliki penyimpanan yang terinstal**, pilih langganan yang Anda gunakan di lab ini, dan klik **Buat penyimpanan**.

1. Dari panel Cloud Shell, jalankan yang berikut ini untuk mengkloning repositori jarak jauh yang berisi kode untuk aplikasi web.

   ```powershell
   git clone https://github.com/Azure-Samples/php-docs-hello-world
   ```

1. Dari panel Cloud Shell, jalankan yang berikut ini untuk mengatur lokasi saat ini ke klon repositori lokal yang baru dibuat yang berisi kode aplikasi web sampel.

   ```powershell
   Set-Location -Path $HOME/php-docs-hello-world/
   ```

1. Dari panel Cloud Shell, jalankan perintah berikut untuk menambahkan git jarak jauh (pastikan untuk mengganti tempat penampung `[deployment_user_name]` dan `[git_clone_url]` dengan nilai nama pengguna **Informasi masuk Penyebaran** dan **Git Clone Url** , masing-masing, yang Anda identifikasi di tugas sebelumnya):

   ```powershell
   git remote add [deployment_user_name] [git_clone_url]
   ```

    >**Catatan**: Nilai berikut `git remote add` ini tidak harus cocok dengan nama pengguna **Informasi Masuk Penyebaran**, tetapi harus unik

1. Dari panel Cloud Shell, jalankan yang berikut ini untuk mendorong sampel kode aplikasi web dari repositori lokal ke slot penyebaran penahapan aplikasi web Azure (pastikan untuk mengganti tempat penampung `[deployment_user_name]` dengan nilai nama pengguna **Informasi masuk Penyebaran**, yang Anda identifikasi di tugas sebelumnya):

   ```powershell
   git push [deployment_user_name] master
   ```

1. Jika diminta untuk mengautentikasi, ketik `[deployment_user_name]` dan sandi yang sesuai (yang Anda tetapkan di tugas sebelumnya).

1. Tutup panel Cloud Shell.

1. Pada panel slot pentahapan, klik **Ringkasan** lalu klik tautan **URL** untuk menampilkan laman web bawaan di tab browser baru.

1. Verifikasi bahwa halaman browser menampilkan **Halo Dunia** pesan dan tutup tab baru.

#### <a name="task-5-swap-the-staging-slots"></a>Tugas 5: Menukar slot pentahapan

Dalam tugas ini, Anda akan menukar slot pentahapan dengan slot produksi

1. Navigasi kembali ke panel yang menampilkan slot produksi aplikasi web.

1. Di bagian **Penyebaran**, klik **Slot penyebaran**, lalu klik ikon toolbar **Tukar**.

1. Pada panel **Tukar**, tinjau pengaturan default dan klik **Tukar**.

1. Klik **Gambaran Umum** pada panel slot produksi aplikasi web lalu klik tautan **URL** untuk menampilkan halaman beranda situs web di tab browser baru.

1. Pastikan halaman web default telah diganti dengan **Halo Dunia!** halaman.

#### <a name="task-6-configure-and-test-autoscaling-of-the-azure-web-app"></a>Tugas 6: Mengonfigurasi dan menguji penskalaan otomatis aplikasi web Azure

Dalam tugas ini, Anda akan mengonfigurasi dan menguji penskalaan otomatis aplikasi web Azure.

1. Pada panel yang menampilkan slot produksi aplikasi web, di bagian **Pengaturan**, klik **Peluasan skala (paket App Service)** .

1. Klik **Skala otomatis kustom**.

    >**Catatan**: Anda juga memiliki opsi untuk menskalakan aplikasi web secara manual.

1. Biarkan opsi default **Menskalakan berdasarkan metrik** yang dipilih dan klik **+ Tambahkan aturan**

1. Pada panel **Aturan skala**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- |--- |
    | Sumber metrik | **Sumber daya saat ini** |
    | Agregasi waktu | **Maksimum** |
    | Namespace layanan metrik | **Metrik standar paket App Service** |
    | Nama metrik | **Persentase CPU** |
    | Operator | **Lebih besar dari** |
    | Ambang metrik untuk memicu tindakan skala | **10** |
    | Durasi (dalam menit) | **1** |
    | Statistik butir waktu | **Maksimum** |
    | Operasi | **Tingkatkan jumlah sebesar** |
    | Jumlah instans | **1** |
    | Pendinginan (menit) | **5** |

    >**Catatan**: Jelas nilai-nilai ini tidak mewakili konfigurasi yang realistis, karena tujuannya adalah untuk memicu penskalaan otomatis sesegera mungkin, tanpa masa tunggu yang diperpanjang.

1. Klik **Tambahkan** dan, kembali pada panel penskalaan paket App Service, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- |--- |
    | Batas instans Minimum | **1** |
    | Batas instans Maksimum | **2** |
    | Batas instans Default | **1** |

1. Klik **Simpan**.

    >**Catatan**: Jika Anda mendapatkan kesalahan saat mengeluh tentang penyedia sumber daya 'microsoft.insights' yang tidak terdaftar, jalankan `az provider register --namespace 'Microsoft.Insights'` di cloudshell Anda dan coba simpan kembali aturan skala otomatis Anda.

1. Di portal Microsoft Azure, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**.

1. Dari panel Cloud Shell, jalankan yang berikut ini untuk mengidentifikasi URL aplikasi web Azure.

   ```powershell
   $rgName = 'az104-09a-rg1'

   $webapp = Get-AzWebApp -ResourceGroupName $rgName
   ```

1. Dari panel Cloud Shell, jalankan yang berikut ini untuk memulai dan perulangan tak terbatas yang mengirim permintaan HTTP ke aplikasi web:

   ```powershell
   while ($true) { Invoke-WebRequest -Uri $webapp.DefaultHostName }
   ```

1. Minimalkan panel Cloud Shell (tetapi jangan tutup) dan, pada panel aplikasi web, di bagian **Pemantauan**, klik **Penjelajah proses**.

    >**Catatan**: Penjelajah proses memfasilitasi pemantauan jumlah instans dan pemanfaatan sumber dayanya.

1. Pantau pemanfaatan dan jumlah instans selama beberapa menit.

    >**Catatan**: Anda mungkin perlu melakukan **Refresh** halamannya.

1. Setelah Anda melihat bahwa jumlah instans telah meningkat menjadi 2, buka kembali panel Cloud Shell dan hentikan skrip dengan menekan **Ctrl+C**.

1. Tutup panel Cloud Shell.

#### <a name="clean-up-resources"></a>Membersihkan sumber daya

>**Catatan**: Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak digunakan akan memastikan bahwa Anda tidak akan melihat biaya yang tidak diharapkan.

>**Catatan**:  Jangan khawatir jika sumber daya lab tidak dapat segera dihapus. Terkadang sumber daya memiliki ketergantungan dan membutuhkan waktu lama untuk dihapus. Ini adalah tugas Administrator yang umum untuk memantau penggunaan sumber daya, jadi tinjau sumber daya Anda secara berkala di Portal untuk melihat bagaimana pembersihannya. 

1. Di portal Microsoft Azure, buka sesi **PowerShell** dalam panel **Cloud Shell**.

1. Buat daftar semua grup sumber daya yang dibuat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-09a*'
   ```

1. Hapus semua grup sumber daya yang Anda buat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-09a*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Catatan**: Perintah dijalankan secara asinkron (sebagaimana yang ditentukan oleh parameter -AsJob), jadi saat Anda akan dapat menjalankan perintah PowerShell lain langsung setelahnya dalam sesi PowerShell yang sama, proses ini akan memakan waktu beberapa menit sebelum grup sumber daya benar-benar dihapus.

#### <a name="review"></a>Tinjauan

Di lab ini, Anda telah:

+ Membuat aplikasi web Azure
+ Menukar slot pentahapan
+ Mengonfigurasikan pengaturan penyebaran aplikasi web
+ Menyebarkan kode ke slot penyebaran pentahapan
+ Tukar slot pentahapan
+ Mengonfigurasi dan menguji penskalaan otomatis aplikasi web Azure
