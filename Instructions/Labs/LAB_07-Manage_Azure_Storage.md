---
lab:
  title: 'Lab 07: Mengelola penyimpanan Azure'
  module: Administer Azure Storage
---

# Lab 07 - Mengelola Azure Storage
# Panduan lab siswa

## Skenario laboratorium

Anda perlu mengevaluasi penggunaan penyimpanan Azure untuk menyimpan file yang saat ini berada di penyimpanan data lokal. Meskipun sebagian besar file ini tidak sering diakses, ada beberapa pengecualian. Anda ingin meminimalkan biaya penyimpanan dengan menempatkan file yang jarang diakses di tingkat penyimpanan dengan harga lebih rendah. Selain itu, rencanakan untuk menjelajahi berbagai mekanisme perlindungan yang ditawarkan Azure Storage, termasuk akses jaringan, autentikasi, otorisasi, dan replikasi. Terakhir, Anda ingin menentukan sejauh mana layanan Azure Files mungkin cocok untuk menghosting berbagi file lokal Anda.

**Catatan:** Tersedia **[simulasi lab interaktif](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2011)** yang memungkinkan Anda mengklik lab ini sesuai keinginan Anda. Anda mungkin menemukan sedikit perbedaan antara simulasi interaktif dan lab yang dihosting, tetapi konsep dan ide utama yang ditunjukkan sama. 

## Tujuan

Di lab ini Anda akan:

+ Tugas 1: Memprovisikan lingkungan lab
+ Tugas 2: Membuat dan mengonfigurasi akun Azure Storage
+ Tugas 3: Mengelola penyimpanan blob
+ Tugas 4: Mengelola autentikasi dan otorisasi untuk Azure Storage
+ Tugas 5: Membuat dan mengonfigurasi berbagi Azure Files
+ Tugas 6: Mengelola akses jaringan untuk Azure Storage

## Perkiraan waktu: 40 menit

## Diagram arsitektur

![gambar](../media/lab07.png)


### Petunjuk

## Latihan 1

## Tugas 1: Memprovisikan lingkungan lab

Dalam tugas ini, Anda akan menerapkan komputer virtual Azure yang akan Anda gunakan nanti di lab ini.

1. Masuk ke **[portal Azure](https://portal.azure.com)**.

1. Di portal Microsoft Azure, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**.

    >**Catatan**: Jika ini adalah pertama kalinya Anda memulai **Cloud Shell** dan Anda disajikan dengan **pesan Anda tidak memiliki penyimpanan yang dipasang** , pilih langganan yang Anda gunakan di lab ini, dan klik **Buat penyimpanan**.

1. Di bilah alat panel Cloud Shell, klik ikon **Unggah/Unduh file**, di menu menurun, klik **Unggah** dan unggah file **\\Allfiles\\Labs\\07\\az104-07-vm-template.json** dan **\\Allfiles\\Labs\\07\\az104-07-vm-parameters.json** ke direktori beranda Cloud Shell.

1. Dari panel Cloud Shell, jalankan elemen berikut ini untuk membuat grup sumber daya yang akan menghosting komputer virtual (ganti tempat penampung '[Azure_region]' dengan nama wilayah Azure tempat Anda ingin menerapkan komputer virtual Azure)

    >**Catatan**: Untuk mencantumkan nama wilayah Azure, jalankan `(Get-AzLocation).Location`
    >**Catatan**: Setiap perintah di bawah ini harus di ketikkan secara terpisah

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

    >**Catatan**: Anda akan diminta untuk memberikan kata sandi Admin.

   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-07-vm-template.json `
      -TemplateParameterFile $HOME/az104-07-vm-parameters.json `
      -AsJob
   ```

    >**Catatan**: Jangan tunggu penyebaran selesai, tetapi lanjutkan ke tugas berikutnya.

    >**Catatan**: Jika Anda mendapatkan kesalahan yang menyatakan ukuran VM tidak tersedia, silakan minta bantuan instruktur Anda dan coba langkah-langkah ini.
    > 1. Klik tombol `{}` di CloudShell Anda, pilih **az104-07-vm-parameters.json** dari bilah sisi kiri dan catat nilai parameter `vmSize`.
    > 1. Periksa lokasi di mana grup sumber daya 'az104-04-rg1' diterapkan. Anda dapat menjalankan `az group show -n az104-04-rg1 --query location` di CloudShell Anda untuk mendapatkannya.
    > 1. Jalankan `az vm list-skus --location <Replace with your location> -o table --query "[? contains(name,'Standard_D2s')].name"` di CloudShell Anda.
    > 1. Ganti nilai parameter `vmSize` dengan salah satu nilai yang dikembalikan oleh perintah yang baru saja Anda jalankan.
    > 1. Sekarang sebarkan ulang template Anda dengan menjalankan perintah `New-AzResourceGroupDeployment` lagi. Anda dapat menekan tombol atas beberapa kali yang akan membawa ke perintah yang dieksekusi terakhir.

1. Tutup panel Cloud Shell.

## Tugas 2: Membuat dan mengonfigurasi akun Azure Storage

Dalam tugas ini, Anda akan membuat dan mengonfigurasi akun Azure Storage.

1. Di portal Azure, cari dan pilih **Akun penyimpanan**, lalu klik **+ Buat**.

1. Pada tab **Dasar** pada bilah **Buat akun penyimpanan**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | nama grup sumber daya **baru** **az104-07-rg1** |
    | Nama akun penyimpanan | nama unik global apa pun yang panjangnya antara 3 dan 24 yang terdiri atas huruf dan angka |
    | Wilayah | nama wilayah Azure tempat Anda dapat membuat akun Azure Storage  |
    | Performa | **Standard**
           |
    | Redundansi geografis | **Penyimpanan Geo-redundant (GRS)** |

1. Klik **Berikutnya: >** Tingkat Lanjut, pada tab **Tingkat Lanjut** dari bilah **Buat akun** penyimpanan, tinjau opsi yang tersedia, terima default, dan klik **Berikutnya: Jaringan >**.

1. Pada tab **Jaringan dari bilah **Buat akun** penyimpanan, tinjau opsi yang tersedia, terima opsi **default Aktifkan akses publik dari semua jaringan** dan klik **Berikutnya: Perlindungan data >****.

1. Pada tab **Perlindungan data** dari bilah **Buat akun penyimpanan**, tinjau opsi yang tersedia, terima defaultnya, klik **Tinjau + Buat**, tunggu proses validasi untuk selesai dan klik **Buat**.

    >**Catatan**: Tunggu hingga akun Penyimpanan dibuat. Proses ini memerlukan waktu sekitar 2 menit.

1. Pada bilah penerapan, klik **Buka sumber daya** untuk menampilkan bilah akun Azure Storage.

1. Pada bilah akun Penyimpanan, di bagian **Manajemen data**, klik **Redundansi** dan catat lokasi sekunder. 

1. Di daftar menu dropdown **Redundansi** pilih **Penyimpanan yang berlebihan secara lokal (LRS)** dan simpan perubahannya. Perhatikan bahwa, saat ini, akun Penyimpanan hanya memiliki lokasi primer.

1. Pada bilah akun Penyimpanan, di bagian **Pengaturan**, pilih **Konfigurasi**. Tetapkan **tingkat akses Blob (default)** ke **Cool**, dan simpan perubahannya.

    > **Catatan**: Tingkat akses dingin optimal untuk data yang tidak sering diakses.

## Tugas 3: Mengelola penyimpanan blob

Dalam tugas ini, Anda akan membuat kontainer blob dan mengunggah blob ke dalamnya.

1. Pada bilah akun Penyimpanan, di bagian **Penyimpanan data**, klik **Kontainer**.

1. Klik **+ Penampung** dan buat penampung dengan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **az104-07-container**  |
    | Tingkat akses publik | **Pribadi (tidak ada akses anonim**) |

1. Dalam daftar kontainer, klik **az104-07-container**, lalu klik **Unggah**.

1. Telusuri **\\Allfiles\\Labs\\07\\LISENSI** di komputer lab Anda dan klik **Buka**.

1. Pada bilah **Unggah blob**, luaskan bagian **Lanjutan** dan tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Jenis blob | **Blob blok** |
    | Ukuran blok | **4 MB** |
    | Tingkat penyimpanan | **Populer** |
    | Unggah ke folder | **lisensi** |

    > **Catatan**: Tingkat akses dapat diatur untuk blob individual.

1. Klik **Unggah**.

    > **Catatan**: Perhatikan bahwa unggahan secara otomatis membuat subfolder bernama **lisensi**.

1. Kembali ke bilah **az104-07-container**, klik **lisensi**, lalu klik **LICENSE**.

1. Pada bilah **lisensi/LICENSE**, tinjau opsi yang tersedia.

    > **Catatan**: Anda memiliki opsi untuk mengunduh blob, mengubah tingkat aksesnya (saat ini diatur ke **Panas**), memperoleh sewa, yang akan mengubah status sewanya menjadi **Terkunci** (saat ini diatur ke **Tidak Terkunci**) dan melindungi blob agar tidak dimodifikasi atau dihapus, serta menetapkan metadata kustom (dengan menentukan pasangan kunci dan nilai arbitrer). Anda juga memiliki kemampuan untuk **Mengedit** file secara langsung di antarmuka portal Azure, tanpa mengunduhnya terlebih dahulu. Anda juga dapat membuat snapshot, serta menghasilkan token SAS (Anda akan menjelajahi opsi ini di tugas berikutnya).

## Tugas 4: Mengelola autentikasi dan otorisasi untuk Azure Storage

Dalam tugas ini, Anda akan mengonfigurasi autentikasi dan otorisasi Azure Storage.

1. Pada bilah **lisensi/LICENSE**, pada tab **Ringkasan**, klik tombol **Salin ke papan klip** di samping entri **URL**.

1. Buka jendela browser lain dengan menggunakan mode InPrivate dan arahkan ke URL yang Anda salin di langkah sebelumnya.

1. Anda akan diberikan pesan berformat XML yang menyatakan **ResourceNotFound** atau **PublicAccessNotPermitted**.

    > **Catatan**: Ini diharapkan, karena kontainer yang Anda buat memiliki tingkat akses publik yang diatur ke **Privat (tidak ada akses anonim)**.

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

1. Klik tombol **Salin ke papan klip** di sebelah entri **Blob SAS URL**.

1. Buka jendela browser lain dengan menggunakan mode InPrivate dan arahkan ke URL yang Anda salin di langkah sebelumnya.

    > **Catatan**: Anda harus dapat melihat konten file dengan mengunduhnya dan membukanya dengan Notepad.

    > **Catatan**: Ini diharapkan, karena sekarang akses Anda diotorisasi berdasarkan token SAS yang baru dibuat.

    > **Catatan**: Simpan URL SAS blob. Anda akan membutuhkannya nanti di lab ini.

1. Tutup jendela browser mode InPrivate, kembali ke jendela browser yang menampilkan bilah **lisensi/LICENSE** dari kontainer Azure Storage, dan dari sana, navigasikan kembali ke bilah **az104-07-container**.

1. Klik tautan **Beralih ke Akun Pengguna Azure AD** di samping label **Metode autentikasi**.

    > **Catatan**: Anda dapat melihat kesalahan saat mengubah metode autentikasi (kesalahannya adalah *"Anda tidak memiliki izin untuk mencantumkan data menggunakan akun pengguna Anda dengan Microsoft Azure AD"*). Hal ini diharapkan.  

    > **Catatan**: Pada titik ini, Anda tidak memiliki izin untuk mengubah metode Autentikasi.

1. Pada bilah **az104-07-container**, klik **Access Control (IAM)**.

1. Pada tab **Periksa akses**, klik **Tambahkan penetapan peran**.

1. Pada bilah **Tambahkan penetapan peran**, tentukan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Peran | **Pemilik Data Blob Penyimpanan** |
    | Tetapkan akses ke | **Pengguna, grup, atau perwakilan layanan** |
    | Anggota | nama akun pengguna Anda |

1. Klik **Tinjau + Tetapkan** lalu **Tinjau + tetapkan**, dan kembali ke bilah **Ringkasan** dari wadah **az104-07-container** dan verifikasi bahwa Anda dapat mengubah metode Autentikasi ke (Beralih ke Akun Pengguna Azure AD).

    > **Catatan**: Mungkin perlu waktu sekitar 5 menit agar perubahan diterapkan.

## Tugas 5: Membuat dan mengonfigurasi berbagi Azure Files

Dalam tugas ini, Anda akan membuat dan mengonfigurasi pembagian Azure Files.

> **Catatan**: Sebelum Anda memulai tugas ini, verifikasi bahwa komputer virtual yang Anda provisikan dalam tugas pertama lab ini sedang berjalan.

1. Di portal Azure, navigasikan kembali ke bilah akun penyimpanan yang Anda buat di tugas pertama lab ini dan, di bagian **Penyimpanan data**, klik **Berbagi file**.

1. Klik **+ Berbagi file** dan pada tab **Dasar** memberi nama berbagi file, **az104-07-share**. Tinjau pengaturan lain pada tab ini. 

1. Pindah ke tab **Cadangan**, dan pastikan **Aktifkan Pencadangan** tidak** dicentang**.

1. Klik **Tinjau dan buat**, lalu **Buat**. Tunggu hingga berbagi file disebarkan. 

1. Klik berbagi file yang baru dibuat dan catat informasi yang tersedia di bilah **az104-07-share** .

1. Klik **Telusuri** dan perhatikan bahwa tidak ada file atau folder dalam berbagi file baru. Klik **Sambungkan**.

1. Pada panel **Hubungkan**, pastikan tab **Windows** dipilih. Di bawah ini Anda akan menemukan tombol dengan label **Tampilkan Skrip**. Klik pada tombol dan Anda akan menemukan kotak teks abu-abu dengan skrip, di sudut kanan bawah kotak tersebut arahkan kursor ke ikon halaman dan klik **Salin ke papan klip**.

1. Di portal Azure, cari dan pilih **Komputer virtual**, dan, dalam daftar komputer virtual, klik **az104-07-vm0**.

1. Pada bilah **az104-07-vm0**, di bagian **Operasi**, klik **Jalankan perintah**.

1. Pada bilah **az104-07-vm0 - Jalankan perintah**, klik **RunPowerShellScript**.

1. Pada bilah **Jalankan Skrip Perintah**, tempel skrip yang Anda salin sebelumnya dalam tugas ini ke panel **PowerShell Script** dan klik **Jalankan**.

1. Verifikasi bahwa skrip berhasil diselesaikan.

1. Ganti konten panel **Skrip PowerShell** dengan skrip berikut dan klik **Jalankan**:

   ```powershell
   New-Item -Type Directory -Path 'Z:\az104-07-folder'

   New-Item -Type File -Path 'Z:\az104-07-folder\az-104-07-file.txt'
   ```

1. Verifikasi bahwa skrip berhasil diselesaikan.

1. Navigasi kembali ke **bilah az104-07-share \| Telusuri** berbagi file, klik **Refresh**, dan verifikasi bahwa **folder** az104-07 muncul dalam daftar folder.

1. Klik **az104-07-folder** dan verifikasi bahwa **az104-07-file.txt** muncul dalam daftar file.

## Tugas 6: Mengelola akses jaringan untuk Azure Storage

Dalam tugas ini, Anda akan mengonfigurasi akses jaringan Azure Storage.

1. Di portal Microsoft Azure, navigasikan kembali ke bilah akun penyimpanan yang Anda buat di tugas pertama lab ini dan, di bagian **Keamanan + Jaringan**, klik **Jaringan** lalu klik **Firewall dan jaringan virtual**.

1. Klik opsi **Diaktifkan dari jaringan virtual dan alamat IP yang dipilih** dan tinjau pengaturan konfigurasi yang tersedia setelah opsi ini diaktifkan.

    > **Catatan**: Anda dapat menggunakan pengaturan ini untuk mengonfigurasi konektivitas langsung antara komputer virtual Azure pada subnet jaringan virtual yang ditunjuk dan akun penyimpanan dengan menggunakan titik akhir layanan.

1. Klik kotak centang **Tambahkan alamat IP klien Anda** dan simpan perubahannya.

1. Buka jendela browser lain dengan menggunakan mode InPrivate dan navigasikan ke URL blob SAS yang Anda buat di tugas sebelumnya.

    > **Catatan**: Jika Anda tidak merekam URL SAS dari tugas 4, Anda harus membuat url baru dengan konfigurasi yang sama. Gunakan Tugas 4 langkah 4-6 sebagai panduan untuk membuat URL blob SAS baru. 

1. Anda akan disajikan dengan konten laman **Lisensi MIT (MIT)**.

    > **Catatan**: Ini diharapkan, karena Anda terhubung dari alamat IP klien Anda.

1. Tutup jendela browser mode InPrivate, kembali ke jendela browser yang menampilkan bilah **Jaringan** akun Azure Storage.

1. Di portal Microsoft Azure, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**.

1. Dari panel Cloud Shell, jalankan perintah berikut untuk mencoba mengunduh blob LICENSE dari penampung **az104-07-kontainer** akun penyimpanan (ganti `[blob SAS URL]` tempat penampung dengan URL blob SAS yang Anda buat di tugas sebelumnya):

   ```powershell
   Invoke-WebRequest -URI '[blob SAS URL]'
   ```
1. Verifikasi bahwa upaya pengunduhan gagal.

    > **Catatan**: Anda harus menerima pesan yang menyatakan **AuthorizationFailure: Permintaan ini tidak berwenang untuk melakukan operasi** ini. Hal ini diharapkan, karena Anda terhubung dari alamat IP yang ditetapkan ke Azure VM yang menghosting instans Cloud Shell.

1. Tutup panel Cloud Shell.

## Membersihkan sumber daya

>**Catatan**: Ingatlah untuk menghapus sumber daya Azure yang baru dibuat yang tidak lagi Anda gunakan. Dengan menghapus sumber daya yang tidak digunakan, Anda tidak akan melihat biaya yang tak terduga.

>**Catatan**: Jangan khawatir jika sumber daya lab tidak dapat segera dihapus. Terkadang sumber daya memiliki ketergantungan dan membutuhkan waktu lama untuk dihapus. Ini adalah tugas Administrator yang umum untuk memantau penggunaan sumber daya, jadi tinjau sumber daya Anda secara berkala di Portal untuk melihat bagaimana pembersihannya. Anda juga dapat mencoba menghapus Grup Sumber Daya tempat sumber daya berada. Itu adalah pintasan Administrator cepat. Jika Anda memiliki kekhawatiran berbicara dengan instruktur Anda.

1. Di portal Azure, buka sesi **PowerShell** dalam panel **Cloud Shell**.

1. Buat daftar semua grup sumber daya yang dibuat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-07*'
   ```

1. Hapus semua grup sumber daya yang Anda buat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-07*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Catatan**: Perintah dijalankan secara asinkron (seperti yang ditentukan oleh parameter -AsJob), jadi sementara Anda akan dapat menjalankan perintah PowerShell lain segera setelah itu dalam sesi PowerShell yang sama, akan memakan waktu beberapa menit sebelum grup sumber daya benar-benar dihapus.

## Tinjau

Di lab ini, Anda telah:

- Memprovisikan lingkungan lab
- Membuat dan mengonfigurasi akun Azure Storage
- Penyimpanan blob terkelola
- Autentikasi dan otorisasi terkelola untuk Azure Storage
- Membuat dan mengonfigurasi berbagi file Azure
- Akses jaringan terkelola untuk Azure Storage
