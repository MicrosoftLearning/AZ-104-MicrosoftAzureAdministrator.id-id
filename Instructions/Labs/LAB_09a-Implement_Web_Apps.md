---
lab:
  title: 'Lab 09a: Menerapkan Web Apps'
  module: Administer PaaS Compute Options
---

# Lab 09a - Menerapkan Aplikasi Web
# Panduan lab siswa

## Skenario lab

Anda perlu mengevaluasi penggunaan aplikasi Web Azure untuk menghosting situs web Contoso, yang saat ini dihosting di pusat data lokal perusahaan. Situs web berjalan di server Windows menggunakan tumpukan runtime PHP. Anda juga perlu menentukan bagaimana Anda dapat menerapkan praktik DevOps dengan memanfaatkan slot penyebaran aplikasi web Azure.

**Catatan:** Tersedia **[simulasi lab interaktif](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2013)** yang memungkinkan Anda mengklik lab ini sesuai keinginan Anda. Anda mungkin menemukan sedikit perbedaan antara simulasi interaktif dan lab yang dihosting, tetapi konsep dan ide utama yang ditunjukkan sama. 

## Tujuan

Di lab ini Anda akan:

+ Tugas 1: Membuat aplikasi web Azure
+ Tugas 2: Membuat slot penyebaran pentahapan
+ Tugas 3: Konfigurasikan pengaturan penyebaran aplikasi web
+ Tugas 4: Sebarkan kode ke slot pentahapan pementasan
+ Tugas 5: Menukar slot pentahapan
+ Tugas 6: Mengonfigurasi dan menguji penskalaan otomatis aplikasi web Azure

## Perkiraan waktu: 30 menit

## Diagram arsitektur

![gambar](../media/lab09a.png)

### Petunjuk

## Latihan 1

## Tugas 1: Membuat aplikasi web Azure

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
    | Tumpukan runtime | **PHP 8.2** |
    | Sistem operasi | **Linux** |
    | Wilayah | nama wilayah Azure tempat Anda dapat memprovisikan aplikasi web Azure |
    | Paket harga | menerima konfigurasi default |

1. Klik **Tinjauan + buat**. Pada tab **Tinjauan + buat** panel **Buat Aplikasi Web**, pastikan validasi lulus dan klik **Buat**.

    >**Catatan**: Tunggu hingga aplikasi web dibuat sebelum Anda melanjutkan ke tugas berikutnya. Ini akan memakan waktu sekitar satu menit.

1. Pada panel penyebaran, klik **Buka sumber daya**.

## Tugas 2: Membuat slot penyebaran pentahapan

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

## Tugas 3: Konfigurasikan pengaturan penyebaran aplikasi web

Dalam tugas ini, Anda akan mengonfigurasi pengaturan penyebaran aplikasi web.

1. Pada panel slot penyebaran pentahapan, di bagian **Penyebaran**, klik **Pusat Penyebaran** lalu pilih tab **Pengaturan**.

    >**Catatan:** Pastikan Anda berada di panel slot pentahapan (bukan slot produksi).
    
1. Pada tab **Pengaturan**, di daftar menurun **Sumber**, pilih **Git Lokal** dan klik tombol **Simpan**

1. Pada bilah **Pusat Penyebaran** , salin entri **Uri Klon Git** ke Notepad.

    >**Catatan:** Anda akan memerlukan nilai Uri Klon Git dalam tugas lab ini berikutnya.

1. Pada panel **Pusat Penyebaran**, pilih tab **Informasi masuk Git/FTPS Lokal**, di bagian **Cakupan Pengguna**, tentukan pengaturan berikut, dan klik **Simpan**.

    | Pengaturan | Nilai |
    | --- | ---|
    | Nama pengguna | nama unik global apa pun (lihat catatan)  |
    | Kata sandi | kata sandi apa pun yang memenuhi persyaratan kompleksitas (lihat catatan) |

    >**Catatan:** Salin kredensial ini ke Notepad. Anda akan membutuhkannya nanti.
    
    >**Catatan:** Kredensial ini akan diteruskan melalui URI. Jangan sertakan pesuruh khusus yang memengaruhi interpretasi URI. Misalnya, @, $, atau #. Tanda asterick atau plus (di tengah string) akan berfungsi.
    
## Tugas 4: Sebarkan kode ke slot pentahapan pementasan

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

1. Dari panel Cloud Shell, jalankan yang berikut ini untuk menambahkan git jarak jauh (pastikan untuk mengganti `[deployment_user_name]` tempat penampung dan `[git_clone_uri]` dengan nilai nama pengguna **Kredensial Penyebaran** dan **Git Clone Uri**, masing-masing, yang Anda identifikasi di tugas sebelumnya):

   ```powershell
   git remote add [deployment_user_name] [git_clone_uri]
   ```

    >**Catatan**: Nilai berikut `git remote add` ini tidak harus cocok dengan nama pengguna **Informasi Masuk Penyebaran**, tetapi harus unik

1. Dari panel Cloud Shell, jalankan yang berikut ini untuk mendorong contoh kode aplikasi web dari repositori lokal ke slot penyebaran penahapan aplikasi web Azure (pastikan untuk mengganti nilai tempat penampung dengan nilai nama pengguna dan kata sandi **Kredensial Penyebaran** dan nama aplikasi, yang Anda identifikasi di tugas sebelumnya):

   ```powershell
    git push https://<deployment-username>:<deployment-password>@<app-name>.scm.azurewebsites.net/<app-name>.git master
   ```

1. Tutup panel Cloud Shell.

1. Pada panel slot pentahapan, klik **Ringkasan** lalu klik tautan **URL** untuk menampilkan laman web bawaan di tab browser baru.

1. Verifikasi bahwa halaman browser menampilkan **Halo Dunia** pesan dan tutup tab baru.

## Tugas 5: Menukar slot pentahapan

Dalam tugas ini, Anda akan menukar slot pentahapan dengan slot produksi

1. Navigasi kembali ke panel yang menampilkan slot produksi aplikasi web.

1. Di bagian **Penyebaran**, klik **Slot penyebaran**, lalu klik ikon toolbar **Tukar**.

1. Pada panel **Tukar**, tinjau pengaturan default dan klik **Tukar**.

1. Klik **Gambaran Umum** pada panel slot produksi aplikasi web lalu klik tautan **URL** untuk menampilkan halaman beranda situs web di tab browser baru.

1. Pastikan halaman web default telah diganti dengan **Halo Dunia!** halaman.

## Tugas 6: Mengonfigurasi dan menguji penskalaan otomatis aplikasi web Azure

Dalam tugas ini, Anda akan mengonfigurasi dan menguji penskalaan otomatis aplikasi web Azure.

1. Pada panel yang menampilkan slot produksi aplikasi web, di bagian **Pengaturan**, klik **Peluasan skala (paket App Service)** .

1. Dari **bagian Penskalakan** pilih opsi **Berbasis Aturan** , lalu klik tautan **Kelola penskalakan berbasis aturan** .

1. Klik **Skala otomatis kustom**.

    >**Catatan**: Anda juga memiliki opsi untuk menskalakan aplikasi web secara manual.

1. Pilih **Skala berdasarkan metrik** dan klik **+ Tambahkan aturan**

1. Pada panel **Aturan skala**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- |--- |
    | Sumber metrik | **Sumber daya saat ini** |
    | Namespace layanan metrik | **metrik standar** |
    | Nama metrik | **Persentase CPU** |
    | Operator | **Lebih besar dari** |
    | Ambang metrik untuk memicu tindakan skala | **10** |
    | Durasi (dalam menit) | **1** |
    | Statistik butir waktu | **Maksimum** |
    | Agregasi waktu | **Maksimum** |
    | Operasi | **Tingkatkan jumlah sebesar** |
    | Jumlah instans | **1** |
    | Pendinginan (menit) | **5** |

    >**Catatan**: Nilai ini tidak menunjukkan konfigurasi yang realistis, karena tujuannya adalah untuk memicu penskalaan otomatis sesegera mungkin, tanpa masa tunggu yang diperpanjang.

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

1. Perkecil panel Cloud Shell (namun jangan ditutup) dan, di bilah aplikasi web, di bagian Pengaturan, klik **Skalakan (Paket App Service)** .

1. Pilih **Pengaturan Skala Otomatis**, pilih tab **Jalankan riwayat** , dan periksa **jumlah instans sumber daya yang diamati**.

1. Pantau pemanfaatan dan jumlah instans selama beberapa menit. 

    >**Catatan**: Anda mungkin perlu melakukan **Refresh** halamannya.

1. Setelah Anda melihat bahwa jumlah instans telah meningkat menjadi 2, buka kembali panel Cloud Shell dan hentikan skrip dengan menekan **Ctrl+C**.

1. Tutup panel Cloud Shell.

## Membersihkan sumber daya

>**Catatan**: Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak digunakan akan memastikan bahwa Anda tidak akan melihat biaya yang tidak diharapkan.

>**Catatan**:  Jangan khawatir jika sumber daya lab tidak dapat segera dihapus. Terkadang sumber daya memiliki ketergantungan dan membutuhkan waktu lama untuk dihapus. Ini adalah tugas Administrator yang umum untuk memantau penggunaan sumber daya, jadi tinjauan sumber daya Anda secara berkala di Portal untuk melihat bagaimana pembersihannya. 

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

## Tinjau

Di lab ini, Anda telah:

+ Membuat aplikasi web Azure
+ Menukar slot pentahapan
+ Mengonfigurasikan pengaturan penyebaran aplikasi web
+ Menyebarkan kode ke slot penyebaran pentahapan
+ Tukar slot pentahapan
+ Mengonfigurasi dan menguji penskalaan otomatis aplikasi web Azure
