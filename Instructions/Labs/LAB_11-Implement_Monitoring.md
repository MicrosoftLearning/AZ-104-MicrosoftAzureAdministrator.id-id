---
lab:
  title: 11 - Implementasi Pemantauan
  module: Module 11 - Monitoring
ms.openlocfilehash: 10c3fe049aaf037892a34299c21dfd8213ce40b2
ms.sourcegitcommit: be14e4ff5bc638e8aee13ec4b8be29525d404028
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 05/11/2022
ms.locfileid: "145198182"
---
# <a name="lab-11---implement-monitoring"></a>Lab 11 - Implementasi Pemantauan
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Anda perlu mengevaluasi fungsionalitas Azure yang akan memberikan wawasan tentang kinerja dan konfigurasi sumber daya Azure, dengan fokus khususnya pada mesin virtual Azure. Untuk mencapai ini, Anda bermaksud untuk memeriksa kemampuan Azure Monitor, termasuk Analisis Log.

## <a name="objectives"></a>Tujuan

Di lab ini Anda akan:

+ Tugas 1: Memprovisikan lingkungan lab
+ Tugas 2: Mendaftarkan penyedia sumber daya Microsoft.Insights dan Microsoft.AlertsManagement
+ Tugas 3: Membuat dan mengonfigurasi ruang kerja Azure Log Analytics dan solusi berbasis Azure Automation
+ Tugas 4: Meninjau pengaturan pemantauan default mesin virtual Azure
+ Tugas 5: Mengonfigurasi pengaturan diagnostik mesin virtual Azure
+ Tugas 6: Meninjau fungsionalitas Azure Monitor
+ Tugas 7: Meninjau fungsionalitas Azure Log Analytics

## <a name="estimated-timing-45-minutes"></a>Perkiraan waktu: 45 menit

## <a name="instructions"></a>Instruksi

### <a name="exercise-1"></a>Latihan 1

#### <a name="task-1-provision-the-lab-environment"></a>Tugas 1: Memprovisikan lingkungan lab

Dalam tugas ini, Anda akan menggunakan mesin virtual yang akan digunakan untuk menguji skenario pemantauan.

1. Masuk ke [portal Microsoft Azure](https://portal.azure.com).

1. Di portal Microsoft Azure, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Microsoft Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**.

    >**Catatan**: Jika ini pertama kalinya Anda memulai **Cloud Shell** dan Anda melihat pesan **Anda tidak memiliki penyimpanan yang terinstal**, pilih langganan yang Anda gunakan di lab ini, dan klik **Buat penyimpanan**.

1. Di bilah alat panel Cloud Shell, klik ikon **Unggah/Unduh file**, di menu menurun, klik **Unggah** dan unggah file **\\Allfiles\\Labs\\11\\az104-11-vm-template.json** dan **\\Allfiles\\Labs\\11\\az104-11-vm-parameters.json** ke direktori beranda Cloud Shell.

1. Edit file Parameter yang baru saja Anda unggah dan ubah kata sandinya. Jika memerlukan bantuan untuk mengedit file di Shell, mintalah bantuan instruktur Anda. Sebagai praktik terbaik, rahasia, seperti kata sandi, harus disimpan dengan lebih aman di Key Vault. 

1. Dari panel Cloud Shell, jalankan elemen berikut ini untuk membuat grup sumber daya yang akan menghosting mesin virtual (ganti `[Azure_region]` tempat penampung dengan nama wilayah Azure tempat Anda ingin menerapkan mesin virtual Azure):

    >**Catatan**: Pastikan untuk memilih salah satu wilayah yang terdaftar sebagai **should be translated** dalam referensi di [dokumentasi pemetaan Ruang Kerja](https://docs.microsoft.com/en-us/azure/automation/how-to/region-mappings)

   ```powershell
   $location = '[Azure_region]'

   $rgName = 'az104-11-rg0'

   New-AzResourceGroup -Name $rgName -Location $location
   ```

1. Dari panel Cloud Shell, jalankan perintah berikut untuk membuat jaringan virtual pertama dan menerapkan mesin virtual ke dalamnya dengan menggunakan file templat dan parameter yang Anda unggah:

   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-11-vm-template.json `
      -TemplateParameterFile $HOME/az104-11-vm-parameters.json `
      -AsJob
   ```

    >**Catatan**: Jangan menunggu penerapan selesai tetapi lanjutkan ke tugas berikutnya. Penyebaran akan memakan waktu sekitar 3 menit.

#### <a name="task-2-register-the-microsoftinsights-and-microsoftalertsmanagement-resource-providers"></a>Tugas 2: Daftarkan penyedia sumber daya Microsoft.Insights dan Microsoft.AlertsManagement.

1. Dari panel Cloud Shell, jalankan perintah berikut untuk mendaftarkan penyedia sumber daya Microsoft.Insights dan Microsoft.AlertsManagement.

   ```powershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.Insights

   Register-AzResourceProvider -ProviderNamespace Microsoft.AlertsManagement
   ```

1. Kecilkan panel Cloud Shell (tetapi jangan tutup).

#### <a name="task-3-create-and-configure-an-azure-log-analytics-workspace-and-azure-automation-based-solutions"></a>Tugas 3: Membuat dan mengonfigurasi ruang kerja Azure Log Analytics dan solusi berbasis Azure Automation

Dalam tugas ini, Anda akan membuat dan mengonfigurasi ruang kerja Azure Log Analytics dan solusi berbasis Azure Automation

1. Di portal Azure, telusuri dan pilih **ruang kerja Log Analytics** dan, pada panel **ruang kerja Log Analytics**, klik **+ Buat**.

1. Pada tab **Dasar** bilah **Buat ruang kerja Analytics Log**, masukkan pengaturan berikut, klik **Tinjauan + Buat**, lalu klik **Buat**:

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | nama grup sumber daya baru **az104-11-rg1** |
    | Ruang Kerja Analitik Log | nama unik apa pun |
    | Wilayah | nama wilayah Azure tempat Anda menggunakan mesin virtual di tugas sebelumnya |

    >**Catatan**: Pastikan Anda menentukan wilayah yang sama tempat Anda menerapkan mesin virtual di tugas sebelumnya.

    >**Catatan**: Tunggu hingga penyebaran selesai. Penyebaran akan memakan waktu sekitar 1 menit.

1. Di portal Azure, telusuri dan pilih **Akun Automasi**, dan pada panel **Akun Automasi**, klik **+ Buat**.

1. Pada bilah **Buat Akun Azure Automation**, tentukan pengaturan berikut, dan klik **Tinjauan + Buat** setelah validasi, klik **Buat**:

    | Pengaturan | Nilai |
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

    >**Catatan**: Tunggu hingga penginstalan solusi Analitik Log yang sesuai selesai. Penginstalan mungkin memakan waktu sekitar 3 menit.

    >**Catatan**: Ini juga secara otomatis menginstal solusi **Ubah pelacakan**.

1. Pada bilah akun Azure Automation, di bagian **Pengelolaan Pembaruan**, klik **Perbarui pengelolaan** dan klik **Aktifkan**.

    >**Catatan**: Tunggu hingga penginstalan selesai. Proses ini mungkin perlu waktu sekitar 5 menit.

#### <a name="task-4-review-default-monitoring-settings-of-azure-virtual-machines"></a>Tugas 4: Meninjau pengaturan pemantauan default mesin virtual Azure

Dalam tugas ini, Anda akan meninjau pengaturan pemantauan default mesin virtual Azure

1. Di portal Azure, cari dan pilih **Komputer virtual**, dan pada panel **Komputer virtual**, klik **az104-11-vm0**.

1. Pada bilah **az104-11-vm0**, di bagian **Pemantauan**, klik **Metrik**.

1. Pada bilah **az104-11-vm0 \| Metrik**, pada bagan default, perhatikan bahwa satu-satunya **Ruang Nama Metrik** yang tersedia adalah **Host Komputer Virtual**.

    >**Catatan**: Hal ini diharapkan, karena belum ada pengaturan diagnostik tingkat tamu yang dikonfigurasi. Namun, Anda memiliki opsi untuk mengaktifkan metrik memori tamu langsung dari daftar menurun **Namespace Metrik**. Anda akan mengaktifkannya nanti dalam latihan ini.

1. Di daftar menurun **Metrik**, tinjau daftar metrik yang tersedia.

    >**Catatan**: Daftar tersebut mencakup berbagai metrik terkait CPU, disk, dan jaringan yang dapat dikumpulkan dari host mesin virtual, tanpa memiliki akses ke metrik tingkat tamu.

1. Di daftar menurun **Metrik**, pilih **Persentase CPU**, di daftar menurun **Agregasi**, pilih **Rata-rata**, dan tinjau grafik yang dihasilkan.

#### <a name="task-5-configure-azure-virtual-machine-diagnostic-settings"></a>Tugas 5: Mengonfigurasi pengaturan diagnostik mesin virtual Azure

Dalam tugas ini, Anda akan mengonfigurasi pengaturan diagnostik mesin virtual Azure.

1. Pada bilah **az104-11-vm0**, di bagian **Pemantauan**, klik **Pengaturan diagnostik**.

1. Pada tab **Ringkasan** bilah **az104-11-vm0 \| Pengaturan diagnostik**, klik **Aktifkan pemantauan tingkat tamu**.

    >**Catatan**: Tunggu hingga operasi berlaku. Penginstalan mungkin memakan waktu sekitar 3 menit.

1. Beralih ke tab **Penghitung kinerja** bilah **az104-11-vm0 \| Pengaturan diagnostik** dan tinjau penghitung yang tersedia.

    >**Catatan**: Secara default, penghitung CPU, memori, disk, dan jaringan diaktifkan. Anda dapat beralih ke tampilan **Kustom** untuk daftar yang lebih rinci.

1. Beralih ke tab **Log** pada bilah **az104-11-vm0 \| Pengaturan diagnostik** dan tinjau opsi pengumpulan log peristiwa yang tersedia.

    >**Catatan**: Secara default, pengumpulan log mencakup entri kritis, kesalahan, dan peringatan dari Log Aplikasi dan log Sistem, serta entri kegagalan Audit dari log Keamanan. Di sini Anda juga dapat beralih ke tampilan **Kustom** untuk pengaturan konfigurasi yang lebih detail.

1. Pada bilah **az104-11-vm0**, di bagian **Pemantauan**, klik **Log** lalu klik **Aktifkan**.

1. Pada panel **az104-11-vm0 - Log**, pastikan bahwa ruang kerja Analisis Log yang Anda buat sebelumnya di lab ini dipilih dalam daftar menurun **Pilih Ruang Kerja Analisis Log** dan klik **Aktifkan**.

    >**Catatan**: Jangan menunggu operasi selesai, tetapi lanjutkan ke langkah berikutnya. Operasi mungkin memakan waktu sekitar 5 menit.

1. Pada bilah **az104-11-vm0 \| Log**, di bagian **Pemantauan**, klik **Metrik**.

1. Pada bilah **az104-11-vm0 \| Metrik**, pada bagan default, perhatikan bahwa pada titik ini, daftar menurun **Ruang Nama Metrik**, selain Entri **Host Komputer Virtual** juga menyertakan entri **Tamu (klasik)** .

    >**Catatan**: Ini diharapkan, karena Anda mengaktifkan pengaturan diagnostik tingkat tamu. Anda juga memiliki opsi untuk **Mengaktifkan metrik memori tamu baru**.

1. Dalam daftar menurun **Namespace Metrik**, pilih entri **Tamu (klasik)** .

1. Di daftar menurun **Metrik**, tinjau daftar metrik yang tersedia.

    >**Catatan**: Daftar tersebut mencakup metrik tingkat tamu tambahan yang tidak tersedia jika hanya mengandalkan pemantauan tingkat host.

1. Di daftar menurun **Metrik**, pilih **Memori\\ Byte yang Tersedia**, di daftar menurun **Agregasi**, pilih **Maks**, dan tinjau bagan yang dihasilkan.

#### <a name="task-6-review-azure-monitor-functionality"></a>Tugas 6: Meninjau fungsionalitas Azure Monitor

1. Di portal Azure, telusuri dan pilih **Monitor** dan, pada panel **Monitor \| Tinjau**, klik **Metrik**.

1. Pada panel **Pilih cakupan**, pada tab **Telusuri**, navigasikan ke grup sumber daya **az104-11-rg0**, perluas, centang kotak di sebelah **az104-11-vm0** entri mesin virtual dalam grup sumber daya tersebut, dan klik **Terapkan**.

    >**Catatan**: Ini memberi Anda tampilan dan opsi yang sama seperti yang tersedia dari bilah **az104-11-vm0 - Metrik**.

1. Di daftar menurun **Metrik**, pilih **Persentase CPU**, di daftar menurun **Agregasi**, pilih **Rata-rata**, dan tinjau grafik yang dihasilkan.

1. Pada bilah **Pantau \| Metrik**, pada panel **CPU Persentase Rata-rata untuk az104-11-vm0**, klik **Aturan peringatan baru**.

    >**Catatan**: Membuat aturan peringatan dari Metrik tidak didukung untuk metrik dari ruang nama metrik Tamu (klasik). Ini dapat dilakukan dengan menggunakan templat Azure Resource Manager, seperti yang dijelaskan dalam dokumen [Kirim metrik OS Tamu ke penyimpanan metrik Azure Monitor menggunakan templat Resource Manager untuk mesin virtual Windows](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/collect-custom-metrics-guestos-resource-manager-vm)

1. Pada bilah **Buat aturan peringatan**, di bagian **Kondisi**, klik entri ketentuan yang ada.

1. Pada bilah **Konfigurasi logika sinyal**, dalam daftar sinyal, di bagian **Logika peringatan**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya) dan klik **Selesai**:

    | Pengaturan | Nilai |
    | --- | --- |
    | Ambang | **Statis** |
    | Operator | **Lebih besar dari** |
    | Jenis agregasi | **Rata-rata** |
    | Ambang nilai | **2** |
    | Granularitas agregasi (Periode) | **1 menit** |
    | Frekuensi evaluasi | **Setiap 1 Menit** |

1. Klik **Berikutnya: Tindakan >** , pada bilah **Buat aturan peringatan**, di bagian **Grup tindakan**, klik tombol **+ Buat grup tindakan**.

1. Pada tab **Dasar** panel **Buat grup tindakan**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya) dan pilih **Berikutnya: Pemberitahuan >** :

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | **az104-11-rg1** |
    | Nama grup tindakan | **az104-11-ag1** |
    | Nama tampilan | **az104-11-ag1** |

1. Pada tab **Pemberitahuan** panel **Buat grup tindakan**, dalam daftar menurun **Jenis pemberitahuan**, pilih **Pesan email/SMS/Push/Suara**. Di kotak teks **Nama**, ketik **email admin**. Klik ikon **Edit detail** (pensil).

1. Pada panel **Email/pesan SMS/Push/Suara**, pilih kotak centang **Email**, ketik alamat email Anda di kotak teks **Email**, biarkan yang lain dengan nilai defaultnya, klik **OK**, kembali ke tab **Notifikasi** pada panel **Buat grup tindakan**, pilih **Berikutnya: Tindakan >** .

1. Pada tab **Tindakan** panel **Buat grup tindakan**, tinjau item yang tersedia di daftar menurun **Jenis tindakan** tanpa membuat perubahan apa pun dan pilih **Tinjauan + buat**.

1. Pada tab **Tinjau + buat** panel **Buat grup tindakan**, pilih **Buat**.

1. Kembali ke bilah **Buat aturan peringatan**, klik **Berikutnya: Details >** , dan di bagian **Detail aturan peringatan**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama aturan pemberitahuan | **Persentase CPU di atas ambang pengujian** |
    | Deskripsi aturan peringatan | **Persentase CPU di atas ambang pengujian** |
    | Tingkat keparahan | **Sev 3** |
    | Aktifkan saat pembuatan | **Ya** |

1. Klik **Tinjauan + buat** dan pada tab **Tinjauan + buat** klik **Buat**.

    >**Catatan**: Dibutuhkan waktu hingga 10 menit agar aturan pemberitahuan metrik menjadi aktif.

1. Di portal Azure, cari dan pilih **Komputer virtual**, dan pada panel **Komputer virtual**, klik **az104-11-vm0**.

1. Pada bilah **az104-11-vm0**, klik **Hubungkan**, di menu menurun, klik **RDP**, di bilah **Hubungkan dengan RDP**, klik **Unduh File RDP** dan ikuti petunjuk untuk memulai sesi Desktop Jarak Jauh.

    >**Catatan**: Langkah ini mengacu pada menghubungkan melalui Desktop Jauh dari komputer Windows. Di Mac, Anda dapat menggunakan Klien Desktop Jauh dari Mac App Store dan di komputer Linux Anda dapat menggunakan perangkat lunak klien RDP sumber terbuka.

    >**Catatan**: Anda dapat mengabaikan permintaan peringatan apa pun saat menghubungkan ke mesin virtual target.

1. Saat diminta, masuk dengan menggunakan nama pengguna **Siswa** dan sandi dari file parameter.

1. Dalam sesi Desktop Jauh, klik **Mulai**, luaskan folder **Sistem Windows**, dan klik **Prompt Perintah**.

1. Dari Prompt Perintah, jalankan perintah berikut untuk memicu peningkatan penggunaan CPU pada **az104-11-vm0** Azure VM:

   ```sh
   for /l %a in (0,0,1) do echo a
   ```

    >**Catatan**: Ini akan memulai perulangan tak terbatas yang akan meningkatkan penggunaan CPU di atas ambang batas aturan peringatan yang baru dibuat.

1. Biarkan sesi Desktop Jarak Jauh terbuka dan beralih kembali ke jendela browser yang menampilkan portal Azure di komputer lab Anda.

1. Di portal Azure, navigasikan kembali ke bilah **Monitor** dan klik **Peringatan**.

1. Catat jumlah peringatan **Sev 3** lalu klik baris **Sev 3**.

    >**Catatan**: Anda mungkin perlu menunggu beberapa menit dan mengeklik **Refresh**.

1. Pada bilah **Semua Peringatan**, tinjau peringatan yang dihasilkan.

#### <a name="task-7-review-azure-log-analytics-functionality"></a>Tugas 7: Meninjau fungsionalitas Azure Log Analytics

1. Di portal Azure, navigasikan kembali ke bilah **Monitor**, klik **Log**.

    >**Catatan**: Anda mungkin perlu mengeklik **Memulai** jika ini adalah pertama kalinya Anda mengakses Log Analytics.

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

    > **Catatan**: Kueri tidak boleh memiliki kesalahan (ditunjukkan dengan blok merah di bilah gulir kanan). Jika kueri tidak akan menempel tanpa kesalahan langsung dari instruksi, tempel kode kueri ke editor teks seperti Notepad, lalu salin dan tempel ke jendela kueri dari sana.


1. Klik **Permintaan** di bilah alat, pada panel **Permintaan**, cari ubin **Lacak ketersediaan VM** dan klik dua kali untuk mengisi jendela kueri, klik tombol perintah **Jalankan** di ubin, dan tinjau hasilnya.

1. Pada tab **Kueri Baru 1**, pilih header **Tabel**, dan tinjau daftar tabel di bagian **Komputer virtual**.

    >**Catatan**: Nama beberapa tabel sesuai dengan solusi yang Anda instal sebelumnya di lab ini.

1. Arahkan mouse ke entri **VMComputer** dan klik ikon **Lihat Pratinjau data**.

1. Jika ada data yang tersedia, di panel **Perbarui**, klik **Gunakan di editor**.

    >**Catatan**: Anda mungkin perlu menunggu beberapa menit sebelum data pembaruan tersedia.

#### <a name="clean-up-resources"></a>Membersihkan sumber daya

>**Catatan**: Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak digunakan akan memastikan bahwa Anda tidak akan melihat biaya yang tidak diharapkan.

>**Catatan**:  Jangan khawatir jika sumber daya lab tidak dapat segera dihapus. Terkadang sumber daya memiliki dependensi dan membutuhkan waktu lebih lama untuk dihapus. Memantau penggunaan sumber daya adalah tugas Administrator yang umum, jadi tinjau sumber daya Anda secara berkala di Portal untuk melihat bagaimana pembersihannya. 

1. Di portal Microsoft Azure, buka sesi **PowerShell** dalam panel **Cloud Shell**.

1. Buat daftar semua grup sumber daya yang dibuat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-11*'
   ```

1. Hapus semua grup sumber daya yang Anda buat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-11*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Catatan**: Perintah dijalankan secara asinkron (sebagaimana yang ditentukan oleh parameter -AsJob), jadi saat Anda akan dapat menjalankan perintah PowerShell lain langsung setelahnya dalam sesi PowerShell yang sama, proses ini akan memakan waktu beberapa menit sebelum grup sumber daya benar-benar dihapus.

#### <a name="review"></a>Tinjauan

Di lab ini, Anda telah:

+ Memprovisikan lingkungan lab
+ Membuat dan mengonfigurasi ruang kerja Azure Log Analytics dan solusi berbasis Azure Automation
+ Meninjau pengaturan pemantauan default mesin virtual Azure
+ Pengaturan diagnostik mesin virtual Azure yang dikonfigurasi
+ Meninjau fungsionalitas Azure Monitor
+ Meninjau fungsionalitas Azure Log Analytics
