---
lab:
  title: 'Lab 10: Menerapkan Perlindungan Data'
  module: Administer Data Protection
---

# Lab 10 - Menerapkan Perlindungan Data

## Pengenalan lab    

Di lab ini, Anda mempelajari tentang pencadangan dan pemulihan komputer virtual Azure. Anda belajar membuat vault Layanan Pemulihan dan kebijakan pencadangan untuk komputer virtual Azure. Anda mempelajari tentang pemulihan bencana dengan Azure Site Recovery. 

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur di lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkahnya ditulis menggunakan **US** Timur dan **AS** Barat.

## Perkiraan waktu: 40 menit

## Skenario lab

Organisasi Anda sedang mengevaluasi Azure Recovery Services untuk pencadangan dan pemulihan komputer virtual Azure. Organisasi ingin mengidentifikasi metode melindungi data dari kehilangan data yang tidak disengaja atau berbahaya.

## Simulasi lab interaktif

Ada simulasi lab interaktif yang mungkin berguna bagi Anda untuk topik ini. Simulasi ini memungkinkan Anda mengklik skenario serupa dengan kecepatan Anda sendiri. Ada perbedaan antara simulasi interaktif dan lab ini, tetapi banyak konsep intinya sama. Langganan Azure tidak diperlukan.

+ **[Cadangkan komputer virtual dan file lokal.](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2016)**. Buat vault layanan pemulihan dan terapkan pencadangan komputer virtual Azure. Terapkan pencadangan file dan folder lokal menggunakan agen Microsoft Azure Recovery Services. Cadangan lokal berada di luar cakupan lab ini, tetapi mungkin berguna untuk melihat langkah-langkah tersebut. 

## Tugas

+ Tugas 1: Memprovisikan lingkungan lab
+ Tugas 2: Membuat vault Layanan Pemulihan
+ Tugas 3: Menerapkan pencadangan tingkat komputer virtual Azure
+ Tugas 4: Memantau Azure Backup
+ Tugas 5: Menerapkan Azure Site Recovery untuk komputer virtual

## Perkiraan waktu: 40 menit

## Diagram arsitektur

![Diagram tugas arsitektur.](../media/az104-lab10-architecture-diagram.png)

## Tugas 1: Memprovisikan lingkungan lab

Dalam tugas ini, Anda akan menggunakan templat untuk menyebarkan komputer virtual. Komputer virtual akan digunakan untuk menguji skenario pencadangan yang berbeda.

1. Jika perlu, unduh **\\file lab Allfiles\\Labs\\10\\az104-10-vms-edge-template.json** .

1. Masuk ke **portal Azure** - `https://portal.azure.com`.

1. Cari dan pilih `Deploy a custom template`.

1. Pada halaman penyebaran kustom, pilih **Bangun templat Anda sendiri di editor**.

1. Pada halaman edit templat, pilih **Muat file**.

1. Temukan dan pilih **\\file Allfiles\\Lab10\\az104-10-vms-edge-template.json** dan pilih **Buka**.
2. 
   >**Catatan:** Luangkan waktu sejenak untuk meninjau templat. Berapa banyak komputer virtual dan jaringan virtual yang sedang disebarkan? 

1. Pilih **Simpan**.
 
   >**Catatan:** Perhatikan bahwa templat ini memiliki banyak pemisah yang dapat diubah administrator. 

1. Gunakan informasi berikut untuk menyelesaikan bidang penyebaran kustom, meninggalkan semua bidang lain dengan nilai defaultnya:

    | Pengaturan       | Nilai         | 
    | ---           | ---           |
    | Langganan  | Langganan Azure Anda |
    | Grup sumber daya| `az104-rg10` (Jika perlu, pilih **Buat baru**)
    | Wilayah        | **US Timur**   |
    | Nama Pengguna      | `Student`   |
    | Kata sandi      | Berikan kata sandi yang kompleks |

1. Pilih **Tinjau + Buat**, kemudian pilih **Buat**.

    >**Catatan:** Tunggu hingga templat disebarkan, lalu pilih **Buka sumber daya**. Anda harus memiliki satu komputer virtual dalam satu jaringan virtual. 

## Tugas 2: Membuat vault Layanan Pemulihan

Dalam tugas ini, Anda akan membuat vault Layanan Pemulihan. Vault Layanan Pemulihan menyediakan data cadangan untuk komputer virtual Azure.

1. Di portal Azure, cari dan pilih `Recovery Services vaults` dan, pada bilah **vault** Layanan Pemulihan, klik **+ Buat**.

1. Pada bilah **Buat Recovery Services vault**, tentukan pengaturan berikut:

    | Pengaturan | Value |
    | --- | --- |
    | Langganan | nama langganan Azure Anda |
    | Grup sumber daya | **az104-rg10vault** (jika perlu, pilih Buat baru) |
    | Nama Vault | `az104-vault1` |
    | Wilayah | **US** Barat (atau wilayah yang tidak Anda gunakan dalam tugas sebelumnya untuk menyebarkan VM) |

    >**Catatan**: Pastikan Anda menentukan wilayah yang berbeda tempat Anda menyebarkan komputer virtual di tugas sebelumnya.

    ![Cuplikan layar vault layanan pemulihan.](../media/az104-lab10-create-rsv.png)

1. Klik **Tinjau + Buat**, pastikan validasi lolos lalu klik **Buat**.

    >**Catatan**: Tunggu hingga penyebaran selesai. Penyebaran akan memakan waktu kurang dari 1 menit.

1. Saat penyebaran selesai, klik **Buka Sumber Daya**.

1. Pada bilah **vault Layanan Pemulihan az104-vault1**, di bagian **Pengaturan**, klik **Properti**.

1. Pada bilah **az104-vault1 - Properties**, klik tautan Perbarui** di **bawah **label Konfigurasi** Cadangan.

1. Pada bilah **Konfigurasi** Cadangan, tinjau pilihan untuk **Jenis** replikasi Penyimpanan. Biarkan pengaturan default **Geo-redundant** di tempatnya dan tutup bilah.

    >**Catatan**: Pengaturan ini hanya dapat dikonfigurasi jika tidak ada item cadangan yang ada.

1. Kembali ke bilah **az104-vault1 - Properties**, klik **tautan Perbarui** di bawah **label Keamanan Pengaturan**.

1. Pada bilah **Pengaturan Keamanan**, perhatikan bahwa **Penghapusan Sementara (Untuk beban kerja yang berjalan di Azure)** **Diaktifkan**.

1. Tutup bilah **Pengaturan** Keamanan dan, kembali ke bilah **vault Layanan Pemulihan az104-vault1**, klik **Gambaran Umum**.

## Tugas 3: Menerapkan pencadangan tingkat komputer virtual Azure

Dalam tugas ini, Anda akan menerapkan pencadangan tingkat mesin virtual Azure. Sebagai bagian dari cadangan VM, Anda harus menentukan kebijakan pencadangan dan retensi yang berlaku untuk cadangan. VM yang berbeda dapat memiliki kebijakan pencadangan dan retensi yang berbeda yang ditetapkan untuk mereka.

   >**Catatan**: Sebelum Anda memulai tugas ini, pastikan bahwa penyebaran yang Anda mulai di tugas pertama lab ini telah berhasil diselesaikan.

1. Pada bilah **vault Layanan Pemulihan az104-vault1** , klik **Gambaran Umum**, lalu klik **+ Cadangan**.

1. Pada bilah **Sasaran Pencadangan**, tentukan pengaturan berikut:

    | Pengaturan | Value |
    | --- | --- |
    | Di mana beban kerja Anda berjalan? | **Azure** (perhatikan opsi Anda yang lain) |
    | Apa yang ingin Anda cadangkan? | **Komputer virtual** (perhatikan opsi Anda yang lain |

1. Pada bilah **Sasaran Pencadangan**, klik **Cadangan**.

1. Pada **Kebijakan pencadangan**, tinjau pengaturan **DefaultPolicy** dan pilih **Buat kebijakan baru**.

1. Tetapkan kebijakan pencadangan baru dengan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | ---- | ---- |
    | Nama Azure Policy | `az104-policy` |
    | Frekuensi
           | **Harian** |
    | Waktu | **12:00 Siang** |
    | Timezone | nama zona waktu lokal Anda |
    | Pertahankan snapshot pemulihan instans untuk | **2** Hari |

    ![Cuplikan layar halaman kebijakan cadangan.](../media/az104-lab10-backup-policy.png)

1. Klik **OK** untuk membuat kebijakan, lalu di bagian **Virtual Machines**, pilih **Tambah**.

1. Pada bilah **Pilih mesin virtual**, pilih **az-104-10-vm0**, klik **OK**, dan kembali ke bilah **Cadangan** , klik **Aktifkan pencadangan**.

    >**Catatan**: Tunggu hingga cadangan diaktifkan. Ini akan memakan waktu sekitar 2 menit.

1. Navigasikan kembali ke bilah **vault Layanan Pemulihan az104-vault1** , di bagian **Item** yang dilindungi, klik **Item** cadangan, lalu klik **entri komputer** virtual Azure.

1. Pada bilah **Item Cadangan (Mesin Virtual Azure)** pilih tautan **Lihat detail** untuk **az104-10-vm0**, dan tinjau nilai **Cadangan Entri Pra-Pemeriksaan** dan **Status Cadangan Terakhir**.

1. Pada bilah **az104-10-vm0** Item Cadangan, klik **Cadangkan sekarang**, terima nilai default di daftar dropdown **Pertahankan Pencadagan Sampai**, dan klik **Ok**.

    >**Catatan**: Jangan tunggu hingga pencadangan selesai tetapi lanjutkan ke tugas berikutnya.

## Tugas 4: Memantau Azure Backup

Dalam tugas ini, Anda akan menyebarkan akun penyimpanan Azure. Kemudian Anda akan mengonfigurasi vault untuk mengirim log dan metrik ke akun penyimpanan. Repositori ini kemudian dapat digunakan dengan Analitik Log atau solusi pemantauan pihak ketiga lainnya.

1. Dari portal Azure, cari dan pilih `Storage accounts`.

1. Pada halaman Akun penyimpanan, pilih **Buat**.

1. Gunakan informasi berikut untuk menentukan akun penyimpanan, lalu pilih **Tinjau**.

    | Pengaturan | Value |
    | --- | --- | 
    | Langganan          | *Langganan Anda*    |
    | Grup sumber daya        | **az104-rg10**        |
    | Nama akun penyimpanan  | Berikan nama yang unik secara global, misalnya *backupdiag1042024123*    |
    | Wilayah                | **US** Timur (atau wilayah di dekat Anda)    |

1. Pada tab Tinjau, pilih **Buat**.

    >**Catatan**: Tunggu hingga penyebaran selesai. Perlu waktu sekitar satu menit.

1. Navigasikan **ke vault Layanan Pemulihan az104-vault1** .

1. Pada halaman **az104-vault1**, pilih **Diagnostik Pengaturan**.

1. Pada halaman Pengaturan diagnostik, pilih **Tambahkan pengaturan** diagnostik.

1. Pada halaman Pengaturan Diagnosis, beri nama pengaturan `Logs and Metrics to storage`.

1. Tempatkan tanda centang di samping katagori log dan metrik berikut:

    - **Data Pelaporan Azure Backup**
    - **Data Pekerjaan Azure Backup Addon**
    - **Data Pemberitahuan Azure Backup Addon**
    - **Pekerjaan Azure Site Recovery**
    - **Peristiwa Azure Site Recovery**
    - **Kesehatan**

1. Di detail Tujuan, letakkan tanda centang di samping **Arsipkan ke akun** penyimpanan.

1. Di bidang drop-down Akun penyimpanan, pilih akun penyimpanan yang Anda sebarkan sebelumnya dalam tugas ini.

1. Pilih **Simpan**.

1. Kembali ke **halaman sumber daya az104-vault1** . 

1. Pada **az104-vault1**, pilih **Pekerjaan** pencadangan.

1. Temukan operasi pencadangan untuk **VM az104-10-vm0** , dan pilih **Detail**

    >**Catatan**: Bergantung pada resolusi layar, Anda mungkin perlu menggulir ke kanan dalam tabel pekerjaan cadangan.

1. Tinjau detail pekerjaan pencadangan yang Anda picu pada VM. Apa dua subtugas dari pekerjaan pencadangan?


## Tugas 5: Menerapkan Azure Site Recovery

Dalam tugas ini, 

1. Cari dan pilih Vault Layanan Pemulihan Anda, **az104-vault1**.

1. Dari bilah **Gambaran Umum** , pilih **+ Aktifkan Site Recovery**.

1. Tinjau opsi Anda lalu pilih di bagian ****Azure Virtual Machines** Aktifkan replikasi**.

1. Pada tab **Sumber** , konfigurasikan pengaturan.

    | Pengaturan | Nilai |
    | ---- | ---- |
    | Wilayah| **US** Timur (baca pemberitahuan tentang replikasi di wilayah yang sama) |
    | Grup sumber daya | **az104-rg10** |
    | Model penyebaran komputer virtual | **Resource Manager** |
    | Pemulihan bencana antar zona ketersediaan | **Tidak** |

1. Pilih **Berikutnya** dan pada tab Komputer virtual **** pilih **az104-10-vm0**.

1. Pilih **Berikutnya** dan pindah ke tab **Pengaturan** replikasi. Perhatikan lokasi target dan informasi jaringan failover. Sumber daya ini akan dibuat secara otomatis. Ambil default dan pilih **Berikutnya**.

1. Pada tab **Kelola** , tinjau parameter.

    | Pengaturan | Nilai |
    | ---- | ---- |
    | Kebijakan replikasi | **Kebijakan retensi** 24 jam (ini dapat diubah dari 0 menjadi 15 hari) |
    | Perbarui pengaturan | **Izinkan ASR mengelola** |

1. Pilih **Berikutnya** lalu **Aktifkan replikasi**.

    >**Catatan**: Mengaktifkan replikasi akan sekitar 15 menit. Tonton pesan pemberitahuan di kanan atas portal.

1. Setelah replikasi selesai, cari dan temukan Vault Layanan Pemulihan Anda, **az104-vault1**.

1. Di bagian **Item** terproteksi, pilih **Item** yang direplikasi. Periksa apakah komputer virtual menunjukkan sehat untuk kesehatan replikasi. Perhatikan bahwa status akan menampilkan status sinkronisasi (mulai dari 0%) dan pada akhirnya menunjukkan **Dilindungi** setelah sinkronisasi awal selesai. 

    **Apakah Anda tahu?** Ini adalah praktik yang baik untuk [menguji failover VM](https://learn.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure#run-a-test-failover-for-a-single-vm) yang dilindungi.


## Poin penting

Selamat atas penyelesaian lab. Berikut adalah takeaway utama untuk lab ini. 

+ Layanan Azure Backup menyediakan solusi sederhana, aman, dan hemat biaya untuk mencadangkan dan memulihkan data Anda.
+ Azure Backup dapat melindungi sumber daya lokal dan cloud termasuk komputer virtual dan berbagi file.
+ Kebijakan Azure Backup mengonfigurasi frekuensi pencadangan dan periode retensi untuk titik pemulihan. 
+ Azure Site Recovery adalah solusi pemulihan bencana yang memberikan perlindungan untuk komputer virtual dan aplikasi Anda.
+ Azure Site Recovery mereplikasi beban kerja Anda ke situs sekunder, dan jika terjadi pemadaman atau bencana, Anda dapat melakukan failover ke situs sekunder dan melanjutkan operasi dengan waktu henti minimal.
+ Vault Layanan Pemulihan menyimpan data cadangan Anda dan meminimalkan overhead manajemen.


## Membersihkan sumber daya Anda

Jika Anda bekerja dengan langganan Anda sendiri membutuhkan waktu satu menit untuk menghapus sumber daya lab. Ini akan memastikan sumber daya dibebankan dan biaya diminimalkan. Cara term mudah untuk menghapus sumber daya lab adalah dengan menghapus grup sumber daya lab. 

+ Di portal Azure, pilih grup sumber daya, pilih **Hapus grup** sumber daya, **Masukkan nama** grup sumber daya, lalu klik **Hapus**.
+ Menggunakan Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Menggunakan CLI, `az group delete --name resourceGroupName`.


