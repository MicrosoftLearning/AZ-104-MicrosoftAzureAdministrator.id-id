---
lab:
  title: 'Lab 11: Menerapkan Pemantauan'
  module: Administer Monitoring
---

# Lab 11 - Implementasi Pemantauan

## Pengenalan lab

Di lab ini, Anda mempelajari tentang Azure Monitor. Anda belajar membuat pemberitahuan dan mengirimkannya ke grup tindakan. Anda memicu dan menguji pemberitahuan dan memeriksa log aktivitas.  

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur di lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkahnya ditulis menggunakan **US** Timur.

## Perkiraan waktu: 40 menit

## Skenario lab

Organisasi Anda telah memigrasikan infrastruktur mereka ke Azure. Penting bagi Administrator untuk diberi tahu tentang perubahan infrastruktur yang signifikan. Anda berencana untuk memeriksa kemampuan Azure Monitor, termasuk Analitik Log.

## Simulasi lab interaktif

Ada simulasi lab interaktif yang mungkin berguna bagi Anda untuk topik ini. Simulasi ini memungkinkan Anda mengklik skenario serupa dengan kecepatan Anda sendiri. Ada perbedaan antara simulasi interaktif dan lab ini, tetapi banyak konsep intinya sama. Langganan Azure tidak diperlukan.

+ [Menerapkan pemantauan](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2017). Buat ruang kerja Analitik Log dan solusi azure-automation. Tinjau pengaturan pemantauan dan diagnostik untuk komputer virtual. Tinjau fungsionalitas Azure Monitor dan Analitik Log. 

## Diagram arsitektur

![Diagram tugas arsitektur](../media/az104-lab11-architecture.png)

## Keterampilan pekerjaan

+ Tugas 1: Gunakan templat untuk memprovisikan infrastruktur.
+ Tugas 2: Buat pemberitahuan.
+ Tugas 3: Mengonfigurasi pemberitahuan grup tindakan.
+ Tugas 4: Memicu pemberitahuan dan mengonfirmasi bahwa pemberitahuan berfungsi.
+ Tugas 5: Mengonfigurasi aturan pemrosesan pemberitahuan.
+ Tugas 6: Gunakan kueri log Azure Monitor.

## Tugas 1: Menggunakan templat untuk memprovisikan infrastruktur

Dalam tugas ini, Anda akan menggunakan komputer virtual yang akan digunakan untuk menguji skenario monitoring.

1. **\\Unduh file lab Allfiles\\Lab11\\az104-11-vm-template.json** ke komputer Anda.

1. Masuk ke **portal Azure** - `https://portal.azure.com`.

1. Dari portal Azure, cari dan pilih `Deploy a custom template`.

1. Pada halaman penyebaran kustom, pilih **Bangun templat Anda sendiri di editor**.

1. Pada halaman edit templat, pilih **Muat file**.

1. Temukan dan pilih **\\file az104-11-vm-template.json** Allfiles\\Labs11\\dan pilih **Buka**.

1. Pilih **Simpan**.

1. Gunakan informasi berikut untuk menyelesaikan bidang penyebaran kustom, meninggalkan semua bidang lain dengan nilai defaultnya:

    | Pengaturan       | Nilai         | 
    | ---           | ---           |
    | Langganan  | Langganan Azure Anda |
    | Grup sumber daya| `az104-rg11` (Jika perlu, pilih **Buat baru**)
    | Wilayah        | **US Timur**   |
    | Nama Pengguna      | `localadmin`   |
    | Kata sandi      | Berikan kata sandi yang kompleks |
    
1. Pilih **Tinjau + Buat**, kemudian pilih **Buat**.

1. Tunggu hingga penyebaran selesai, lalu klik **Buka grup** sumber daya.

1. Tinjau sumber daya apa yang disebarkan. Harus ada satu jaringan virtual dengan satu komputer virtual.

**Mengonfigurasi Azure Monitor untuk komputer virtual (ini akan digunakan dalam tugas terakhir)**

1. Di portal, cari dan pilih **Pantau**.

1. Luangkan waktu satu menit untuk meninjau semua wawasan, deteksi, triase, dan alat diagnosis yang tersedia.

1. Pilih **Tampilkan** di kotak **Wawasan** VM, lalu pilih **Konfigurasikan Wawasan**.

1. Pilih komputer virtual Anda, lalu **Aktifkan** (dua kali).

1. Ambil default untuk aturan langganan dan pengumpulan data, lalu pilih **Konfigurasikan**. 

1. Dibutuhkan beberapa menit bagi agen komputer virtual untuk menginstal dan mengonfigurasi, lanjutkan ke langkah berikutnya. 
   
## Tugas 2: Membuat pemberitahuan

Dalam tugas ini, Anda membuat pemberitahuan saat komputer virtual dihapus. 

1. Lanjutkan di **halaman Monitor** , pilih **Pemberitahuan**. 

1. Pilih **Buat +** dan pilih **Aturan** pemberitahuan. 

1. Pilih kotak untuk grup sumber daya, lalu pilih **Terapkan**. Pemberitahuan ini akan berlaku untuk komputer virtual apa pun di grup sumber daya. Atau, Anda hanya dapat menentukan satu komputer tertentu. 

1. Pilih tab **Kondisi** lalu pilih **tautan Lihat semua sinyal** .

1. Cari dan pilih **Hapus Komputer Virtual (Komputer Virtual)**. Perhatikan sinyal bawaan lainnya. Pilih **Terapkan**

1. **Di area Logika** pemberitahuan (gulir ke bawah), tinjau **Pilihan tingkat** peristiwa. Biarkan default **Semua dipilih**.

1. **Tinjau Pilihan status**. Biarkan default **Semua dipilih**.

1. Biarkan panel **Buat aturan** pemberitahuan terbuka untuk tugas berikutnya.

## Tugas 3: Mengonfigurasi pemberitahuan grup tindakan

Dalam tugas ini, jika pemberitahuan dipicu, kirim pemberitahuan email ke tim operasi. 

1. Lanjutkan bekerja pada pemberitahuan Anda. Pilih **Berikutnya: Tindakan**, lalu pilih **Buat grup** tindakan.

    >**Apakah Anda tahu?** Anda dapat menambahkan hingga lima grup tindakan ke aturan pemberitahuan. Grup tindakan dijalankan secara bersamaan, tanpa urutan tertentu. Beberapa aturan pemberitahuan dapat menggunakan grup tindakan yang sama. 

1. Pada tab **Dasar**, masukkan nilai berikut untuk setiap pengaturan.

    | Pengaturan | Nilai |
    |---------|---------|
    | **Detail proyek** |
    | Langganan | langganan Anda |
    | Grup sumber daya | **az104-rg11** |
    | Wilayah | **Global** (default) |
    | **Detail instans** |
    | Nama grup tindakan | `Alert the operations team` (harus unik dalam grup sumber daya) |
    | Nama tampilan | `AlertOpsTeam` |

1. Pilih **Berikutnya: Pemberitahuan** dan masukkan nilai berikut untuk setiap pengaturan.

    | Pengaturan | Nilai |
    |---------|---------|
    | Jenis pemberitahuan | Pilih **Email/ Pesan SMS/Push/Voice** |
    | Nama | `VM was deleted` |

1. Pilih **Email**, dan di kotak **Email**, masukkan alamat email Anda, lalu pilih **OK**. 

    >**Catatan:** Anda akan menerima pemberitahuan email yang mengatakan bahwa Anda ditambahkan ke grup tindakan. Mungkin ada penundaan beberapa menit, tetapi itu adalah tanda pasti aturan telah disebarkan.

1. Setelah grup tindakan dibuat, pindahkan ke tab **Berikutnya: Detail** dan masukkan nilai berikut untuk setiap pengaturan.

    | Pengaturan | Nilai |
    |---------|---------|
    | Nama aturan pemberitahuan | `VM was deleted` |
    | Deskripsi aturan peringatan | `A VM in your resource group was deleted` |

1. Pilih **Tinjau + buat** untuk memvalidasi input Anda, lalu pilih **Buat**.

## Tugas 4: Memicu pemberitahuan dan mengonfirmasi bahwa pemberitahuan berfungsi

Dalam tugas ini, Anda memicu pemberitahuan dan mengonfirmasi pemberitahuan dikirim. 

>**Catatan:** Jika Anda menghapus komputer virtual sebelum aturan pemberitahuan disebarkan, aturan pemberitahuan mungkin tidak dipicu. 

1. Di portal, cari dan pilih **Komputer virtual**.

1. Centang kotak untuk **komputer virtual az104-vm0** .

1. Pilih **Hapus** dari bilah menu.

1. Centang kotak untuk **Terapkan penghapusan** paksa. Masukkan `delete` untuk mengonfirmasi lalu pilih **Hapus**. 

1. Di bilah judul, pilih **ikon Pemberitahuan** dan tunggu hingga **vm0** berhasil dihapus.

1. Anda akan menerima email pemberitahuan yang berbunyi, **Pemberitahuan penting: VM pemberitahuan Azure Monitor dihapus telah diaktifkan...** Jika tidak, buka program email Anda dan cari email dari azure-noreply@microsoft.com.

    ![Tangkap layar email peringatan Anda.](../media/az104-lab11-alert-email.png)
   
1. Pada menu sumber daya portal Azure, pilih **Pantau**, lalu pilih **Pemberitahuan** di menu di sebelah kiri.

1. Anda harus memiliki tiga pemberitahuan verbose yang dihasilkan dengan menghapus **vm0**.

   >**Catatan:** Diperlukan waktu beberapa menit agar email pemberitahuan dikirim dan agar pemberitahuan diperbarui di portal. Jika Anda tidak ingin menunggu, lanjutkan ke tugas berikutnya lalu kembali. 

1. Pilih nama salah satu peringatan (Misalnya, **VM telah dihapus**). Panel **Rincian peringatan** muncul yang menampilkan detail selengkapnya tentang acara tersebut.

## Tugas 5: Mengonfigurasi aturan pemrosesan pemberitahuan

Dalam tugas ini, Anda membuat aturan pemberitahuan untuk menekan pemberitahuan selama periode pemeliharaan. 

1. Lanjutkan di bilah **Pemberitahuan** , pilih **Aturan** pemrosesan pemberitahuan lalu **+ Buat**. 
   
1. Pilih grup** sumber daya Anda**, lalu pilih **Terapkan**.
   
1. Pilih **Berikutnya: Pengaturan aturan**, lalu pilih **Sembunyikan pemberitahuan**.
   
1. Pilih **Berikutnya: Penjadwalan**.
   
1. Secara default, aturan berfungsi sepanjang waktu, kecuali Anda menonaktifkannya atau mengonfigurasi jadwal. Anda akan menentukan aturan untuk menekan pemberitahuan selama pemeliharaan semalam.
Masukkan pengaturan ini untuk penjadwalan aturan pemrosesan pemberitahuan:

    | Pengaturan | Nilai |
    |---------|---------|
    | Menerapkan aturan | Pada waktu tertentu |
    | Mulai | Masukkan tanggal hari ini pukul 22.00. |
    | Akhir | Masukkan tanggal besok pukul 07.00. |
    | Zona waktu | Pilih zona waktu lokal. |

    ![Cuplikan layar bagian penjadwalan aturan pemrosesan pemberitahuan](../media/az104-lab11-alert-processing-rule-schedule.png)

1. Pilih **Berikutnya: Detail** dan masukkan pengaturan ini:

    | Pengaturan | Nilai |
    |---------|---------|
    | Grup sumber daya | **az104-rg11** |
    | Nama aturan | `Planned Maintenance` |
    | Deskripsi | `Suppress notifications during planned maintenance.` |

1. Pilih **Tinjau + buat** untuk memvalidasi input Anda, lalu pilih **Buat**.

## Tugas 6: Menggunakan kueri log Azure Monitor

Dalam tugas ini, Anda akan menggunakan Azure Monitor untuk mengkueri data yang diambil dari komputer virtual.

1. Di portal Azure, cari dan pilih `Monitor` bilah, klik **Log**.

1. Jika perlu tutup layar splash. 

1. Pilih cakupan, grup** sumber daya Anda**. Pilih **Terapkan**. 

1. Di tab **Kueri** , pilih **Komputer virtual** (panel kiri). 

1. Tinjau kueri yang tersedia. **Jalankan** (arahkan mouse ke atas kueri) **kueri Hitung heartbeats** .

1. Anda harus menerima jumlah heartbeat ketika komputer virtual berjalan.

1. Tinjau kueri. Kueri ini menggunakan *tabel heartbeat* . 

1. Ganti kueri dengan kueri ini, lalu klik **Jalankan**. Tinjau bagan yang dihasilkan. 

   ```
    InsightsMetrics
    | where TimeGenerated > ago(1h)
    | where Name == "UtilizationPercentage"
    | summarize avg(Val) by bin(TimeGenerated, 5m), Computer //split up by computer
    | render timechart
   ```

1. Saat Anda memiliki waktu, tinjau dan jalankan kueri lain. 

    >**Tahukah Anda?**: Jika Anda ingin berlatih dengan kueri lain, ada [Lingkungan](https://learn.microsoft.com/azure/azure-monitor/logs/log-analytics-tutorial#open-log-analytics) Demo Analitik Log.
    
    >**Tahukah Anda?**: Setelah menemukan kueri yang Anda suka, Anda bisa membuat pemberitahuan dari kueri tersebut. 

## Membersihkan sumber daya Anda

Jika Anda bekerja dengan **langganan** Anda sendiri membutuhkan waktu satu menit untuk menghapus sumber daya lab. Ini akan memastikan sumber daya dibebankan dan biaya diminimalkan. Cara term mudah untuk menghapus sumber daya lab adalah dengan menghapus grup sumber daya lab. 

+ Di portal Azure, pilih grup sumber daya, pilih **Hapus grup** sumber daya, **Masukkan nama** grup sumber daya, lalu klik **Hapus**.
+ Menggunakan Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Menggunakan CLI, `az group delete --name resourceGroupName`.

## Perluas pembelajaran Anda dengan Copilot
Copilot dapat membantu Anda mempelajari cara menggunakan alat pembuatan skrip Azure. Salinan juga dapat membantu di area yang tidak tercakup dalam lab atau di mana Anda memerlukan informasi lebih lanjut. Buka browser Edge dan pilih Copilot (kanan atas) atau navigasi ke *copilot.microsoft.com*. Luangkan beberapa menit untuk mencoba perintah ini.

+ Apa langkah-langkah konfigurasi dasar yang akan diperingatkan di Azure saat komputer virtual tidak berfungsi?
+ Bagaimana cara diberi tahu saat pemberitahuan Azure dipicu?
+ Buat kueri Azure Monitor untuk memberikan informasi performa CPU komputer virtual.

## Pelajari lebih lanjut dengan pelatihan mandiri

+ [Meningkatkan respons insiden dengan pemberitahuan di Azure](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/). Tanggapi insiden dan aktivitas di infrastruktur Anda melalui kapabilitas peringatan di Azure Monitor.
+ [Pantau komputer virtual Azure Anda dengan Azure Monitor](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/). Pantau Azure VM Anda dengan menggunakan Azure Monitor untuk mengumpulkan dan menganalisis metrik dan log host dan klien VM.

## Poin penting

Selamat atas penyelesaian lab. Berikut adalah takeaway utama untuk lab ini. 

+ Pemberitahuan membantu Anda mendeteksi dan mengatasi masalah sebelum pengguna melihat mungkin ada masalah dengan infrastruktur atau aplikasi Anda.
+ Anda dapat memperingatkan metrik atau sumber data log apa pun di platform data Azure Monitor.
+ Aturan pemberitahuan memantau data Anda dan menangkap sinyal yang menunjukkan ada sesuatu yang terjadi pada sumber daya yang ditentukan.
+ Pemberitahuan dipicu jika kondisi aturan pemberitahuan terpenuhi. Beberapa tindakan (email, SMS, push, suara) dapat dipicu.
+ Grup tindakan mencakup individu yang harus diberi tahu tentang pemberitahuan.
