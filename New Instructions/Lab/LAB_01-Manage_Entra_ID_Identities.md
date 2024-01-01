---
lab:
  title: 'Lab 01: Mengelola Identitas ID Microsoft Entra'
  module: Administer Identity
---

# Lab 01 - Mengelola Identitas ID Microsoft Entra

## Pengenalan lab

Ini adalah yang pertama dalam serangkaian lab untuk Administrator Azure. Di lab ini, Anda mempelajari tentang pengguna dan grup. Pengguna dan grup adalah blok penyusun dasar untuk solusi identitas. Anda juga terbiasa dengan alat administrator dasar. 

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur di lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkahnya ditampilkan di US Timur.

## Perkiraan waktu: 40 menit

## Skenario laboratorium

Organisasi Anda sedang membangun lingkungan lab baru untuk pengujian pra-produksi aplikasi dan layanan.  Beberapa teknisi sedang dipekerjakan untuk mengelola lingkungan lab, termasuk komputer virtual. Untuk mengizinkan insinyur mengautentikasi dengan menggunakan ID Microsoft Entra, Anda telah ditugaskan untuk menyediakan pengguna dan akun grup. Untuk meminimalkan overhead administratif, keanggotaan grup harus diperbarui secara otomatis berdasarkan jabatan. Anda juga perlu tahu cara menghapus pengguna untuk mencegah akses setelah teknisi meninggalkan organisasi Anda.

## Simulasi lab interaktif

Ada simulasi lab interaktif yang mungkin berguna bagi Anda untuk topik ini. Simulasi ini memungkinkan Anda mengklik skenario serupa dengan kecepatan Anda sendiri. Ada perbedaan antara simulasi interaktif dan lab ini, tetapi banyak konsep intinya sama. Langganan Azure tidak diperlukan. 

+ [Kelola Identitas](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%201) ID Entra*. Membuat dan mengonfigurasi pengguna dan menetapkan ke grup. Buat penyewa Azure dan kelola akun tamu. 

## Diagram arsitektur
![Diagram arsitektur lab 01.](../media/az104-lab01-user-and-groups2.png)

## Tugas

+ Tugas 1: Membuat grup sumber daya.
+ Tugas 2: Membuat dan mengonfigurasi akun pengguna.
+ Tugas 3: Buat grup dan tambahkan anggota.
+ Tugas 4: Biasakan diri Anda dengan Cloud Shell.
+ Tugas 5: Berlatih dengan Azure PowerShell.
+ Tugas 6: Berlatihlah dengan shell Bash.

## Tugas 1: Membuat grup sumber daya

Dalam tugas ini, Anda membuat grup sumber daya. Grup sumber daya adalah pengelompokan sumber daya terkait. Misalnya, semua sumber daya untuk proyek, departemen, atau aplikasi. 

>**Catatan:** Untuk setiap lab dalam kursus ini, Anda akan membuat grup sumber daya baru. Ini memungkinkan Anda dengan cepat menemukan dan mengelola sumber daya lab Anda. 

1. Masuk ke **portal Azure** - `https://portal.azure.com`.

    >**Catatan:** portal Azure digunakan di semua lab. Jika Anda baru menggunakan Azure, ketik `Quickstart Center` di kotak pencarian teratas. Kemudian beberapa menit untuk menonton **video Memulai di portal Azure**. Bahkan jika Anda telah menggunakan portal sebelumnya, Anda akan menemukan beberapa tips dan trik tentang menavigasi dan menyesuaikan antarface. 
   
1. Di portal Azure, cari dan pilih `Resource groups`.
   
1. Pada bilah **Grup** sumber daya, klik **+ Buat**, dan berikan informasi yang diperlukan. 

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama langganan | langganan Anda |
    | Nama grup sumber daya | `az104-rg1` |
    | Lokasi | **US Timur** |

    >**Catatan:** Semua lab menggunakan **US** Timur. **Tonton video Pilih wilayah** terbaik di Pusat** Mulai Cepat untuk mempelajari apa yang harus dipertimbangkan **saat memilih wilayah.  
    
1. Klik **Tinjau + buat** lalu klik **Buat**.

    >**Catatan**: Tunggu hingga grup sumber daya disebarkan. **Gunakan ikon Pemberitahuan** (kanan atas) untuk melacak kemajuan penyebaran.

1. Pilih **Buka sumber daya**, refresh halaman dan verifikasi grup sumber daya baru Anda muncul dalam daftar grup sumber daya.

    ![Cuplikan layar daftar grup sumber daya.](../media/az104-lab01-create-resource-group.png)

## Tugas 2: Membuat dan mengonfigurasi akun pengguna

Dalam tugas ini, Anda akan membuat dan mengonfigurasi akun pengguna. Akun pengguna akan menyimpan data pengguna seperti nama, departemen, lokasi, dan informasi kontak.

1. Lanjutkan di portal Azure. 

1. Cari dan pilih `Microsoft Entra ID`.

1. Pada bilah ID Microsoft Entra, gulir ke bawah ke bagian **Kelola** , klik **Pengaturan** pengguna, dan tinjau opsi konfigurasi yang tersedia.

1. Navigasi kembali ke panel **Pengguna - Semua pengguna**, lalu klik **+ Pengguna baru**.

1. Buat pengguna baru dengan pengaturan berikut (biarkan pengguna lain dengan default mereka). Perhatikan semua jenis data yang dapat disertakan dalam akun pengguna. 

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama principal pengguna | `az104-user1` |
    | Nama tampilan | `az104-user1` |
    | Buat Kata Sandi secara otomatis | batal pilih |
    | Kata sandi awal | **Berikan kata sandi yang aman** |
    | Jabatan pekerjaan (tab Properti) | `Cloud Administrator` |
    | Departemen (tab Properti) | `IT` |
    | Lokasi penggunaan (tab Properti) | **Amerika Serikat** |

### Tugas 4: Membuat grup dan menambahkan anggota

Dalam tugas ini, Anda membuat grup. Grup digunakan untuk akun pengguna atau perangkat. Beberapa grup memiliki anggota yang ditetapkan secara statis. Beberapa grup memiliki anggota yang ditetapkan secara dinamis. Grup dinamis diperbarui secara otomatis berdasarkan properti akun atau perangkat pengguna. Grup statis memerlukan lebih banyak overhead administratif (administrator harus menambahkan dan menghapus anggota secara manual).

1. Di portal Azure, cari dan pilih `Groups`.

1. Perhatikan informasi grup seperti **Jenis** keanggotaan, **Sumber**, dan **Jenis**. Perhatikan juga, jumlah anggota dalam grup. 

1. Pilih **+ Grup** baru dan buat grup baru. 

    | Pengaturan | Nilai |
    | --- | --- |
    | Jenis grup | **Keamanan** |
    | Nama grup | `IT Lab Administrators` (sesuaikan nama jika nama ini tidak tersedia) |
    | Deskripsi grup | `Administrators that manage the IT lab` |
    | Jenis keanggotaan | **Ditetapkan** |

    >**Catatan**: Daftar drop-down Jenis** keanggotaan Anda **mungkin berwarna abu-abu. Di sinilah Anda dapat beralih dari grup yang ditetapkan ke grup dinamis. Ini memerlukan lisensi Entra ID Premium P1 atau P2.

    ![Cuplikan layar buat grup yang ditetapkan.](../media/az104-lab01-create-assigned-group.png)

1. Klik **Tidak ada anggota yang dipilih**.

1. Dari bilah **Tambahkan anggota** , cari akun pengguna Anda. **Pilih** **az104-user1** dan tambahkan ke grup. 

1. Klik **Buat** untuk menyelesaikan pembuatan grup. 

## Tugas 5: Biasakan diri Anda dengan Cloud Shell.

Dalam tugas ini, Anda bekerja dengan Azure Cloud Shell. Azure Cloud Shell adalah terminal interaktif, diautentikasi, dan dapat diakses browser untuk mengelola sumber daya Azure. Ini memberi fleksibilitas untuk memilih pengalaman shell yang paling sesuai dengan cara Anda bekerja, baik Bash atau PowerShell. 

1. **Pilih ikon Cloud Shell** di kanan atas Portal Microsoft Azure. Secara bergantian, Anda dapat menavigasi langsung ke `https://shell.azure.com`.

1. Saat diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**. Bash digunakan dalam tugas berikutnya.

    >**Apakah Anda tahu?**  Jika Anda sebagian besar bekerja dengan sistem Linux, Azure CLI terasa lebih alami. Jika Anda sebagian besar bekerja dengan sistem Windows, Azure PowerShell terasa lebih alami. 

1. **Pada layar Anda tidak memiliki penyimpanan yang dipasang** pilih **Tampilkan pengaturan** tingkat lanjut dan berikan informasi yang diperlukan. Setelah selesai, pilih **Buat penyimpanan**. 

    | Pengaturan | Nilai |
    |  -- | -- |
    | Grup Sumber Daya | **Membuat grup sumber daya baru** |
    | Akun penyimpanan (Buat akun baru menggunakan nama unik global (misalnya: cloudshellstoragemystorage)) | **cloudshellxxxxxxx** |
    | Berbagi file (buat baru) | **shellstorage** |

    >**Catatan:** Jika Anda bekerja di lingkungan lab yang dihosting, Anda perlu mengonfigurasi penyimpanan cloud shell setiap kali lingkungan lab baru dibuat.

    >**Catatan:** Tugas 6 memungkinkan Anda berlatih dengan Azure PowerShell. Tugas 7 memungkinkan Anda berlatih dengan CLI. Anda dapat melakukan kedua tugas atau hanya tugas yang paling Anda minati. 

## Tugas 6: Berlatih dengan Azure PowerShell

Dalam tugas ini, Anda membuat grup sumber daya dan grup Microsoft Azure AD dengan menggunakan sesi Azure PowerShell dalam Cloud Shell.

    >**Note:** Use the arrow keys to move through the command history. Use the tab key to autocomplete commands and parameters.

1. Lanjutkan bekerja di Cloud Shell. Kapan saja gunakan **cls** untuk menghapus jendela perintah.

1. Azure PowerShell menggunakan *format Kata Benda Kata Kerja**-* untuk cmdlet. Misalnya, cmdlet untuk membuat grup sumber daya baru adalah **New-AzResourceGroup**. Untuk melihat cara menggunakan cmdlet, jalankan perintah Get-Help.

   ```powershell
   Get-Help New-AzResourceGroup -detailed
   ```
1. Untuk membuat grup sumber daya dari sesi PowerShell dalam Cloud Shell, jalankan perintah berikut. Perhatikan bahwa perintah yang dimulai dengan tanda dolar ($) membuat variabel yang dapat Anda gunakan dalam perintah selanjutnya. Pastikan Anda menerima pesan yang berhasil. 

   ```powershell
   $location = 'eastus'
   $rgName = 'az104-rg-ps'
   New-AzResourceGroup -Name $rgName -Location $location
   ```

1. Untuk mengambil properti grup sumber daya yang baru dibuat, jalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name $rgName
   ```

1. Sekarang, mari kita coba membuat grup Azure baru.

   ```powershell
   Get-Help New-AzureADGroup -detailed
   ```

1. Menggunakan contoh dalam Bantuan, coba perintah ini. Perhatikan bahwa Anda harus terlebih dahulu menyambungkan ke Microsoft Azure AD.

   ```powershell
   Connect-AzureAD 
   New-AzureADGroup -DisplayName "MyPSgroup" -MailEnabled $false -SecurityEnabled $true -MailNickName "MyPSgroup"
   ```

1. Kembali ke portal Microsoft Azure. Konfirmasikan bahwa Anda memiliki grup sumber daya baru dan grup Azure baru. Anda mungkin perlu Menyegarkan halaman. 

## Tugas 7: Berlatih dengan shell Bash

Dalam tugas ini, Anda membuat grup sumber daya dan grup Azure dengan menggunakan sesi Azure CLI dalam Cloud Shell.

1. Lanjutkan di Cloud Shell. Gunakan menu drop-down untuk beralih ke **Bash**.

    >**Catatan:** Gunakan tombol panah untuk menelusuri riwayat perintah. Gunakan kunci tab untuk melengkapi perintah dan parameter secara otomatis. 

1. Azure CLI menggunakan sintaks yang mudah dibaca. Misalnya, untuk berinteraksi dengan grup sumber daya, perintahnya adalah **grup** az.  

   ```sh
   az group --help
   ```

1. Opsi **buat** terlihat menjanjikan. Perhatikan nama berkapitalisasi membuat variabel yang dapat Anda referensikan dalam perintah berikutnya. 

   ```sh
   RGNAME='az104-rg1-cli'
   LOCATION='eastus'
   az group create --name $RGNAME --location $LOCATION
   ```
   
1. Untuk memverifikasi dan mengambil properti untuk grup sumber daya yang baru dibuat, coba **perintah tampilkan** . 

   ```sh
   az group show --name $RGNAME
   ```
1. Sekarang, mari kita gunakan bantuan untuk mempelajari selengkapnya tentang membuat grup Azure.

    ```sh
    az ad group --help
    ```

1. **Buat** grup dan **cantumkan** grup untuk diverifikasi.

   ```sh
   az ad group create --display-name MyCLIgroup --mail-nickname MyCLIgroup
   az ad group list
   ```

1. Kembali ke portal Microsoft Azure. Konfirmasikan bahwa Anda memiliki grup sumber daya baru dan grup Azure baru. Anda mungkin perlu Menyegarkan halaman.   
    
## Tinjau titik utama lab

Selamat atas penyelesaian lab. Berikut adalah beberapa jalur utama untuk lab ini:

+ portal Azure adalah cara yang baik untuk mulai membuat dan mengelola sumber daya Azure. Administrator dapat menyesuaikan portal dan berbagi dasbor.
+ Grup sumber daya adalah cara pengelompokan sumber daya terkait,. Anda dapat menggunakan grup sumber daya untuk proyek, departemen, atau aplikasi. Ini memudahkan untuk mengelola dan memantau sekelompok sumber daya terkait. 
+ Ada berbagai jenis akun pengguna di ID Microsoft Entra. Setiap jenis akun pengguna memiliki tingkat akses khusus untuk cakupan pekerjaan yang diharapkan. 
+ Grup akun grup bersama-sama pengguna atau perangkat terkait. Keanggotaan grup dapat ditetapkan secara statis atau dinamis. 
+ Cloud Shell adalah terminal interaktif dan terautentikasi untuk mengelola sumber daya Azure. Cloud Shell menyediakan akses ke Bash atau Azure PowerShell.
+ Azure PowerShell dan Bash menyediakan cara berskrip untuk membuat sumber daya. 

## Membersihkan sumber daya Anda

Jika Anda bekerja dengan langganan Anda sendiri membutuhkan waktu satu menit untuk menghapus sumber daya lab. Ini akan memastikan sumber daya dibebankan dan biaya diminimalkan. Cara term mudah untuk menghapus sumber daya lab adalah dengan menghapus grup sumber daya lab. 

+ Di portal Azure, pilih grup sumber daya, pilih **Hapus grup** sumber daya, **Masukkan nama** grup sumber daya, lalu klik **Hapus**.

+ Menggunakan Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.

+ Menggunakan CLI, `az group delete --name resourceGroupName`.

