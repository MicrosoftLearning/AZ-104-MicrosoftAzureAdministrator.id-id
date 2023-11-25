---
lab:
  title: 'Lab 11: Menerapkan Pemantauan'
  module: Administer Monitoring
---

# Lab 11 - Implementasi Pemantauan
# Panduan lab siswa

## Skenario laboratorium

Anda perlu mengevaluasi fungsionalitas Azure yang akan memberikan wawasan tentang kinerja dan konfigurasi sumber daya Azure, dengan fokus khususnya pada komputer virtual Azure. Untuk mencapai ini, Anda bermaksud untuk memeriksa kemampuan Azure Monitor, termasuk Analisis Log.

**Catatan:** Tersedia **[simulasi lab interaktif](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2017)** yang memungkinkan Anda mengklik lab ini sesuai keinginan Anda. Anda mungkin menemukan sedikit perbedaan antara simulasi interaktif dan lab yang dihosting, tetapi konsep dan ide utama yang ditunjukkan sama. 

## Tujuan

Di lab ini Anda akan:

+ Tugas 1: Memprovisikan lingkungan lab
+ Tugas 2: Mendaftarkan penyedia sumber daya Microsoft.Insights dan Microsoft.AlertsManagement
+ Tugas 3: Membuat dan mengonfigurasi ruang kerja Azure Log Analytics dan solusi berbasis Azure Automation
+ Tugas 4: Meninjau pengaturan pemantauan default komputer virtual Azure
+ Tugas 5: Mengonfigurasi pengaturan diagnostik komputer virtual Azure
+ Tugas 6: Meninjau fungsionalitas Azure Monitor
+ Tugas 7: Meninjau fungsionalitas Azure Log Analytics

## Perkiraan waktu: 45 menit

## Diagram arsitektur

![gambar](../media/lab11.png)

### Petunjuk

## Latihan 1

## Tugas 1: Memprovisikan lingkungan lab

Dalam tugas ini, Anda akan menggunakan komputer virtual yang akan digunakan untuk menguji skenario monitoring.

1. Masuk ke [portal Azure](https://portal.azure.com).

1. Di portal Microsoft Azure, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**.

    >**Catatan**: Jika ini adalah pertama kalinya Anda memulai **Cloud Shell** dan Anda disajikan dengan **pesan Anda tidak memiliki penyimpanan yang dipasang** , pilih langganan yang Anda gunakan di lab ini, dan klik **Buat penyimpanan**.

1. Di bilah alat panel Cloud Shell, klik ikon **Unggah/Unduh file**, di menu menurun, klik **Unggah** dan unggah file **\\Allfiles\\Labs\\11\\az104-11-vm-template.json** dan **\\Allfiles\\Labs\\11\\az104-11-vm-parameters.json** ke direktori beranda Cloud Shell.

1. Dari panel Cloud Shell, jalankan elemen berikut ini untuk membuat grup sumber daya yang akan menghosting komputer virtual (ganti `[Azure_region]` tempat penampung dengan nama wilayah Azure tempat Anda ingin menerapkan komputer virtual Azure):

    >**Catatan**: Pastikan untuk memilih salah satu wilayah yang tercantum sebagai **Wilayah** Ruang Kerja Analitik Log dalam dokumentasi pemetaan Ruang Kerja yang direferensikan [](https://docs.microsoft.com/en-us/azure/automation/how-to/region-mappings)

   ```powershell
   $location = '[Azure_region]'

   $rgName = 'az104-11-rg0'

   New-AzResourceGroup -Name $rgName -Location $location
   ```

1. Dari panel Cloud Shell, jalankan perintah berikut untuk membuat jaringan virtual pertama dan menyebarkan mesin virtual ke dalamnya menggunakan file templat dan parameter yang Anda unggah:

    >**Catatan**: Anda akan diminta untuk memberikan kata sandi Admin.
    
   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-11-vm-template.json `
      -TemplateParameterFile $HOME/az104-11-vm-parameters.json `
      -AsJob
   ```

    >**Catatan**: Jangan tunggu penyebaran selesai tetapi lanjutkan ke tugas berikutnya. Penyebaran akan memakan waktu sekitar 3 menit.

## Tugas 2: Daftarkan penyedia sumber daya Microsoft.Insights dan Microsoft.AlertsManagement.

1. Dari panel Cloud Shell, jalankan perintah berikut untuk mendaftarkan penyedia sumber daya Microsoft.Insights dan Microsoft.AlertsManagement.

   ```powershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.Insights

   Register-AzResourceProvider -ProviderNamespace Microsoft.AlertsManagement
   ```

1. Kecilkan panel Cloud Shell (tetapi jangan tutup).

## Tugas 3: Membuat dan mengonfigurasi ruang kerja Azure Log Analytics dan solusi berbasis Azure Automation

Dalam tugas ini, Anda akan membuat dan mengonfigurasi ruang kerja Azure Log Analytics dan solusi berbasis Azure Automation

1. Di portal Azure, telusuri dan pilih **ruang kerja Log Analytics** dan, pada panel **ruang kerja Log Analytics**, klik **+ Buat**.

1. Pada tab **Dasar** bilah **Buat ruang kerja Analytics Log**, masukkan pengaturan berikut, klik **Tinjau + Buat**, lalu klik **Buat**:

    | Pengaturan | Value |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | nama grup sumber daya baru **az104-11-rg1** |
    | Ruang Kerja Analitik Log | nama unik apa pun |
    | Wilayah | nama wilayah Azure tempat Anda menggunakan komputer virtual di tugas sebelumnya |

    >**Catatan**: Pastikan Anda menentukan wilayah yang sama tempat Anda menyebarkan komputer virtual di tugas sebelumnya.

    >**Catatan**: Tunggu hingga penyebaran selesai. Penyebaran akan memakan waktu sekitar 1 menit.

1. Di portal Azure, telusuri dan pilih **Akun Automasi**, dan pada panel **Akun Automasi**, klik **+ Buat**.

1. Pada bilah **Buat Akun Azure Automation**, tentukan pengaturan berikut, dan klik **Tinjau + Buat** setelah validasi, klik **Buat**:

    | Pengaturan | Value |
    | --- | --- |
    | Nama akun Automation | nama unik apa pun |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | **az104-11-rg1** |
    | Wilayah | nama wilayah Azure yang ditentukan berdasarkan [Dokumentasi pemetaan ruang kerja](https://docs.microsoft.com/en-us/azure/automation/how-to/region-mappings) |

    >**Catatan**: Pastikan Anda menentukan wilayah Azure berdasarkan [dokumentasi pemetaan Ruang Kerja](https://docs.microsoft.com/en-us/azure/automation/how-to/region-mappings)

    >**Catatan**: Tunggu hingga penyebaran selesai. Penyebaran mungkin memakan waktu sekitar 3 menit.

1. Klik **Buka sumber daya**.

1. Pada bilah akun Azure Automation, di bagian **Manajemen Konfigurasi**, klik **Inventaris**.

1. Di panel **Inventaris**, di daftar menurun **ruang kerja Analitik Log**, pilih ruang kerja Analitik Log yang Anda buat sebelumnya dalam tugas ini dan klik **Aktifkan**.

    >**Catatan**: Tunggu hingga penginstalan solusi Analitik Log yang sesuai selesai. Ini mungkin memakan waktu sekitar 3 menit.

    >**Catatan**: Ini secara otomatis menginstal **solusi Pelacakan** perubahan juga.

1. Pada bilah akun Azure Automation, di bagian **Pengelolaan Pembaruan**, klik **Perbarui pengelolaan** dan klik **Aktifkan**.

    >**Catatan**: Tunggu hingga penginstalan selesai. Proses ini mungkin perlu waktu sekitar 5 menit.

## Tugas 4: Meninjau pengaturan pemantauan default komputer virtual Azure

Dalam tugas ini, Anda akan meninjau pengaturan pemantauan default komputer virtual Azure

1. Di portal Azure, cari dan pilih **Komputer virtual**, dan pada panel **Komputer virtual**, klik **az104-11-vm0**.

1. Pada bilah **az104-11-vm0**, di bagian **Pemantauan**, klik **Metrik**.

1. Pada bilah **az104-11-vm0 \| Metrik**, pada bagan default, perhatikan bahwa satu-satunya **Ruang Nama Metrik** yang tersedia adalah **Host Komputer Virtual**.

    >**Catatan**: Ini diharapkan, karena belum ada pengaturan diagnostik tingkat tamu yang dikonfigurasi. Namun, Anda memiliki opsi untuk mengaktifkan metrik memori tamu langsung dari daftar menurun **Namespace Metrik**. Anda akan mengaktifkannya nanti dalam latihan ini.

1. Di daftar menurun **Metrik**, tinjau daftar metrik yang tersedia.

    >**Catatan**: Daftar ini mencakup berbagai metrik terkait CPU, disk, dan jaringan yang dapat dikumpulkan dari host komputer virtual, tanpa memiliki akses ke metrik tingkat tamu.

1. Di daftar menurun **Metrik**, pilih **Persentase CPU**, di daftar menurun **Agregasi**, pilih **Rata-rata**, dan tinjau grafik yang dihasilkan.

## Tugas 5: Mengonfigurasi pengaturan diagnostik komputer virtual Azure

Dalam tugas ini, Anda akan mengonfigurasi pengaturan diagnostik komputer virtual Azure.

1. Pada bilah **az104-11-vm0**, di bagian **Pemantauan**, klik **Pengaturan diagnostik**.

1. Pada tab ****Gambaran Umum** dari bilah pengaturan** Diagnostik az104-11-vm0\|, pilih **akun** penyimpanan Diagnostik, lalu klik **Aktifkan pemantauan** tingkat tamu.

    >**Catatan**: Tunggu hingga ekstensi pengaturan diagnostik diinstal. Ini mungkin memakan waktu sekitar 3 menit.

1. Beralih ke tab **Penghitung kinerja** bilah **az104-11-vm0 \| Pengaturan diagnostik** dan tinjau penghitung yang tersedia.

    >**Catatan**: Secara default, penghitung CPU, memori, disk, dan jaringan diaktifkan. Anda dapat beralih ke tampilan **Kustom** untuk daftar yang lebih rinci.

1. Beralih ke tab **Log** pada bilah **az104-11-vm0 \| Pengaturan diagnostik** dan tinjau opsi pengumpulan log peristiwa yang tersedia.

    >**Catatan**: Secara default, pengumpulan log mencakup entri penting, kesalahan, dan peringatan dari log Log Aplikasi dan Sistem, serta entri kegagalan Audit dari log Keamanan. Di sini Anda juga dapat beralih ke tampilan **Kustom** untuk pengaturan konfigurasi yang lebih detail.

1. Pada bilah **az104-11-vm0**, di bagian **Monitoring**, klik **Log** lalu klik **Aktifkan**.

1. Pada bilah **az104-11-vm0 - Logs** , perhatikan **agen** Azure Monitor akan diinstal, lalu klik **Konfigurasikan**.  

    >**Catatan**: Jangan tunggu hingga operasi selesai, tetapi lanjutkan ke langkah berikutnya. Operasi mungkin memakan waktu sekitar 5 menit.

1. Pada bilah **az104-11-vm0 \| Log**, di bagian **Pemantauan**, klik **Metrik**.

1. Pada bilah **az104-11-vm0 \| Metrik**, pada bagan default, perhatikan bahwa pada titik ini, daftar menurun **Ruang Nama Metrik**, selain Entri **Host Komputer Virtual** juga menyertakan entri **Tamu (klasik)**.

    >**Catatan**: Ini diharapkan, karena Anda mengaktifkan pengaturan diagnostik tingkat tamu. Anda juga memiliki opsi untuk **Mengaktifkan metrik memori tamu baru**.

1. Dalam daftar menurun **Namespace Metrik**, pilih entri **Tamu (klasik)**.

1. Di daftar menurun **Metrik**, tinjau daftar metrik yang tersedia.

    >**Catatan**: Daftar ini menyertakan metrik tingkat tamu tambahan yang tidak tersedia saat hanya mengandalkan pemantauan tingkat host.

1. Di daftar menurun **Metrik**, pilih **Memori\\ Bytes yang Tersedia**, di daftar menurun **Agregasi**, pilih **Maks**, dan tinjau bagan yang dihasilkan.

## Tugas 6: Meninjau fungsionalitas Azure Monitor

1. Di portal Azure, telusuri dan pilih **Monitor** dan, pada panel **Monitor \| Tinjau**, klik **Metrics**.

1. Pada panel **Pilih cakupan**, pada tab **Telusuri**, navigasikan ke grup sumber daya **az104-11-rg0**, perluas, centang kotak di sebelah **az104-11-vm0** entri komputer virtual dalam grup sumber daya tersebut, dan klik **Terapkan**.

    >**Catatan**: Ini memberi Anda tampilan dan opsi yang sama dengan yang tersedia dari **bilah az104-11-vm0 - Metrik** .

1. Di daftar menurun **Metrik**, pilih **Persentase CPU**, di daftar menurun **Agregasi**, pilih **Rata-rata**, dan tinjau grafik yang dihasilkan.

1. Pada bilah **Pantau \| Metrik**, pada panel **CPU Persentase Rata-rata untuk az104-11-vm0**, klik **Aturan peringatan baru**.

    >**Catatan**: Membuat aturan pemberitahuan dari Metrik tidak didukung untuk metrik dari namespace metrik Tamu (klasik). Ini dapat dilakukan dengan menggunakan templat Azure Resource Manager, seperti yang dijelaskan dalam dokumen [Kirim metrik OS Tamu ke penyimpanan metrik Azure Monitor menggunakan templat Resource Manager untuk komputer virtual Windows](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/collect-custom-metrics-guestos-resource-manager-vm)

1. Pada bilah **Buat aturan peringatan**, di bagian **Kondisi**, klik entri ketentuan yang ada.

1. Pada bilah **Konfigurasi logika sinyal**, dalam daftar sinyal, di bagian **Logika peringatan**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya) dan klik **Selesai**:

    | Pengaturan | Value |
    | --- | --- |
    | Ambang | **Statik** |
    | Jenis agregasi | **Tengah** |
    | Operator | **Lebih besar dari** |
    | Ambang nilai | **2** |
    | Periksa setiap | **1 menit** |
    | Periode lookback| **1 Menit** |

1. Klik **Berikutnya: Tindakan >**, pada bilah **Buat aturan** pemberitahuan, di bagian **Grup tindakan** , klik tombol **+ Buat grup** tindakan.

1. Pada tab **Dasar dari bilah **Buat grup** tindakan, tentukan pengaturan berikut (biarkan orang lain dengan nilai defaultnya) dan pilih **Berikutnya: Pemberitahuan >****:

    | Pengaturan | Value |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | **az104-11-rg1** |
    | Nama grup tindakan | **az104-11-ag1** |
    | Nama tampilan | **az104-11-ag1** |

1. Pada tab **Pemberitahuan** panel **Buat grup tindakan**, dalam daftar menurun **Jenis pemberitahuan**, pilih **Pesan email/SMS/Push/Suara**. Di kotak teks **Nama**, ketik **email admin**. Klik ikon **Edit detail** (pensil).

1. Pada bilah **Email/PESAN SMS/Push/Voice**, pilih **kotak centang Email**, ketik alamat email Anda di **kotak teks Email**, biarkan orang lain dengan nilai defaultnya, klik **OK**, kembali pada tab Pemberitahuan** dari ****bilah Buat grup** tindakan, pilih **Berikutnya: Tindakan >**.

1. Pada tab **Tindakan** panel **Buat grup tindakan**, tinjau item yang tersedia di daftar menurun **Jenis tindakan** tanpa membuat perubahan apa pun dan pilih **Tinjau + buat**.

1. Pada tab **Tinjau + buat** panel **Buat grup tindakan**, pilih **Buat**.

1. Kembali ke bilah **Buat aturan** pemberitahuan, klik **Berikutnya: Detail >**, dan di bagian **Detail** aturan pemberitahuan, tentukan pengaturan berikut (biarkan orang lain dengan nilai defaultnya):

    | Pengaturan | Value |
    | --- | --- |
    | Nama aturan pemberitahuan | **Persentase CPU di atas ambang pengujian** |
    | Deskripsi aturan peringatan | **Persentase CPU di atas ambang pengujian** |
    | Tingkat keparahan | **Sev 3** |
    | Aktifkan saat pembuatan | **Ya** |

1. Klik **Tinjau + buat** dan pada tab **Tinjau + buat** klik **Buat**.

    >**Catatan**: Diperlukan waktu hingga 10 menit agar aturan pemberitahuan metrik aktif.

1. Di portal Azure, cari dan pilih **Komputer virtual**, dan pada panel **Komputer virtual**, klik **az104-11-vm0**.

1. Pada bilah **az104-11-vm0**, klik **Hubungkan**, di menu menurun, klik **RDP**, di bilah **Hubungkan dengan RDP**, klik **Unduh File RDP** dan ikuti petunjuk untuk memulai sesi Desktop Jarak Jauh.

    >**Catatan**: Langkah ini mengacu pada menyambungkan melalui Desktop Jauh dari komputer Windows. Di Mac, Anda dapat menggunakan Klien Desktop Jauh dari Mac App Store dan di komputer Linux Anda dapat menggunakan perangkat lunak klien RDP sumber terbuka.

    >**Catatan**: Anda dapat mengabaikan perintah peringatan saat menyambungkan ke komputer virtual target.

1. Saat diminta, masuk menggunakan nama pengguna **Siswa** dan sandi dari file parameter.

1. Dalam sesi Desktop Jauh, klik **Mulai**, luaskan folder **Sistem Windows**, dan klik **Prompt Perintah**.

1. Dari Prompt Perintah, jalankan perintah berikut untuk memicu peningkatan penggunaan CPU pada **az104-11-vm0** Azure VM:

   ```sh
   for /l %a in (0,0,1) do echo a
   ```

    >**Catatan**: Ini akan memulai perulangan tak terbatas yang harus meningkatkan pemanfaatan CPU di atas ambang batas aturan pemberitahuan yang baru dibuat.

1. Biarkan sesi Desktop Jarak Jauh terbuka dan beralih kembali ke jendela browser yang menampilkan portal Azure di komputer lab Anda.

1. Di portal Azure, navigasikan kembali ke bilah **Monitor** dan klik **Peringatan**.

1. Catat jumlah peringatan **Sev 3** lalu klik baris **Sev 3**.

    >**Catatan**: Anda mungkin perlu menunggu beberapa menit dan klik **Refresh**.

1. Pada bilah **Semua Peringatan**, tinjau peringatan yang dihasilkan.

## Tugas 7: Meninjau fungsionalitas Azure Log Analytics

1. Di portal Azure, navigasikan kembali ke bilah **Monitor**, klik **Log**.

    >**Catatan**: Anda mungkin perlu mengklik **Mulai** jika ini pertama kalinya Anda mengakses Analitik Log.

1. Jika perlu, klik **Pilih cakupan**, pada panel **Pilih cakupan**, pilih tab **Terbaru**, pilih **az104-11-vm0**, dan klik **Terapkan**.

1. Di jendela kueri, tempel kueri berikut, klik **Jalankan**, dan tinjau bagan yang dihasilkan:

   ```sh
   // Virtual Machine available memory
   // Chart the VM's available memory over the last hour.
   InsightsMetrics
   | where TimeGenerated > ago(1h)
   | where Name == "AvailableMB"
   | project TimeGenerated, Name, Val
   | render timechart
   ```

    > **Catatan**: Kueri seharusnya tidak memiliki kesalahan (ditunjukkan oleh blok merah di bilah gulir kanan). Jika kueri tidak akan menempel tanpa kesalahan langsung dari instruksi, tempel kode kueri ke editor teks seperti Notepad, lalu salin dan tempel ke jendela kueri dari sana.


1. Klik **Permintaan** di bilah alat, pada panel **Permintaan**, cari ubin **Lacak ketersediaan VM** dan klik dua kali untuk mengisi jendela kueri, klik tombol perintah **Jalankan** di ubin, dan tinjau hasilnya.

1. Pada tab **Kueri Baru 1**, pilih header **Tabel**, dan tinjau daftar tabel di bagian **Komputer virtual**.

    >**Catatan**: Nama beberapa tabel sesuai dengan solusi yang Anda instal sebelumnya di lab ini.

1. Arahkan mouse ke entri **VMComputer** dan klik ikon **Lihat Pratinjau data**.

1. Jika ada data yang tersedia, di panel **Perbarui**, klik **Gunakan di editor**.

    >**Catatan**: Anda mungkin perlu menunggu beberapa menit sebelum data pembaruan tersedia.

## Membersihkan sumber daya

>**Catatan**: Ingatlah untuk menghapus sumber daya Azure yang baru dibuat yang tidak lagi Anda gunakan. Dengan menghapus sumber daya yang tidak digunakan, Anda tidak akan melihat biaya yang tak terduga.

>**Catatan**: Jangan khawatir jika sumber daya lab tidak dapat segera dihapus. Terkadang sumber daya memiliki dependensi dan membutuhkan waktu lebih lama untuk dihapus. Ini adalah tugas Administrator yang umum untuk memantau penggunaan sumber daya, jadi tinjau sumber daya Anda secara berkala di Portal untuk melihat bagaimana pembersihannya. 

1. Di portal Azure, buka sesi **PowerShell** dalam panel **Cloud Shell**.

1. Buat daftar semua grup sumber daya yang dibuat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-11*'
   ```

1. Hapus semua grup sumber daya yang Anda buat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-11*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Catatan**: Perintah dijalankan secara asinkron (seperti yang ditentukan oleh parameter -AsJob), jadi sementara Anda akan dapat menjalankan perintah PowerShell lain segera setelah itu dalam sesi PowerShell yang sama, akan memakan waktu beberapa menit sebelum grup sumber daya benar-benar dihapus.

## Tinjau

Di lab ini, Anda telah:

+ Memprovisikan lingkungan lab
+ Membuat dan mengonfigurasi ruang kerja Azure Log Analytics dan solusi berbasis Azure Automation
+ Meninjau pengaturan pemantauan default komputer virtual Azure
+ Pengaturan diagnostik komputer virtual Azure yang dikonfigurasi
+ Meninjau fungsionalitas Azure Monitor
+ Meninjau fungsionalitas Azure Log Analytics
