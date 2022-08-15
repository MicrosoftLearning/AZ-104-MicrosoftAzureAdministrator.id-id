---
lab:
  title: 07 - Mengelola penyimpanan Azure
  module: Module 07 - Azure Storage
ms.openlocfilehash: 34b6dba73d87731df935f80a1b5909e44075e871
ms.sourcegitcommit: 6df80c7697689bcee3616cdd665da0a38cdce6cb
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 06/26/2022
ms.locfileid: "146587466"
---
# <a name="lab-07---manage-azure-storage"></a>Lab 07 - Mengelola Azure Storage
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Anda perlu mengevaluasi penggunaan penyimpanan Azure untuk menyimpan file yang saat ini berada di penyimpanan data lokal. Meskipun sebagian besar file ini tidak sering diakses, ada beberapa pengecualian. Anda ingin meminimalkan biaya penyimpanan dengan menempatkan file yang jarang diakses di tingkat penyimpanan dengan harga lebih rendah. Anda juga berencana untuk menjelajahi berbagai mekanisme perlindungan yang ditawarkan Azure Storage, termasuk akses jaringan, autentikasi, otorisasi, dan replikasi. Terakhir, Anda ingin menentukan sejauh mana layanan Azure Files mungkin cocok untuk menghosting berbagi file lokal Anda.

## <a name="objectives"></a>Tujuan

Di lab ini Anda akan:

+ Tugas 1: Memprovisikan lingkungan lab
+ Tugas 2: Membuat dan mengonfigurasi akun Azure Storage
+ Tugas 3: Mengelola penyimpanan blob
+ Tugas 4: Mengelola autentikasi dan otorisasi Azure Storage
+ Tugas 5: Membuat dan mengonfigurasi berbagi Azure Files
+ Tugas 6: Mengelola akses jaringan Azure Storage

## <a name="estimated-timing-40-minutes"></a>Perkiraan waktu: 40 menit

## <a name="architecture-diagram"></a>Diagram arsitektur

![gambar](../media/lab07.png)


## <a name="instructions"></a>Instruksi

### <a name="exercise-1"></a>Latihan 1

#### <a name="task-1-provision-the-lab-environment"></a>Tugas 1: Memprovisikan lingkungan lab

Dalam tugas ini, Anda akan menerapkan komputer virtual Azure yang akan Anda gunakan nanti di lab ini.

1. Masuk ke [portal Microsoft Azure](https://portal.azure.com).

1. Di portal Microsoft Azure, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**.

    >**Catatan**: Jika ini pertama kalinya Anda memulai **Cloud Shell** dan Anda melihat pesan **Anda tidak memiliki penyimpanan yang terinstal**, pilih langganan yang Anda gunakan di lab ini, dan klik **Buat penyimpanan**.

1. Di bilah alat panel Cloud Shell, klik ikon **Unggah/Unduh file**, di menu menurun, klik **Unggah** dan unggah file **\\Allfiles\\Labs\\07\\az104-07-vm-template.json** dan **\\Allfiles\\Labs\\07\\az104-07-vm-parameters.json** ke direktori beranda Cloud Shell.

1. Edit file **Parameter** yang baru saja Anda unggah dan ubah kata sandinya. Jika memerlukan bantuan untuk mengedit file di Shell, mintalah bantuan instruktur Anda. Sebagai praktik terbaik, rahasia, seperti kata sandi, harus disimpan dengan lebih aman di Key Vault. 

1. Dari panel Cloud Shell, jalankan elemen berikut ini untuk membuat grup sumber daya yang akan menghosting komputer virtual (ganti tempat penampung '[Azure_region]' dengan nama wilayah Azure tempat Anda ingin menerapkan komputer virtual Azure)

    >**Catatan**: Untuk mencantumkan nama wilayah Azure, jalankan `(Get-AzLocation).Location`
    >**Catatan**: Setiap perintah di bawah ini harus diketik secara terpisah

    ```powershell
    $location = '[Azure_region]'
    ```
  
    ```powershell
     $rgName = 'az104-07-rg0'
    ```

    ```powershell
    New-AzResourceGroup -Name $rgName -Location $location
    ```
    
1. Dari panel Cloud Shell, jalankan perintah berikut untuk menyebarkan komputer virtual menggunakan templat yang diupload dan file parameter:

   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-07-vm-template.json `
      -TemplateParameterFile $HOME/az104-07-vm-parameters.json `
      -AsJob
   ```

    >**Catatan**: Jangan menunggu penerapan selesai, tetapi lanjutkan ke tugas berikutnya.

    >**Catatan**: Jika mendapatkan kesalahan yang menyatakan ukuran VM tidak tersedia, mintalah bantuan instruktur Anda dan coba langkah-langkah ini.
    > 1. Klik tombol `{}` di CloudShell Anda, pilih **az104-07-vm-parameters.json** dari bilah sisi kiri dan catat nilai parameter `vmSize`.
    > 1. Periksa lokasi di mana grup sumber daya 'az104-04-rg1' diterapkan. Anda dapat menjalankan `az group show -n az104-04-rg1 --query location` di CloudShell Anda untuk mendapatkannya.
    > 1. Jalankan `az vm list-skus --location <Replace with your location> -o table --query "[? contains(name,'Standard_D2s')].name"` di CloudShell Anda.
    > 1. Ganti nilai parameter `vmSize` dengan salah satu nilai yang dikembalikan oleh perintah yang baru saja Anda jalankan.
    > 1. Sekarang sebarkan ulang template Anda dengan menjalankan perintah `New-AzResourceGroupDeployment` lagi. Anda dapat menekan tombol atas beberapa kali yang akan membawa ke perintah yang dieksekusi terakhir.

1. Tutup panel Cloud Shell.

#### <a name="task-2-create-and-configure-azure-storage-accounts"></a>Tugas 2: Membuat dan mengonfigurasi akun Azure Storage

Dalam tugas ini, Anda akan membuat dan mengonfigurasi akun Azure Storage.

1. Di portal Azure, cari dan pilih **Akun penyimpanan**, lalu klik **+ Buat**.

1. Pada tab **Dasar** pada bilah **Buat akun penyimpanan**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | nama grup sumber daya **baru** **az104-07-rg1** |
    | Nama akun penyimpanan | nama unik global apa pun yang panjangnya antara 3 dan 24 yang terdiri atas huruf dan angka |
    | Wilayah | nama wilayah Azure tempat Anda dapat membuat akun Azure Storage  |
    | Performa | **Standard** |
    | Redundansi | **Penyimpanan Geo-redundant (GRS)** |

1. Klik **Berikutnya: Lanjutan >** , pada tab **Lanjutan** bilah **Buat akun penyimpanan**, tinjau opsi yang tersedia, terima defaultnya, dan klik **Berikutnya: Jaringan >** .

1. Pada tab **Jaringan** pada panel **Buat akun penyimpanan**, tinjau opsi yang tersedia, terima opsi default **Aktifkan akses publik dari semua jaringan** dan klik **Berikutnya: Perlindungan data >** .

1. Pada tab **Perlindungan data** dari bilah **Buat akun penyimpanan**, tinjau opsi yang tersedia, terima defaultnya, klik **Tinjauan + Buat**, tunggu proses validasi untuk selesai dan klik **Buat**.

    >**Catatan**: Tunggu hingga akun Penyimpanan dibuat. Proses ini memerlukan waktu sekitar 2 menit.

1. Pada bilah penerapan, klik **Buka sumber daya** untuk menampilkan bilah akun Azure Storage.

1. Pada bilah akun Penyimpanan, di bagian **Pengelolaan data**, klik **Replikasi geografis** dan catat lokasi sekundernya. 

1. Pada panel akun Penyimpanan, di bagian **Pengaturan**, pilih **Konfigurasi**, di daftar menurun **Replikasi** pilih **Penyimpanan redundan secara lokal (LRS)** dan simpan perubahannya.

1. Beralih kembali ke bilah **replika geografis** dan perhatikan bahwa, pada titik ini, akun Penyimpanan hanya memiliki lokasi utama.

1. Tampilkan kembali bilah **Konfigurasi** akun Penyimpanan, atur **Tingkat akses blob (default)** ke **dingin**, dan simpan perubahannya.

    > **Catatan**: Tingkat akses dingin optimal untuk data yang tidak sering diakses.

#### <a name="task-3-manage-blob-storage"></a>Tugas 3: Mengelola penyimpanan blob

Dalam tugas ini, Anda akan membuat kontainer blob dan mengunggah blob ke dalamnya.

1. Pada bilah akun Penyimpanan, di bagian **Penyimpanan data**, klik **Kontainer**.

1. Klik **+ Penampung** dan buat penampung dengan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **az104-07-container**  |
    | Tingkat akses publik | **Privat (tidak ada akses anonim)** |

1. Dalam daftar kontainer, klik **az104-07-container**, lalu klik **Unggah**.

1. Telusuri **\\Allfiles\\Labs\\07\\LISENSI** di komputer lab Anda dan klik **Buka**.

1. Pada bilah **Unggah blob**, luaskan bagian **Lanjutan** dan tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Jenis autentikasi | **Kunci akun**  |
    | Jenis blob | **Blob blok** |
    | Ukuran blok | **4 MB** |
    | Tingkat penyimpanan | **Populer** |
    | Unggah ke folder | **lisensi** |

    > **Catatan**: Tingkat akses dapat diatur untuk blob individu.

1. Klik **Unggah**.

    > **Catatan**: Perhatikan bahwa unggahan secara otomatis membuat subfolder bernama **lisensi**.

1. Kembali ke bilah **az104-07-container**, klik **lisensi**, lalu klik **LICENSE**.

1. Pada bilah **lisensi/LICENSE**, tinjau opsi yang tersedia.

    > **Catatan**: Anda memiliki opsi untuk mengunduh blob, mengubah tingkat aksesnya (saat ini diatur ke **Panas**), memperoleh sewa, yang akan mengubah status sewanya menjadi **Terkunci** (saat ini atur ke **Tidak Terkunci**) dan lindungi blob agar tidak dimodifikasi atau dihapus, serta tetapkan metadata khusus (dengan menentukan pasangan kunci dan nilai arbitrer). Anda juga memiliki kemampuan untuk **Mengedit** file secara langsung di antarmuka portal Azure, tanpa mengunduhnya terlebih dahulu. Anda juga dapat membuat snapshot, serta menghasilkan token SAS (Anda akan menjelajahi opsi ini di tugas berikutnya).

#### <a name="task-4-manage-authentication-and-authorization-for-azure-storage"></a>Tugas 4: Mengelola autentikasi dan otorisasi Azure Storage

Dalam tugas ini, Anda akan mengonfigurasi autentikasi dan otorisasi Azure Storage.

1. Pada bilah **lisensi/LICENSE**, pada tab **Ringkasan**, klik tombol **Salin ke clipboard** di samping entri **URL**.

1. Buka jendela browser lain dengan menggunakan mode InPrivate dan arahkan ke URL yang Anda salin di langkah sebelumnya.

1. Anda akan diberikan pesan berformat XML yang menyatakan **ResourceNotFound** atau **PublicAccessNotPermitted**.

    > **Catatan**: Ini diharapkan, karena kontainer yang Anda buat memiliki tingkat akses publik yang diatur ke **Privat (tanpa akses anonim)** .

1. Tutup jendela browser mode InPrivate, kembali ke jendela browser yang menampilkan bilah **lisensi/LICENSE** dari wadah Azure Storage, dan alihkan ke tab **Buat SAS**.

1. Pada tab **Buat SAS** pada bilah **lisensi/LICENSE**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Kunci penandatanganan | **Kunci 1** |
    | Izin | **Baca** |
    | Tanggal mulai | tanggal kemarin |
    | Waktu mulai | waktu saat ini |
    | Tanggal kedaluwarsa | tanggal besok |
    | Waktu kedaluwarsa | waktu saat ini |
    | Alamat IP yang diizinkan | biarkan kosong |
    

1. Klik **Hasilkan token SAS dan URL**.

1. Klik tombol **Salin ke clipboard** di sebelah entri **Blob SAS URL**.

1. Buka jendela browser lain dengan menggunakan mode InPrivate dan arahkan ke URL yang Anda salin di langkah sebelumnya.

    > **Catatan**: Jika Anda menggunakan Microsoft Edge, Anda akan dihadapkan pada halaman **Lisensi MIT (MIT)** . Jika Anda menggunakan Chrome, Microsoft Edge (Chromium) atau Firefox, Anda seharusnya dapat melihat konten file dengan mengunduhnya dan membukanya dengan Notepad.

    > **Catatan**: Ini diharapkan, karena sekarang akses Anda diotorisasi berdasarkan token SAS yang baru dibuat.

    > **Catatan**: Simpan URL blob SAS. Anda akan membutuhkannya nanti di lab ini.

1. Tutup jendela browser mode InPrivate, kembali ke jendela browser yang menampilkan bilah **lisensi/LICENSE** dari kontainer Azure Storage, dan dari sana, navigasikan kembali ke bilah **az104-07-container**.

1. Klik tautan **Beralih ke Akun Pengguna Azure AD** di samping label **Metode autentikasi**.

    > **Catatan**: Anda dapat melihat kesalahan saat Anda mengubah metode autentikasi (kesalahannya adalah *"Anda tidak memiliki izin untuk mencantumkan data menggunakan akun pengguna Anda dengan Azure AD"* ). Hal ini diharapkan.  

    > **Catatan**: Pada titik ini, Anda tidak memiliki izin untuk mengubah metode Autentikasi.

1. Pada bilah **az104-07-container**, klik **Access Control (IAM)** .

1. Pada tab **Periksa akses**, klik **Tambahkan penetapan peran**.

1. Pada bilah **Tambahkan penetapan peran**, tentukan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Peran | **Pemilik Data Blob Penyimpanan** |
    | Tetapkan akses ke | **Pengguna, grup, atau perwakilan layanan** |
    | Anggota | nama akun pengguna Anda |

1. Klik **Tinjauan + Tetapkan** lalu **Tinjauan + tetapkan**, dan kembali ke bilah **Ringkasan** dari wadah **az104-07-container** dan verifikasi bahwa Anda dapat mengubah metode Autentikasi ke (Beralih ke Akun Pengguna Azure AD).

    > **Catatan**: Mungkin perlu waktu sekitar 5 menit agar perubahan diterapkan.

#### <a name="task-5-create-and-configure-an-azure-files-shares"></a>Tugas 5: Membuat dan mengonfigurasi berbagi Azure Files

Dalam tugas ini, Anda akan membuat dan mengonfigurasi pembagian Azure Files.

> **Catatan**: Sebelum Anda memulai tugas ini, pastikan komputer virtual yang Anda sediakan di tugas pertama lab ini berjalan.

1. Di portal Azure, navigasikan kembali ke bilah akun penyimpanan yang Anda buat di tugas pertama lab ini dan, di bagian **Penyimpanan data**, klik **Berbagi file**.

1. Klik **+ Berbagi file** dan buat berbagi file dengan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **az104-07-bagikan** |

1. Klik berbagi file yang baru dibuat dan klik **Hubungkan**.

1. Pada panel **Hubungkan**, pastikan tab **Windows** dipilih. Di bawah ini Anda akan menemukan kotak teks abu-abu dengan skrip, di sudut kanan bawah kotak itu arahkan kursor ke ikon laman dan klik **Salin ke clipboard**.

1. Di portal Azure, cari dan pilih **Mesin virtual**, dan, dalam daftar komputer virtual, klik **az104-07-vm0**.

1. Pada bilah **az104-07-vm0**, di bagian **Operasi**, klik **Jalankan perintah**.

1. Pada bilah **az104-07-vm0 - Jalankan perintah**, klik **RunPowerShellScript**.

1. Pada bilah **Jalankan Skrip Perintah**, tempel skrip yang Anda salin sebelumnya dalam tugas ini ke panel **PowerShell Script** dan klik **Jalankan**.

1. Verifikasi bahwa skrip berhasil diselesaikan.

1. Ganti konten panel **PowerShell Script** dengan skrip berikut dan klik **Jalankan**:

   ```powershell
   New-Item -Type Directory -Path 'Z:\az104-07-folder'

   New-Item -Type File -Path 'Z:\az104-07-folder\az-104-07-file.txt'
   ```

1. Verifikasi bahwa skrip berhasil diselesaikan.

1. Navigasikan kembali ke bilah berbagi file **az104-07-berbagi**, klik **Refresh**, dan verifikasi bahwa **az104-07-folder** muncul dalam daftar folder.

1. Klik **az104-07-folder** dan verifikasi bahwa **az104-07-file.txt** muncul dalam daftar file.

#### <a name="task-6-manage-network-access-for-azure-storage"></a>Tugas 6: Mengelola akses jaringan Azure Storage

Dalam tugas ini, Anda akan mengonfigurasi akses jaringan Azure Storage.

1. Di portal Microsoft Azure, navigasikan kembali ke bilah akun penyimpanan yang Anda buat di tugas pertama lab ini dan, di bagian **Keamanan + Jaringan**, klik **Jaringan** lalu klik **Firewall dan jaringan virtual**.

1. Klik opsi **Diaktifkan dari jaringan virtual dan alamat IP yang dipilih** dan tinjau pengaturan konfigurasi yang tersedia setelah opsi ini diaktifkan.

    > **Catatan**: Anda dapat menggunakan pengaturan ini untuk mengonfigurasi konektivitas langsung antara komputer virtual Azure pada subnet jaringan virtual yang ditentukan dan akun penyimpanan dengan menggunakan titik akhir layanan.

1. Klik kotak centang **Tambahkan alamat IP klien Anda** dan simpan perubahannya.

1. Buka jendela browser lain dengan menggunakan mode InPrivate dan navigasikan ke URL blob SAS yang Anda buat di tugas sebelumnya.

    > **Catatan**: Jika Anda tidak merekam URL SAS dari tugas 4, Anda harus membuat yang baru dengan konfigurasi yang sama. Gunakan Tugas 4 langkah 4-6 sebagai panduan untuk membuat URL blob SAS baru. 

1. Anda akan disajikan dengan konten laman **Lisensi MIT (MIT)** .

    > **Catatan**: Ini diharapkan, karena Anda terhubung dari alamat IP klien Anda.

1. Tutup jendela browser mode InPrivate, kembali ke jendela browser yang menampilkan bilah **Jaringan** akun Azure Storage.

1. Di portal Microsoft Azure, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Microsoft Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**.

1. Dari panel Cloud Shell, jalankan perintah berikut untuk mencoba mengunduh blob LICENSE dari penampung **az104-07-kontainer** akun penyimpanan (ganti `[blob SAS URL]` tempat penampung dengan URL blob SAS yang Anda buat di tugas sebelumnya):

   ```powershell
   Invoke-WebRequest -URI '[blob SAS URL]'
   ```
1. Verifikasi bahwa upaya pengunduhan gagal.

    > **Catatan**: Anda akan menerima pesan yang menyatakan **AuthorizationFailure: Permintaan ini tidak diizinkan untuk melakukan operasi ini**. Hal ini diharapkan, karena Anda terhubung dari alamat IP yang ditetapkan ke Azure VM yang menghosting instans Cloud Shell.

1. Tutup panel Cloud Shell.

#### <a name="clean-up-resources"></a>Membersihkan sumber daya

>**Catatan**: Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak digunakan akan memastikan bahwa Anda tidak akan melihat biaya yang tidak diharapkan.

>**Catatan**:  Jangan khawatir jika sumber daya lab tidak dapat segera dihapus. Terkadang sumber daya memiliki ketergantungan dan membutuhkan waktu lama untuk dihapus. Memantau penggunaan sumber daya adalah tugas Administrator yang umum, jadi tinjau sumber daya Anda secara berkala di Portal untuk melihat bagaimana pembersihannya. Anda juga dapat mencoba menghapus Grup Sumber Daya tempat sumber daya berada. Itu adalah pintasan Administrator cepat. Jika Anda memiliki kekhawatiran berbicara dengan instruktur Anda.

1. Di portal Microsoft Azure, buka sesi **PowerShell** dalam panel **Cloud Shell**.

1. Buat daftar semua grup sumber daya yang dibuat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-07*'
   ```

1. Hapus semua grup sumber daya yang Anda buat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-07*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Catatan**: Perintah dijalankan secara asinkron (sebagaimana yang ditentukan oleh parameter -AsJob), jadi saat Anda akan dapat menjalankan perintah PowerShell lain langsung setelahnya dalam sesi PowerShell yang sama, proses ini akan memakan waktu beberapa menit sebelum grup sumber daya benar-benar dihapus.

#### <a name="review"></a>Tinjauan

Di lab ini, Anda telah:

- Memprovisikan lingkungan lab
- Membuat dan mengonfigurasi akun Azure Storage
- Penyimpanan blob terkelola
- Autentikasi dan otorisasi terkelola untuk Azure Storage
- Membuat dan mengonfigurasi berbagi file Azure
- Akses jaringan terkelola untuk Azure Storage
