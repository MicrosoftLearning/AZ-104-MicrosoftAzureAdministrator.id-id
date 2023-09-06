---
lab:
  title: 'Lab 10: Menerapkan Perlindungan Data'
  module: Administer Data Protection
---

# Lab 10 - Mencadangkan mesin virtual
# Panduan lab siswa

## Skenario lab

Anda telah ditugaskan untuk mengevaluasi penggunaan Layanan Pemulihan Azure untuk pencadangan dan pemulihan file yang di-hosting di mesin virtual Azure dan komputer lokal. Selain itu, Anda ingin mengidentifikasi metode untuk melindungi data yang disimpan di Recovery Services vault dari kehilangan data yang tidak disengaja atau berbahaya.

**Catatan:** Tersedia **[simulasi lab interaktif](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2016)** yang memungkinkan Anda mengklik lab ini sesuai keinginan Anda. Anda mungkin menemukan sedikit perbedaan antara simulasi interaktif dan lab yang dihosting, tetapi konsep dan ide utama yang ditunjukkan sama. 

## Tujuan

Di lab ini Anda akan:

+ Tugas 1: Menyediakan lingkungan lab
+ Tugas 2: Membuat vault Recovery Service
+ Tugas 3: Menerapkan pencadangan tingkat mesin virtual Azure
+ Tugas 4: Menerapkan pencadangan File dan Folder
+ Tugas 5: Melakukan pemulihan file menggunakan agen Layanan Pemulihan Azure
+ Tugas 6: Melakukan pemulihan file menggunakan snapshot mesin virtual Azure (opsional)
+ Tugas 7: Meninjau fungsionalitas penghapusan permanen Layanan Pemulihan Azure (opsional)

## Perkiraan waktu: 50 menit

## Diagram arsitektur

![gambar](../media/lab10.png)

### Petunjuk

## Latihan 1

## Tugas 1: Menyediakan lingkungan lab

Dalam tugas ini, Anda akan menyebarkan dua mesin virtual yang akan digunakan untuk menguji skenario pencadangan yang berbeda.

1. Masuk ke [portal Microsoft Azure](https://portal.azure.com).

1. Di portal Microsoft Azure, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**.

    >**Catatan**: Jika ini pertama kalinya Anda memulai **Cloud Shell** dan Anda melihat pesan **Anda tidak memiliki penyimpanan yang terinstal**, pilih langganan yang Anda gunakan di lab ini, dan klik **Buat penyimpanan**.

1. Di toolbar panel Cloud Shell, klik ikon **Unggah/Unduh file**, di menu menurun, klik **Unggah** dan unggah file **\\Allfiles\\Labs\\10\\az104-10-vms-edge-template.json** dan **\\Allfiles\\Labs\\10\\az104-10-vms-edge-parameters.json** ke dalam direktori beranda Cloud Shell.

1. Dari panel Cloud Shell, jalankan cara berikut ini untuk membuat grup sumber daya yang akan meng-hosting mesin virtual (ganti tempat penampung `[Azure_region]` dengan nama wilayah Azure tempat Anda ingin menyebarkan mesin virtual Azure). Ketikkan setiap baris perintah secara terpisah dan jalankan secara terpisah:

   ```powershell
   $location = '[Azure_region]'
    ```
    
   ```powershell
   $rgName = 'az104-10-rg0'
    ```
    
   ```powershell
   New-AzResourceGroup -Name $rgName -Location $location
   ```

1. Dari panel Cloud Shell, jalankan perintah berikut untuk membuat jaringan virtual pertama dan menyebarkan mesin virtual ke dalamnya menggunakan file templat dan parameter yang Anda unggah:
    >**Catatan**: Anda akan diminta untuk memberikan kata sandi Admin.
    
   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-10-vms-edge-template.json `
      -TemplateParameterFile $HOME/az104-10-vms-edge-parameters.json `
      -AsJob
   ```

1. Kecilkan Cloud Shell (tetapi jangan tutup).

    >**Catatan**: Jangan menunggu penyebaran selesai tetapi lanjutkan ke tugas berikutnya. Penyebaran akan memakan waktu sekitar 5 menit.

## Tugas 2: Membuat vault Recovery Service

Dalam tugas ini, Anda akan membuat brankas layanan pemulihan.

1. Di portal Microsoft Azure, cari dan pilih **Recovery Services vaults** dan, pada bilah **Recovery Services vaults**, klik **+ Buat**.

1. Pada bilah **Buat Recovery Services vault**, tentukan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | nama grup sumber daya baru **az104-10-rg1** |
    | Nama Vault | **az104-10-rsv1** |
    | Wilayah | nama wilayah tempat Anda menggunakan dua mesin virtual di tugas sebelumnya |

    >**Catatan**: Pastikan Anda menentukan wilayah yang sama tempat Anda menyebarkan mesin virtual di tugas sebelumnya.

1. Klik **Tinjauan + Buat**, pastikan validasi lulus dan klik **Buat**.

    >**Catatan**: Tunggu hingga penyebaran selesai. Penyebaran akan memakan waktu kurang dari 1 menit.

1. Saat penyebaran selesai, klik **Buka Sumber Daya**.

1. Pada bilah Recovery Services vault **az104-10-rsv1**, di bagian **Pengaturan**, klik **Properti**.

1. Pada bilah **az104-10-rsv1 - Properti**, klik tautan **Perbarui** pada label **Konfigurasi Cadangan**.

1. Pada bilah **Konfigurasi Cadangan**, perhatikan bahwa Anda dapat mengatur **Jenis replikasi penyimpanan** menjadi **Locally-redundant** atau **Geo-redundan**. Biarkan pengaturan default **Geo-redundan** di tempatnya dan tutup bilah.

    >**Catatan**: Pengaturan ini hanya dapat dikonfigurasi jika tidak ada item cadangan yang ada.

1. Kembali ke bilah **az104-10-rsv1 - Properti**, klik tautan **Perbarui** pada label **Pengaturan Keamanan**.

1. Pada bilah **Pengaturan Keamanan**, perhatikan bahwa **Penghapusan Sementara (Untuk beban kerja yang berjalan di Azure)** **Diaktifkan**.

1. Tutup bilah **Pengaturan Keamanan** dan, kembali ke bilah vault **az104-10-rsv1** Layanan Pemulihan, klik **Ringkasan**.

## Tugas 3: Menerapkan pencadangan tingkat mesin virtual Azure

Dalam tugas ini, Anda akan menerapkan pencadangan tingkat mesin virtual Azure.

   >**Catatan**: Sebelum Anda memulai tugas ini, pastikan penyebaran yang Anda mulai di tugas pertama lab ini telah berhasil diselesaikan.

1. Pada bilah Recovery Services vault **az104-10-rsv1**, klik **Ringkasan**, lalu klik **+ Cadangan**.

1. Pada bilah **Sasaran Pencadangan**, tentukan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Di mana beban kerja Anda berjalan? | **Azure** |
    | Apa yang ingin Anda cadangkan? | **Mesin virtual** |

1. Pada bilah **Sasaran Pencadangan**, klik **Cadangan**.

1. Pada **Kebijakan pencadangan**, tinjau pengaturan **DefaultPolicy** dan pilih **Buat kebijakan baru**.

1. Tetapkan kebijakan pencadangan baru dengan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | ---- | ---- |
    | Nama kebijakan | **az104-10-backup-policy** |
    | Frekuensi | **Harian** |
    | Waktu | **12:00 Siang** |
    | Timezone | nama zona waktu lokal Anda |
    | Pertahankan snapshot pemulihan instans untuk | **2** Hari |

1. Klik **OK** untuk membuat kebijakan, lalu di bagian **Virtual Machines**, pilih **Tambah**.

1. Pada bilah **Pilih mesin virtual**, pilih **az-104-10-vm0**, klik **OK**, dan kembali ke bilah **Cadangan** , klik **Aktifkan pencadangan**.

    >**Catatan**: Tunggu hingga cadangan diaktifkan. Proses ini memerlukan waktu sekitar 2 menit.

1. Navigasikan kembali ke bilah Recovery Services vault **az104-10-rsv1**, di bagian **Item yang dilindungi**, klik **Item cadangan**, lalu klik ikon **Azure entri mesin virtual**.

1. Pada bilah **Item Cadangan (Mesin Virtual Azure)** pilih tautan **Lihat detail** untuk **az104-10-vm0**, dan tinjau nilai **Cadangan Entri Pra-Pemeriksaan** dan **Status Cadangan Terakhir**.

1. Pada bilah **az104-10-vm0** Item Cadangan, klik **Cadangkan sekarang**, terima nilai default di daftar dropdown **Pertahankan Pencadagan Sampai**, dan klik **Ok**.

    >**Catatan**: Jangan menunggu hingga pencadangan selesai, tetapi lanjutkan ke tugas berikutnya.

## Tugas 4: Menerapkan pencadangan File dan Folder

Dalam tugas ini, Anda akan menerapkan pencadangan file dan folder menggunakan Layanan Pemulihan Azure.

1. Di portal Microsoft Azure, cari dan pilih **Mesin Virtual**, dan pada bilah **Mesin Virtual**, klik **az104-10-vm1**.

1. Pada bilah **az104-10-vm1**, klik **Hubungkan**, di menu menurun, klik **RDP**, di bilah **Hubungkan dengan RDP**, klik **Unduh File RDP** dan ikuti petunjuk untuk memulai sesi Desktop Jarak Jauh.

    >**Catatan**: Langkah ini mengacu pada menghubungkan melalui Desktop Jauh dari komputer Windows. Di Mac, Anda dapat menggunakan Klien Desktop Jauh dari Mac App Store dan di komputer Linux Anda dapat menggunakan perangkat lunak klien RDP sumber terbuka.

    >**Catatan**: Anda dapat mengabaikan permintaan peringatan apa pun saat menghubungkan ke mesin virtual target.

1. Saat diminta, masuk menggunakan nama pengguna **Siswa** dan sandi dari file parameter.

    >**Catatan:** Karena portal Microsoft Azure tidak mendukung IE11 lagi, Anda harus menggunakan Browser Microsoft Edge untuk tugas ini.

1. Dalam sesi Desktop Jarak Jauh ke mesin virtual Azure **az104-10-vm1**, mulai browser web Edge, jelajahi [portal Microsoft Azure](https://portal.azure.com), dan masuk menggunakan kredensial Anda.

1. Di portal Microsoft Azure, cari dan pilih **Recovery Services vaults** dan, di **Recovery Services vaults**, klik **az104-10-rsv1**.

1. Pada bilah Recovery Services vault **az104-10-rsv1**, klik **+ Cadangan**.

1. Pada bilah **Sasaran Pencadangan**, tentukan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Di mana beban kerja Anda berjalan? | **Lokal** |
    | Apa yang ingin Anda cadangkan? | **File dan folder** |

    >**Catatan**: Meskipun mesin virtual yang Anda gunakan dalam tugas ini berjalan di Azure, Anda dapat memanfaatkannya untuk mengevaluasi kemampuan pencadangan yang berlaku untuk komputer lokal mana pun yang menjalankan sistem operasi Windows Server.

1. Pada bilah **Sasaran Cadangan**, klik **Siapkan infrastruktur**.

1. Pada bilah **Siapkan infrastruktur**, klik tautan **Unduh Agen untuk Windows Server atau Windows Client**.

1. Saat diminta, klik **Jalankan** untuk memulai penginstalan **MARSAgentInstaller.exe** dengan pengaturan default.

    >**Catatan**: Pada halaman **Opsi Ikut Serta Pembaruan Microsoft** pada **Wizard Penyiapan Agen Layanan Pemulihan Microsoft Azure**, pilih opsi penginstalan **Saya tidak ingin menggunakan Pembaruan Microsoft**.

1. Pada halaman **Penginstalan** **Wizard Penyiapan Agen Layanan Pemulihan Microsoft Azure**, klik **Lanjutkan ke Pendaftaran**. Operasi ini akan memulai **Daftarkan Wizard Server**.

1. Beralih ke jendela window web yang menampilkan portal Microsoft Azure, pada bilah **Siapkan infrastruktur**, beri centang kotak **Sudah diunduh atau menggunakan Agen Server Pemulihan terbaru**, dan klik **Unduh**.

1. Saat diminta, entah Anda akan ingin membuka atau menyimpan file kredensial vault, klik **Simpan**. Cara ini akan menyimpan file kredensial vault ke folder Unduhan lokal.

1. Beralih kembali ke jendela **Daftarkan Wizard Server** dan, pada halaman **Identifikasi Vault**, klik **Jelajahi**.

1. Dalam kotak dialog **Pilih Kredensial Vault**, jelajahi ke folder **Unduhan**, klik file kredensial vault yang Anda unduh, dan klik **Buka**.

1. Kembali ke halaman **Identifikasi Vault**, klik **Berikutnya**.

1. Pastikan **Simpan frasa sandi dengan aman ke Azure Key Vault** tidak diperiksa. 

1. Pada halaman **Pengaturan Enkripsi** dari **Daftarkan Wizard Server**, klik **Buat Frasa Sandi**.

1. Pada halaman **Pengaturan Enkripsi** dari **Daftarkan Wizard Server**, klik tombol **Jelajahi** di sebelah **Masukkan lokasi untuk menyimpan frasa sandi**.

1. Dalam kotak dialog **Jelajahi Folder**, pilih folder **Dokumen** dan klik **OK**.

1. Klik **Selesai**, tinjau peringatan **Microsoft Azure Backup** dan klik **Ya**, dan tunggu hingga pendaftaran selesai.

    >**Catatan**: Di lingkungan produksi, Anda harus menyimpan file frasa sandi di lokasi yang aman selain server yang dicadangkan.

1. Pada halaman **Pendaftaran Server** dari **Daftarkan Wizard Server**, tinjau peringatan terkait lokasi file frasa sandi, pastikan bahwa kotak centang **Luncurkan Agen Layanan Pemulihan Microsoft Azure** dipilih dan klik **Tutup**. Cara ini akan secara otomatis membuka konsol **Microsoft Azure Backup**.

1. Di konsol **Microsoft Azure Backup**, di panel **Tindakan**, klik **Jadwalkan Pencadangan**.

1. Di **Jadwalkan Wizard Pencadangan**, pada halaman **Memulai**, klik **Berikutnya**.

1. Pada halaman **Pilih Item untuk Dicadangkan**, klik **Tambahkan Item**.

1. Di kotak dialog **Pilih Item**, perluas **C:\\Windows\\System32\\drivers\\etc\\** , pilih **host** , lalu klik **OK**:

1. Pada halaman **Pilih Item untuk Dicadangkan**, klik **Berikutnya**.

1. Pada halaman **Tentukan Jadwal Pencadangan**, pastikan bahwa opsi **Hari** dipilih, pada kotak daftar dropdown pertama pada **Pada waktu berikut (Nilai maksimum yang diizinkan adalah tiga kali sehari)** , pilih **04:30 Pagi**, lalu klik **Berikutnya**.

1. Pada halaman **Pilih Kebijakan Retensi**, terima nilai defaultnya, lalu klik **Berikutnya**.

1. Pada halaman **Pilih jenis Cadangan Awal**, terima defaultnya, lalu klik **Berikutnya**.

1. Pada halaman **Konfirmasi**, klik **Selesai**. Saat jadwal pencadangan dibuat, klik **Tutup**.

1. Di konsol **Microsoft Azure Backup**, di panel Tindakan, klik **Cadangkan Sekarang**.

    >**Catatan**: Opsi untuk menjalankan pencadangan sesuai permintaan tersedia setelah Anda membuat pencadangan terjadwal.

1. Di Wizard Pencadangan Sekarang, pada halaman **Pilih Item Cadangan**, pastikan bahwa opsi **File dan Folder** dipilih dan klik **Berikutnya**.

1. Pada halaman **Simpan Cadangan Hingga**, terima pengaturan default dan klik **Berikutnya**.

1. Pada halaman **Konfirmasi**, klik **Cadangkan**.

1. Setelah pencadangan selesai, klik **Tutup**, lalu tutup Cadangan Microsoft Azure.

1. Beralih ke jendela browser web yang menampilkan portal Microsoft Azure, navigasikan kembali ke bilah **Recovery Services vault**, di bagian **Item yang dilindungi**, dan klik **Item cadangan**.

1. Pada bilah **az104-10-rsv1 - Cadangan item**, klik **Agen Azure Backup**.

1. Pada bilah **Item Cadangan (Agen Azure Backup)** , pastikan ada entri yang merujuk pada drive **C:\\** dari **az104-10-vm1.** .

## Tugas 5: Lakukan pemulihan file menggunakan agen Layanan Pemulihan Azure (opsional)

Dalam tugas ini, Anda akan melakukan pemulihan file menggunakan agen Layanan Pemulihan Azure.

1. Dalam sesi Desktop Jarak Jauh ke **az104-10-vm1**, buka File Explorer, navigasikan ke folder **C:\\Windows\\System32\\drivers\\etc\\** dan hapus file **host**.

1. Buka Microsoft Azure Backup dan klik **Pulihkan data** di panel **Tindakan**. Cara ini akan memulai **Wizard Pemulihan Data**.

1. Pada halaman **Memulai** dari **Wizard Pemulihan Data**, maka opsi **Server ini (az104-10-vm1.)** dipilih dan klik **Berikutnya**.

1. Pada halaman **Pilih Mode Pemulihan**, pastikan opsi **Berkas dan folder individual** dipilih, lalu klik **Berikutnya**.

1. Pada halaman **Pilih Volume dan Tanggal**, dalam daftar dropdown **Pilih volume**, pilih **C:\\** , terima pilihan default dari cadangan yang tersedia , dan klik **Pasang**.

    >**Catatan**: Tunggu hingga operasi pemasangan selesai. Operasi ini mungkin perlu waktu sekitar 2 menit.

1. Pada halaman **Jelajahi Dan Pulihkan Berkas**, catat huruf drive volume pemulihan dan tinjau tips terkait penggunaan robocopy.

1. Klik **Mulai**, luaskan map **Sistem Windows**, dan klik **Perintah**.

1. Dari Perintah, jalankan perintah berikut untuk menyalin file **host** pemulihan ke lokasi aslinya (ganti `[recovery_volume]` dengan huruf drive volume pemulihan yang Anda identifikasi sebelumnya):

   ```sh
   robocopy [recovery_volume]:\Windows\System32\drivers\etc C:\Windows\system32\drivers\etc hosts /r:1 /w:1
   ```

1. Beralih kembali ke **Wizard Pemulihan Data** dan, pada **Jelajahi dan Pulihkan File**, klik **Lepaskan** dan, ketika diminta untuk mengonfirmasi, klik **Ya**.

1. Hentikan sesi Desktop Jarak Jauh.

## Tugas 6: Melakukan pemulihan file menggunakan snapshot mesin virtual Azure (opsional)

Dalam tugas ini, Anda akan memulihkan file dari cadangan berbasis snapshot tingkat mesin virtual Azure.

1. Beralih ke jendela browser yang berjalan di komputer lab Anda dan tampilkan portal Microsoft Azure.

1. Di portal Microsoft Azure, cari dan pilih **Mesin virtual**, dan pada bilah **Mesin virtual**, klik **az104-10-vm0**.

1. Pada bilah **az104-10-vm0**, klik **Hubungkan**, di menu menurun, klik **RDP**, di bilah **Hubungkan dengan RDP**, klik **Unduh File RDP** dan ikuti petunjuk untuk memulai sesi Desktop Jarak Jauh.

    >**Catatan**: Langkah ini mengacu pada menghubungkan melalui Desktop Jauh dari komputer Windows. Di Mac, Anda dapat menggunakan Klien Desktop Jauh dari Mac App Store dan di komputer Linux Anda dapat menggunakan perangkat lunak klien RDP sumber terbuka.

    >**Catatan**: Anda dapat mengabaikan permintaan peringatan apa pun saat menghubungkan ke mesin virtual target.

1. Saat diminta, masuk menggunakan nama pengguna **Siswa** dan sandi dari file parameter.

   >**Catatan:** Karena portal Microsoft Azure tidak mendukung IE11 lagi, Anda harus menggunakan Browser Microsoft Edge untuk tugas ini.

1. Dalam sesi Desktop Jarak Jauh ke **az104-10-vm0**, klik **Mulai**, luaskan folder **Sistem Windows**, dan klik **Perintah**.

1. Dari Perintah, jalankan perintah berikut untuk menghapus file **host**:

   ```sh
   del C:\Windows\system32\drivers\etc\hosts
   ```

   >**Catatan**: Anda akan memulihkan file ini dari cadangan berbasis snapshot tingkat mesin virtual Azure nanti dalam tugas ini.

1. Dalam sesi Desktop Jarak Jauh ke mesin virtual Azure **az104-10-vm0**, mulai browser web Edge, jelajahi [portal Microsoft Azure](https://portal.azure.com), dan masuk menggunakan kredensial Anda.

1. Di portal Microsoft Azure, cari dan pilih **Recovery Services vaults** dan, di **Recovery Services vaults**, klik **az104-10-rsv1**.

1. Pada bilah Recovery Services vault **az104-10-rsv1**, di bagian **Item yang dilindungi**, klik **Item cadangan**.

1. Pada bilah **az104-10-rsv1 - Cadangan item**, klik **Mesin Virtual Azure**.

1. Pada bilah **Item Cadangan (Mesin Virtual Azure)** , pilih **Lihat detail** untuk **az104-10-vm0**.

1. Pada bilah **az104-10-vm0** Item Cadangan, klik **Pemulihan File**.

    >**Catatan**: Anda memiliki opsi untuk menjalankan pemulihan segera setelah pencadangan dimulai berdasarkan snapshot aplikasi yang konsisten.

1. Pada bilah **Pemulihan File**, terima titik pemulihan default dan klik **Unduh yang Dapat Dieksekusi**.

    >**Catatan**: Skrip memasang disk dari titik pemulihan yang dipilih sebagai drive lokal dalam sistem operasi tempat skrip dijalankan.

1. Klik **Unduh** dan, saat diminta untuk menjalankan atau menyimpan **IaaSVMILRExeForWindows.exe**, klik **Simpan**.

1. Kembali ke jendela File Explorer, klik dua kali file yang baru diunduh.

1. Saat diminta untuk memberikan sandi dari portal, salin sandi dari kotak teks **Kata sandi untuk menjalankan skrip** pada bilah **Pemulihan File**, tempel kata sandi di Perintah, dan tekan **Enter**.

    >**Catatan**: Upaya ini akan membuka jendela Windows PowerShell yang menampilkan progres pemasangan.

    >**Catatan**: Jika Anda menerima pesan kesalahan saat ini, refresh jendela browser web dan ulangi tiga langkah terakhir.

1. Tunggu hingga proses pemasangan selesai, tinjau pesan informasi di jendela Windows PowerShell, catat huruf drive yang ditetapkan untuk hosting volume **Windows**, dan mulai File Explorer.

1. Di File Explorer, navigasikan ke huruf drive yang menampung snapshot volume sistem operasi yang Anda identifikasi di langkah sebelumnya dan tinjau isinya.

1. Beralih ke jendela **Perintah**.

1. Dari Perintah, jalankan perintah berikut untuk menyalin file **host** pemulihan ke lokasi aslinya (ganti `[os_volume]` dengan huruf drive volume sistem operasi yang Anda identifikasi sebelumnya):

   ```sh
   robocopy [os_volume]:\Windows\System32\drivers\etc C:\Windows\system32\drivers\etc hosts /r:1 /w:1
   ```

1. Beralih kembali ke bilah **Pemulihan File** di portal Microsoft Azure dan klik **Lepaskan Disk**.

1. Hentikan sesi Desktop Jarak Jauh.

## Tugas 7: Tinjau fungsionalitas penghapusan sementara Layanan Pemulihan Azure

1. Di komputer lab, di portal Microsoft Azure, cari dan pilih **Recovery Services vaults** dan, di **Recovery Services vaults**, klik **az104-10-rsv1**.

1. Pada bilah Recovery Services vault **az104-10-rsv1**, di bagian **Item yang dilindungi**, klik **Item cadangan**.

1. Pada bilah **az104-10-rsv1 - Cadangan item**, klik **Agen Azure Backup**.

1. Pada bilah **Item Cadangan (Agen Azure Backup)** , klik entri yang mewakili cadangan **az104-10-vm1**.

1. Di **C:\\ pada bilah az104-10-vm1.** , pilih **Lihat detail** untuk **az104-10-vm1.** .

1. Pada panel Detail, klik **az104-10-vm1**.

1. Di bilah **az104-10-vm1.** Server yang Dilindungi, klik **Hapus**.

1. Pada bilah **Hapus**, tentukan pengaturan berikut.

    | Pengaturan | Nilai |
    | --- | --- |
    | KETIKKAN NAMA SERVER | **az104-10-vm1.** |
    | Alasan | **Mendaur ulang server Dev/Uji** |
    | Komentar | **az104 10 lab** |

    >**Catatan**: Pastikan untuk menyertakan titik di akhir saat mengetikkan nama server

1. Aktifkan kotak centang di sebelah label **Ada data cadangan dari 1 item cadangan yang terkait dengan server ini. Saya memahami bahwa mengeklik "Konfirmasi" akan menghapus semua data cadangan cloud secara permanen. Tindakan ini tidak bisa dibatalkan. Peringatan mungkin dikirimkan ke administrator langganan ini yang memberitahukan mereka tentang penghapusan ini** dan klik **Hapus**.

    >**Catatan**: Cara ini akan gagal karena fitur **Penghapusan sementara** harus dinonaktifkan.

1. Navigasikan kembali ke bilah **az104-10-rsv1 - Cadangan item** dan klik **Azure Virtual Machines**.

1. Pada bilah **az104-10-rsv1 - Cadangan item**, klik **Mesin Virtual Azure**.

1. Pada bilah **Item Cadangan (Mesin Virtual Azure)** , pilih **Lihat detail** untuk **az104-10-vm0**.

1. Pada bilah **az104-10-vm0** Item Cadangan, klik **Hentikan pencadangan**.

1. Pada bilah **Hentikan pencadangan**, pilih **Hapus Data Cadangan**, tentukan pengaturan berikut dan klik **Hentikan pencadangan**:

    | Pengaturan | Nilai |
    | --- | --- |
    | Ketikkan nama item Cadangan | **az104-10-vm0** |
    | Alasan | **Lainnya** |
    | Komentar | **az104 10 lab** |

1. Navigasikan kembali ke bilah **az104-10-rsv1 - Cadangan item** dan klik **Refresh**.

    >**Catatan**: Entri **Mesin Virtual Azure** masih mencantumkan **1** item cadangan.

1. Klik entri **Mesin Virtual Azure** dan, pada bilah **Item Cadangan (Mesin Virtual Azure)** , klik entri **az104-10-vm0**.

1. Pada bilah **az104-10-vm0** Item Cadangan, perhatikan bahwa Anda memiliki opsi untuk **Membatalkan penghapusan** cadangan yang dihapus.

    >**Catatan**: Fungsionalitas ini disediakan oleh fitur penghapusan sementara, yang secara default, diaktifkan untuk pencadangan mesin virtual Azure.

1. Navigasikan kembali ke bilah Recovery Services vault **az104-10-rsv1**, dan di bagian **Pengaturan**, klik **Properti**.

1. Pada bilah **az104-10-rsv1 - Properti**, klik tautan **Perbarui** pada label **Pengaturan Keamanan**.

1. Pada bilah **Pengaturan Keamanan**, Nonaktifkan **Penghapusan Sementara (Untuk beban kerja yang berjalan di Azure)** dan juga nonaktifkan **Fitur Keamanan (Untuk beban kerja yang berjalan di tempat)** dan klik **Simpan**.

    >**Catatan**: Cara ini tidak akan memengaruhi item yang sudah dalam status hapus sementara.

1. Tutup bilah **Pengaturan Keamanan** dan, kembali ke bilah vault **az104-10-rsv1** Layanan Pemulihan, klik **Ringkasan**.

1. Navigasikan kembali ke bilah **az104-10-vm0** Item Cadangan dan klik **Batalkan penghapusan**.

1. Pada bilah **Batalkan penghapusan az104-10-vm0**, klik **Batalkan penghapusan**.

1. Tunggu hingga operasi pembatalan penghapusan selesai, refresh halaman browser web, jika perlu navigasikan kembali ke bilah Item Cadangan **az104-10-vm0**, dan klik **Hapus data cadangan**.

1. Pada bilah **Hapus Data Cadangan**, tentukan pengaturan berikut dan klik **Hapus**:

    | Pengaturan | Nilai |
    | --- | --- |
    | Ketikkan nama item Cadangan | **az104-10-vm0** |
    | Alasan | **Lainnya** |
    | Komentar | **az104 10 lab** |

1. Ulangi langkah-langkah di awal tugas ini untuk menghapus item cadangan untuk **az104-10-vm1**.

## Membersihkan sumber daya

>**Catatan**: Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak digunakan akan memastikan bahwa Anda tidak akan melihat biaya yang tidak diharapkan.

>**Catatan**:  Jangan khawatir jika sumber daya lab tidak dapat segera dihapus. Terkadang sumber daya memiliki dependensi dan membutuhkan waktu lebih lama untuk dihapus. Ini adalah tugas Administrator yang umum untuk memantau penggunaan sumber daya, jadi tinjauan sumber daya Anda secara berkala di Portal untuk melihat bagaimana pembersihannya. 

1. Di portal Microsoft Azure, buka sesi **PowerShell** dalam panel **Cloud Shell**.

1. Buat daftar semua grup sumber daya yang dibuat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-10*'
   ```

1. Hapus semua grup sumber daya yang Anda buat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-10*' | Remove-AzResourceGroup -Force -AsJob
   ```

   >**Catatan**: Secara opsional, Anda dapat mempertimbangkan untuk menghapus grup sumber daya yang dibuat secara otomatis dengan awalan **AzureBackupRG_** (tidak ada biaya tambahan yang terkait dengan keberadaannya).

    >**Catatan**: Perintah dijalankan secara asinkron (sebagaimana yang ditentukan oleh parameter -AsJob), jadi saat Anda akan dapat menjalankan perintah PowerShell lain langsung setelahnya dalam sesi PowerShell yang sama, proses ini akan memakan waktu beberapa menit sebelum grup sumber daya benar-benar dihapus.

## Tinjau

Di lab ini, Anda telah:

+ Menyediakan lingkungan lab
+ Membuat Recovery Services vault
+ Menerapkan pencadangan tingkat mesin virtual Azure
+ Menerapkan pencadangan File dan Folder
+ Melakukan pemulihan file menggunakan agen Layanan Pemulihan Azure
+ Melakukan pemulihan file dengan menggunakan snapshot mesin virtual Azure
+ Meninjau fungsionalitas penghapusan sementara Layanan Pemulihan Azure
