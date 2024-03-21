---
lab:
  title: 'Lab 10: Menerapkan Perlindungan Data'
  module: Administer Data Protection
---

# Lab 10 - Menerapkan Perlindungan Data

## Pengenalan lab    

Di lab ini, Anda mempelajari tentang pencadangan dan pemulihan komputer virtual Azure. Anda belajar membuat vault Layanan Pemulihan dan kebijakan pencadangan untuk komputer virtual Azure. Anda mempelajari tentang pemulihan bencana dengan Azure Site Recovery. 

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur di lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkahnya ditulis menggunakan **US** Timur dan **AS** Barat.

## Perkiraan waktu: 50 menit

## Skenario lab

Organisasi Anda sedang mengevaluasi cara mencadangkan dan memulihkan komputer virtual Azure dari kehilangan data yang tidak disengaja atau berbahaya. Selain itu, organisasi ingin menjelajahi menggunakan Azure Site Recovery untuk skenario pemulihan bencana. 

## Simulasi lab interaktif

Ada simulasi lab interaktif yang mungkin berguna bagi Anda untuk topik ini. Simulasi ini memungkinkan Anda mengklik skenario serupa dengan kecepatan Anda sendiri. Ada perbedaan antara simulasi interaktif dan lab ini, tetapi banyak konsep intinya sama. Langganan Azure tidak diperlukan.

+ **[Cadangkan komputer virtual dan file lokal.](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2016)**. Buat vault layanan pemulihan dan terapkan pencadangan komputer virtual Azure. Terapkan pencadangan file dan folder lokal menggunakan agen Microsoft Azure Recovery Services. Cadangan lokal berada di luar cakupan lab ini tetapi mungkin berguna untuk melihat langkah-langkah tersebut. 

## Keterampilan pekerjaan

+ Tugas 1: Gunakan templat untuk memprovisikan infrastruktur.
+ Tugas 2: Membuat dan mengonfigurasi vault Layanan Pemulihan.
+ Tugas 3: Mengonfigurasi pencadangan tingkat komputer virtual Azure.
+ Tugas 4: Pantau Azure Backup.
+ Tugas 5: Aktifkan replikasi komputer virtual. 

## Perkiraan waktu: 40 menit

## Diagram arsitektur

![Diagram tugas arsitektur.](../media/az104-lab10-architecture.png)

## Tugas 1: Menggunakan templat untuk memprovisikan infrastruktur

Dalam tugas ini, Anda akan menggunakan templat untuk menyebarkan komputer virtual. Komputer virtual akan digunakan untuk menguji skenario pencadangan yang berbeda.

1. **\\Unduh file lab Allfiles\\Lab10\\**.

1. Masuk ke **portal Azure** - `https://portal.azure.com`.

1. Cari dan pilih `Deploy a custom template`.

1. Pada halaman penyebaran kustom, pilih **Bangun templat Anda sendiri di editor**.

1. Pada halaman edit templat, pilih **Muat file**.

1. Temukan dan pilih **\\file az104-10-vms-edge-template.json** Allfiles\\Lab10\\dan pilih **Buka**.

   >**Catatan:** Luangkan waktu sejenak untuk meninjau templat. Kami menyebarkan jaringan virtual dan komputer virtual sehingga kami dapat menunjukkan pencadangan dan pemulihan. 

1. **Simpan** perubahan Anda.

1. Pilih **Edit parameter** lalu **Muat file**.

1. Muat dan pilih **\\file az104-10-vms-edge-parameters.json** Allfiles\\Lab10\\.

1. **Simpan** perubahan Anda.

1. Gunakan informasi berikut untuk menyelesaikan bidang penyebaran kustom, meninggalkan semua bidang lain dengan nilai defaultnya:

    | Pengaturan       | Nilai         | 
    | ---           | ---           |
    | Langganan  | Langganan Azure Anda |
    | Grup sumber daya| `az104-rg-region1` (Jika perlu, pilih **Buat baru**)
    | Wilayah        | **US Timur**   |
    | Nama Pengguna      | **localadmin**   |
    | Kata sandi      | Berikan kata sandi yang kompleks |

1. Pilih **Tinjau + Buat**, kemudian pilih **Buat**.

    >**Catatan:** Tunggu hingga templat disebarkan, lalu pilih **Buka sumber daya**. Anda harus memiliki satu komputer virtual dalam satu jaringan virtual. 

## Tugas 2: Membuat dan mengonfigurasi vault Layanan Pemulihan

Dalam tugas ini, Anda akan membuat vault Layanan Pemulihan. Vault Layanan Pemulihan menyediakan penyimpanan untuk data komputer virtual. 

1. Di portal Azure, cari dan pilih `Recovery Services vaults` dan, pada bilah **vault** Layanan Pemulihan, klik **+ Buat**.

1. Pada bilah **Buat Recovery Services vault**, tentukan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure Anda |
    | Grup sumber daya | `az104-rg-region1`  |
    | Nama Vault | `az104-rsv-region1` |
    | Wilayah | **US Timur** |

    >**Catatan**: Pastikan Anda menentukan wilayah yang sama tempat Anda menyebarkan komputer virtual di tugas sebelumnya.

    ![Cuplikan layar vault layanan pemulihan.](../media/az104-lab10-create-rsv.png)

1. Klik **Tinjau + Buat**, pastikan validasi lolos lalu klik **Buat**.

    >**Catatan**: Tunggu hingga penyebaran selesai. Penyebaran harus memakan waktu beberapa menit. 

1. Saat penyebaran selesai, klik **Buka Sumber Daya**.

1. Pada bilah vault Layanan Pemulihan, di bagian **Pengaturan**, klik **Properti**.

1. **Pilih tautan Perbarui** di bawah **label Konfigurasi** Cadangan.

1. Pada bilah **Konfigurasi** Cadangan, tinjau pilihan untuk **Jenis** replikasi Penyimpanan. Biarkan pengaturan default **Geo-redundant** di tempatnya dan tutup bilah.

    >**Catatan**: Pengaturan ini hanya dapat dikonfigurasi jika tidak ada item cadangan yang ada.
    
    >**Apakah Anda tahu?** Opsi Pemulihan Lintas Wilayah memungkinkan Anda memulihkan data di [wilayah berpasangan Azure sekunder](https://learn.microsoft.com/azure/backup/backup-create-recovery-services-vault#set-cross-region-restore). 

1. Kembali ke bilah vault Layanan Pemulihan, klik **tautan Perbarui** di bawah **Label pengaturan Keamanan Pengaturan > Penghapusan Sementara dan keamanan**.

1. Pada bilah **Pengaturan Keamanan**, perhatikan bahwa **Penghapusan Sementara (Untuk beban kerja yang berjalan di Azure)** **Diaktifkan**. Perhatikan bahwa **periode** retensi penghapusan sementara adalah **14** hari. 

1. Kembali ke bilah vault Layanan Pemulihan, pilih bilah **Gambaran Umum** .

>**Apakah Anda tahu?** Azure memiliki dua jenis vault: Vault Layanan Pemulihan dan vault Backup. Perbedaan utamanya adalah sumber data yang dapat dicadangkan. Pelajari selengkapnya tentang [perbedaannya](https://learn.microsoft.com/answers/questions/405915/what-is-difference-between-recovery-services-vault).

## Tugas 3: Mengonfigurasi pencadangan tingkat komputer virtual Azure

Dalam tugas ini, Anda akan menerapkan pencadangan tingkat mesin virtual Azure. Sebagai bagian dari cadangan VM, Anda harus menentukan kebijakan pencadangan dan retensi yang berlaku untuk cadangan. VM yang berbeda dapat memiliki kebijakan pencadangan dan retensi yang berbeda yang ditetapkan untuk mereka.

   >**Catatan**: Sebelum Anda memulai tugas ini, pastikan bahwa penyebaran yang Anda mulai di tugas pertama lab ini telah berhasil diselesaikan.

1. Pada bilah vault Layanan Pemulihan, klik **Gambaran Umum**, lalu klik **+ Cadangan**.

1. Pada bilah **Sasaran Pencadangan**, tentukan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Di mana beban kerja Anda berjalan? | **Azure** (perhatikan opsi Anda yang lain) |
    | Apa yang ingin Anda cadangkan? | **Komputer virtual** (perhatikan opsi Anda yang lain |

1. Pilih **Backup**.

1. Perhatikan ada dua **sub jenis** Kebijakan: **Ditingkatkan** dan **Standar**. Tinjau pilihan dan pilih **Standar**. 

1. Di **Kebijakan** pencadangan, pilih **Buat kebijakan** baru.

1. Tetapkan kebijakan pencadangan baru dengan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | ---- | ---- |
    | Nama Azure Policy | `az104-backup` |
    | Frekuensi
           | **Harian** |
    | Waktu | **12:00 Siang** |
    | Timezone | nama zona waktu lokal Anda |
    | Pertahankan snapshot pemulihan instans untuk | **2** Hari |

    ![Cuplikan layar halaman kebijakan cadangan.](../media/az104-lab10-backup-policy.png)

1. Klik **OK** untuk membuat kebijakan, lalu di bagian **Virtual Machines**, pilih **Tambah**.

1. Pada bilah **Pilih komputer** virtual, pilih **az-104-10-vm0**, klik **OK**, lalu kembali pada bilah **Cadangan** , klik **Aktifkan cadangan**.

    >**Catatan**: Tunggu hingga cadangan diaktifkan. Ini akan memakan waktu sekitar 2 menit.

1. Di bagian **Item** terproteksi, klik **Item** cadangan, lalu klik **entri komputer** virtual Azure.

1. **Pilih tautan Tampilkan detail** untuk **az104-10-vm0**, dan tinjau nilai **entri Pra-Pemeriksaan** Cadangan dan **Status** Pencadangan Terakhir.

    >**Catatan:** Perhatikan bahwa pencadangan tertunda.
    
1. Pilih **Cadangkan sekarang**, terima nilai default di **daftar drop-down Pertahankan Cadangan Hingga** , dan klik **OK**.

    >**Catatan**: Jangan tunggu hingga pencadangan selesai tetapi lanjutkan ke tugas berikutnya.

## Tugas 4: Memantau Azure Backup

Dalam tugas ini, Anda akan menyebarkan akun penyimpanan Azure. Kemudian Anda akan mengonfigurasi vault untuk mengirim log dan metrik ke akun penyimpanan. Repositori ini kemudian dapat digunakan dengan Log Analytics atau solusi pemantauan pihak ketiga lainnya.

1. Dari portal Azure, cari dan pilih `Storage accounts`.

1. Pada halaman Akun penyimpanan, pilih **Buat**.

1. Gunakan informasi berikut untuk menentukan akun penyimpanan, lalu pilih **Tinjau**.

    | Pengaturan | Nilai |
    | --- | --- | 
    | Langganan          | *Langganan Anda*    |
    | Grup sumber daya        | **az104-rg-region1**        |
    | Nama akun penyimpanan  | Berikan nama yang unik secara global   |
    | Wilayah                | **US Timur**   |

1. Pada tab Tinjau, pilih **Buat**.

    >**Catatan**: Tunggu hingga penyebaran selesai. Perlu waktu sekitar satu menit.

1. Cari dan pilih vault Layanan Pemulihan Anda.

1. Pilih **Diagnostik Pengaturan** lalu pilih **Tambahkan pengaturan** diagnostik.

1. Beri nama pengaturan `Logs and Metrics to storage`.

1. Tempatkan tanda centang di samping kategori log dan metrik berikut:

    - **Data Pelaporan Azure Backup**
    - **Data Pekerjaan Azure Backup Addon**
    - **Data Pemberitahuan Azure Backup Addon**
    - **Pekerjaan Azure Site Recovery**
    - **Peristiwa Azure Site Recovery**
    - **Kesehatan**

1. Di detail Tujuan, letakkan tanda centang di samping **Arsipkan ke akun** penyimpanan.

1. Di bidang drop-down Akun penyimpanan, pilih akun penyimpanan yang Anda sebarkan sebelumnya dalam tugas ini.

1. Pilih **Simpan**.

1. Kembali ke vault Layanan Pemulihan Anda, di bilah **Pemantauan** pilih **Pekerjaan** pencadangan.

1. Temukan operasi pencadangan untuk **komputer virtual az104-10-vm0** . 

1. Tinjau detail pekerjaan pencadangan.

## Tugas 5: Mengaktifkan replikasi komputer virtual

1. Di portal Azure, cari dan pilih `Recovery Services vaults` dan, pada bilah **vault** Layanan Pemulihan, klik **+ Buat**.

1. Pada bilah **Buat Recovery Services vault**, tentukan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure Anda |
    | Grup sumber daya | `az104-rg-region2` (Jika perlu, pilih **Buat baru**) |
    | Nama Vault | `az104-rsv-region2` |
    | Wilayah | **US Barat** |

    >**Catatan**: Pastikan Anda menentukan wilayah yang **berbeda** dari komputer virtual.

1. Klik **Tinjau + Buat**, pastikan validasi lolos lalu klik **Buat**.

    >**Catatan**: Tunggu hingga penyebaran selesai. Penyebaran harus memakan waktu beberapa menit. 

1. Cari dan pilih komputer `az104-10-vm0` virtual.

1. Di bilah **Pencadangan + Pemulihan bencana** , pilih **Pemulihan bencana**. 

1. Pilih **Aktifkan replikasi**.

1. Pada tab **Dasar** , perhatikan **wilayah** Target.

1. Pindah ke tab **Pengaturan** tingkat lanjut. Pilihan sumber daya telah dibuat untuk Anda. Penting untuk meninjaunya. 

1. Verifikasi langganan Anda, grup sumber daya vm, jaringan virtual, dan pengaturan ketersediaan (ambil default).

1. Di **Pengaturan** penyimpanan pilih **Perlihatkan detail**.

    | Pengaturan | Nilai |
    | ---- | ---- |
    | Churn untuk vm | **Churn normal**  |
    | Akun penyimpanan cache | **(baru) xxx**  |

   >**Catatan:** Penting bahwa kedua pengaturan ini diisi, atau validasi akan gagal. Jika nilai tidak ada, coba refresh halaman. Jika tidak berhasil, buat akun penyimpanan kosong lalu kembali ke halaman ini.

1. Di **Pengaturan** replikasi pilih **Perlihatkan detail**. Perhatikan bahwa vault sumber daya pemulihan Anda di wilayah 2 dipilih secara otomatis.

1. Pilih **Tinjau + Mulai replikasi** lalu **Aktifkan replikasi**.

    >**Catatan**: Mengaktifkan replikasi akan memakan waktu 10-15 menit. Tonton pesan pemberitahuan di kanan atas portal. Saat Anda menunggu, pertimbangkan untuk meninjau tautan pelatihan mandiri di akhir halaman ini.
    
1. Setelah replikasi selesai, cari dan temukan Vault Layanan Pemulihan Anda, **az104-rsv-region2**. Anda mungkin perlu melakukan **Refresh** halamannya. 

1. Di bagian **Item** terproteksi, pilih **Item** yang direplikasi.

1. Periksa apakah komputer virtual menunjukkan sehat untuk kesehatan replikasi. Perhatikan bahwa status akan menampilkan status sinkronisasi (mulai dari 0%) dan pada akhirnya menunjukkan **Dilindungi** setelah sinkronisasi awal selesai.

   ![Cuplikan layar halaman item yang direplikasi.](../media/az104-lab10-replicated-items.png)

1. Pilih komputer virtual untuk melihat detail selengkapnya.
   
>**Apakah Anda tahu?** Ini adalah praktik yang baik untuk [menguji failover VM](https://learn.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure#run-a-test-failover-for-a-single-vm) yang dilindungi.

## Membersihkan sumber daya Anda

Jika Anda bekerja dengan **langganan** Anda sendiri membutuhkan waktu satu menit untuk menghapus sumber daya lab. Ini akan memastikan sumber daya dibebankan dan biaya diminimalkan. Cara term mudah untuk menghapus sumber daya lab adalah dengan menghapus grup sumber daya lab. 

+ Di portal Azure, pilih grup sumber daya, pilih **Hapus grup** sumber daya, **Masukkan nama** grup sumber daya, lalu klik **Hapus**.
+ Menggunakan Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Menggunakan CLI, `az group delete --name resourceGroupName`.


## Poin penting

Selamat atas penyelesaian lab. Berikut adalah takeaway utama untuk lab ini. 

+ Layanan Azure Backup menyediakan solusi sederhana, aman, dan hemat biaya untuk mencadangkan dan memulihkan data Anda.
+ Azure Backup dapat melindungi sumber daya lokal dan cloud termasuk komputer virtual dan berbagi file.
+ Kebijakan Azure Backup mengonfigurasi frekuensi pencadangan dan periode retensi untuk titik pemulihan. 
+ Azure Site Recovery adalah solusi pemulihan bencana yang memberikan perlindungan untuk komputer virtual dan aplikasi Anda.
+ Azure Site Recovery mereplikasi beban kerja Anda ke situs sekunder, dan jika terjadi pemadaman atau bencana, Anda dapat melakukan failover ke situs sekunder dan melanjutkan operasi dengan waktu henti minimal.
+ Vault Layanan Pemulihan menyimpan data cadangan Anda dan meminimalkan overhead manajemen.

## Pelajari lebih lanjut dengan pelatihan mandiri

+ [Lindungi komputer virtual Anda dengan menggunakan Azure Backup](https://learn.microsoft.com/training/modules/protect-virtual-machines-with-azure-backup/). Gunakan Azure Backup untuk membantu melindungi server lokal, komputer virtual, SQL Server, berbagi file Azure, dan beban kerja lainnya.
+ [Lindungi infrastruktur Azure Anda dengan Azure Site Recovery](https://learn.microsoft.com/en-us/training/modules/protect-infrastructure-with-site-recovery/). Berikan pemulihan bencana untuk infrastruktur Azure Anda dengan menyesuaikan replikasi, failover, dan failback komputer virtual Azure dengan Azure Site Recovery.
