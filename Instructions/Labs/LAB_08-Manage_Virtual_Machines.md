---
lab:
  title: 'Lab 08: Mengelola Komputer Virtual'
  module: Administer Virtual Machines
---

# Lab 08 - Kelola Virtual Machines
# Panduan lab siswa

## Skenario laboratorium

Anda ditugaskan untuk mengidentifikasi berbagai opsi untuk menyebarkan dan mengonfigurasi mesin virtual Azure. Pertama, Anda perlu menentukan opsi ketahanan dan skalabilitas komputasi dan penyimpanan yang berbeda yang dapat Anda terapkan saat menggunakan mesin virtual Azure. Selanjutnya, Anda perlu menyelidiki opsi ketahanan dan skalabilitas komputasi dan penyimpanan yang tersedia saat menggunakan kumpulan skala mesin virtual Azure. Anda juga ingin menjelajahi kemampuan untuk secara otomatis mengonfigurasi mesin virtual dan kumpulan skala mesin virtual menggunakan ekstensi Skrip Kustom Komputer Virtual Azure.

**Catatan:** Tersedia **[simulasi lab interaktif](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2012)** yang memungkinkan Anda mengklik lab ini sesuai keinginan Anda. Anda mungkin menemukan sedikit perbedaan antara simulasi interaktif dan lab yang dihosting, tetapi konsep dan ide utama yang ditunjukkan sama. 

## Tujuan

Di lab ini Anda akan:

+ Tugas 1: Menyebarkan komputer virtual Azure yang tangguh zona dengan menggunakan portal Azure dan templat Azure Resource Manager
+ Tugas 2: Mengonfigurasi komputer virtual Azure dengan menggunakan ekstensi komputer virtual
+ Tugas 3: Menskalakan komputasi dan penyimpanan untuk komputer virtual Azure
+ Tugas 4: Mendaftarkan penyedia sumber daya Microsoft.Insights dan Microsoft.AlertsManagement
+ Tugas 5: Menyebarkan set skala komputer virtual Azure yang tangguh zona dengan menggunakan portal Azure
+ Tugas 6: Mengonfigurasi set skala komputer virtual Azure dengan menggunakan ekstensi komputer virtual
+ Tugas 7: Menskalakan komputasi dan penyimpanan untuk set skala komputer virtual Azure (opsional)

## Perkiraan waktu: 50 menit

## Diagram arsitektur

![gambar](../media/lab08.png)


### Petunjuk

## Latihan 1

## Tugas 1: Menyebarkan komputer virtual Azure yang tangguh zona dengan menggunakan portal Azure dan templat Azure Resource Manager

Dalam tugas ini, Anda akan menyebarkan mesin virtual Azure ke zona ketersediaan yang berbeda menggunakan portal Azure dan templat Azure Resource Manager.

1. Masuk ke [portal Azure](http://portal.azure.com).

1. Di portal Microsoft Azure, cari dan pilih **Mesin virtual** dan, pada panel **Mesin virtual**, klik **+ Buat**, klik **+ Mesin virtual Azure**.

1. Pada tab **Dasar** pada bilah **Buat komputer virtual**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang akan Anda gunakan di lab ini |
    | Grup sumber daya | nama grup sumber daya baru **az104-08-rg01** |
    | Nama komputer virtual | **az104-08-vm0** |
    | Wilayah | pilih salah satu wilayah yang mendukung zona ketersediaan dan lokasi Anda dapat menyediakan mesin virtual Azure |
    | Opsi ketersediaan | **Zona ketersediaan** |
    | Zona ketersediaan | **Zona 1** |
    | Gambar | **Pusta Data Windows Server 2019 - Gen1/Gen2** |
    | Instans Azure Spot | **Tidak** |
    | Ukuran | **Standar D2s v3** |
    | Nama Pengguna | **Mahasiswa** |
    | Kata sandi | **Berikan kata sandi yang aman** |
    | Port masuk publik | **Tidak ada** |
    | Apakah Anda ingin menggunakan lisensi Windows Server yang sudah ada? | **Tidak Dicentang** |

1. Klik **Berikutnya: Disk >** dan, pada tab **Disk** dari bilah **Buat komputer** virtual, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Jenis disk OS | **SSD Premium** |
    | Mengaktifkan kompatibilitas Ultra Disk | **Tidak Dicentang** |

1. Klik **Berikutnya: Jaringan >** dan, pada tab **Jaringan** dari bilah **Buat komputer** virtual, klik **Buat baru** di bawah **kotak teks Jaringan** virtual.

1. Pada bilah **Buat jaringan virtual**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **az104-08-vnet01** |
    | Rentang alamat | **10.80.0.0/20** |
    | Nama subnet | **subnet0** |
    | Rentang subnet | **10.80.0.0/24** |

1. Klik **OK** dan, kembali ke tab **Jaringan** bilah **Buat mesin virtual**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Subnet | **subnet0** |
    | IP Publik | **Default** |
    | kelompok keamanan jaringan NIC | **dasar** |
    | Port masuk publik | **Tidak ada** |
    | Jaringan yang dipercepat | **Off**
    | Tempatkan komputer virtual ini di belakang solusi penyeimbang muatan yang ada? | **Tidak Dicentang** |

1. Klik **Berikutnya: Manajemen >** dan, pada tab **Manajemen** dari bilah **Buat komputer** virtual, tentukan pengaturan berikut (biarkan orang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Opsi orkestrasi patch | **Pembaruan secara manual** |  

1. Klik **Berikutnya: Pemantauan >** dan, pada tab **Pemantauan** dari blade **Buat mesin virtual**, tentukan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Diagnostik boot | **Aktifkan dengan akun penyimpanan kustom** |
    | Akun penyimpanan diagnostik | **menerima nilai default** |

    >**Catatan**: Jika perlu, pilih akun penyimpanan yang sudah ada di daftar dropdown atau buat akun penyimpanan baru. Catat nama akun penyimpanan. Anda akan menggunakannya dalam tugas berikutnya.

1. Klik **Berikutnya: >** Tingkat Lanjut, pada tab **Tingkat Lanjut** dari bilah **Buat komputer** virtual, tinjau pengaturan yang tersedia tanpa memodifikasi salah satunya, dan klik **Tinjau + Buat**.

1. Pada bilah **Tinjau + Buat**, klik **Buat**.

1. Pada bilah penyebaran, klik **Templat**.

1. Tinjau templat yang menunjukkan penyebaran yang sedang berlangsung dan klik **Sebarkan**.

    >**Catatan**: Anda akan menggunakan opsi ini untuk menyebarkan komputer virtual kedua dengan konfigurasi yang cocok kecuali untuk zona ketersediaan.

1. Pada bilah **Penyebaran kustom**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Grup Sumber Daya | **az104-08-rg01** |
    | Nama Antarmuka Jaringan | **az104-08-vm1-nic1** |
    | Nama Alamat IP Publik | **az104-08-vm1-ip** |
    | Nama Mesin Virtual, Name1 Mesin Virtual, Nama Komputer Mesin Virtual   | **az104-08-vm1** |
    | RG Mesin Virtual | **az104-08-rg01** |    
    | Nama Pengguna Admin | **Mahasiswa** |
    | Kata Sandi Admin | **Berikan kata sandi yang aman**  |
    | Aktifkan Hotpatching | **salah** |
    | Zone | **2** |

    >**Catatan**: Anda perlu memodifikasi parameter yang sesuai dengan properti sumber daya berbeda yang Anda sebarkan dengan menggunakan templat, termasuk komputer virtual dan antarmuka jaringannya.

1. Klik **Tinjau + Buat**, pada bilah **Tinjau + Buat**, klik **Buat**.

    >**Catatan**: Tunggu hingga kedua penyebaran selesai sebelum Anda melanjutkan ke tugas berikutnya. Proses ini mungkin perlu waktu sekitar 5 menit.

## Tugas 2: Mengonfigurasi komputer virtual Azure dengan menggunakan ekstensi komputer virtual

Dalam tugas ini, Anda akan menginstal peran Windows Server Web Server pada dua mesin virtual Azure yang Anda gunakan di tugas sebelumnya menggunakan ekstensi mesin virtual Skrip Kustom.

1. Di portal Microsoft Azure, cari dan pilih **Akun penyimpanan** dan, pada bilah **Akun penyimpanan**, klik entri yang mewakili akun penyimpanan diagnostik yang Anda buat di tugas sebelumnya.

1. Pada bilah akun penyimpanan, di bagian **Penyimpanan Data**, klik **Kontainer**, lalu klik **+ Kontainer**.

1. Pada bilah **Kontainer baru**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya) dan klik **Buat**:

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **skrip** |
    | Tingkat akses publik | **Pribadi (tidak ada akses anonim**) |

1. Kembali pada bilah akun penyimpanan yang menampilkan daftar kontainer, klik **skrip**.

1. Pada bilah **skrip**, klik **Unggah**.

1. Pada bilah **Unggah blob**, klik ikon folder, di kotak dialog **Buka**, navigasikan ke folder **\\Allfiles\\Labs\\08**, pilih **az104-08-install_IIS.ps1**, klik **Buka**, dan kembali ke bilah **Unggah blob**, klik **Unggah**.

1. Di portal Microsoft Azure, cari dan pilih **Mesin virtual** dan, pada bilah **Mesin virtual**, klik **az104-08-vm0**.

1. Pada bilah mesin virtual **az104-08-vm0**, di bagian **Pengaturan**, klik **Ekstensi + aplikasi**, dan klik **+ Tambahkan** .

1. Pada bilah **Pasang Ekstensi**, klik **Ekstensi Skrip Kustom**, lalu klik **Berikutnya**.

1. Dari bilah **Konfigurasikan Ekstensi Skrip Kustom**, klik **Telusuri**.

1. Pada bilah **Akun penyimpanan**, klik nama akun penyimpanan tempat Anda mengunggah skrip **az104-08-install_IIS.ps1**, pada bilah **Kontainer**, klik **skrip**, pada bilah **skrip**, klik **az104-08-install_IIS.ps1**, lalu klik **Pilih**.

1. Kembali pada bilah **Pasang ekstensi**, klik **Tinjau + buat** dan, pada bilah **Tinjau + buat** klik **Buat**.

1. Di portal Microsoft Azure, cari dan pilih **Mesin virtual** dan, pada bilah **Mesin virtual**, klik **az104-08-vm1**.

1. Pada bilah **az104-08-vm1**, di bagian **Otomatisasi**, klik **Ekspor templat**.

1. Pada bilah **az104-08-vm1 - Ekspor templat**, klik **Sebarkan**.

1. Pada bilah **Penyebaran kustom**, klik **Edit templat**.

    >**Catatan**: Mengacuhkan pesan yang menyatakan **Grup sumber daya berada di lokasi yang tidak didukung oleh satu atau beberapa sumber daya dalam templat. Pilih grup** sumber daya yang berbeda. Pesan ini diharapkan dan dapat diabaikan dalam kasus ini.

1. Pada bilah **Edit templat**, di bagian yang menampilkan konten templat, masukkan kode berikut yang dimulai dengan baris **20** (tepat di bawah baris `"resources": [`):

   >**Catatan**: Jika Anda menggunakan alat yang menempelkan kode secara sejalan menurut baris intellisense dapat menambahkan tanda kurung ekstra yang menyebabkan kesalahan validasi. Anda mungkin ingin menempelkan kode ke notepad terlebih dahulu lalu menempelkannya ke baris 20.

   ```json
        {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "name": "az104-08-vm1/customScriptExtension",
            "apiVersion": "2018-06-01",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "az104-08-vm1"
            ],
            "properties": {
                "publisher": "Microsoft.Compute",
                "type": "CustomScriptExtension",
                "typeHandlerVersion": "1.7",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "commandToExecute": "powershell.exe Install-WindowsFeature -name Web-Server -IncludeManagementTools && powershell.exe remove-item 'C:\\inetpub\\wwwroot\\iisstart.htm' && powershell.exe Add-Content -Path 'C:\\inetpub\\wwwroot\\iisstart.htm' -Value $('Hello World from ' + $env:computername)"
              }
            }
        },

   ```

   >**Catatan**: Bagian templat ini menentukan ekstensi skrip kustom komputer virtual Azure yang sama dengan yang Anda sebarkan sebelumnya ke komputer virtual pertama melalui Azure PowerShell.

1. Klik **Simpan** dan, kembali pada bilah **Templat kustom**, klik **Tinjau + Buat** dan, pada bilah **Tinjau + Buat**, klik **Buat**

    >**Catatan**: Tunggu hingga penyebaran templat selesai. Anda dapat memantau progresnya dari bilah **Ekstensi** pada mesin virtual **az104-08-vm0** dan **az104-08-vm1**. Operasi ini tidak akan lebih dari 3 menit.

1. Untuk memastikan bahwa konfigurasi berbasis ekstensi Skrip Kustom berhasil, navigasikan kembali pada bilah **az104-08-vm1**, di bagian **Operasi**, klik **Jalankan perintah**, dan dalam daftar perintah, klik **RunPowerShellScript**.

1. Pada bilah **Jalankan Skrip** Perintah, ketik yang berikut ini dan klik **Jalankan** untuk mengakses situs web yang dihosting di **az104-08-vm1**:

   ```powershell
   Invoke-WebRequest -URI http://10.80.0.4 -UseBasicParsing
   ```

    >**Catatan**: Parameter **-UseBasicParsing** diperlukan untuk menghilangkan dependensi pada Internet Explorer untuk menyelesaikan eksekusi cmdlet

    >**Catatan**: Parameter **-URI** adalah **alamat** IP Privat VM. Navigasi ke bilah **az104-08-vm1** , di bagian **Jaringan** , dan klik **Pengaturan jaringan**

    >**Catatan**: Anda juga dapat terhubung ke **az104-08-vm0** dan menjalankan `Invoke-WebRequest -URI http://10.80.0.5 -UseBasicParsing` untuk mengakses situs web yang dihosting di **az104-08-vm1**.

## Tugas 3: Menskalakan komputasi dan penyimpanan untuk komputer virtual Azure

Dalam tugas ini, Anda akan menskalakan komputasi untuk mesin virtual Azure dengan mengubah ukurannya dan menskalakan penyimpanannya dengan melampirkan dan mengonfigurasi disk datanya.

1. Di portal Microsoft Azure, cari dan pilih **Mesin virtual** dan, pada bilah **Mesin virtual**, klik **az104-08-vm0**.

1. Pada bilah mesin virtual **az104-08-vm0**, klik **Ukuran** dan atur ukuran mesin virtual ke **Standar DS1_v2** dan klik **Ubah ukuran**

    >**Catatan**: Pilih ukuran lain jika **DS1_v2** Standar tidak tersedia.

1. Pada bilah mesin virtual **az104-08-vm0**, klik **Disk**, Pada **Data disk** klik **+ Buat dan lampirkan disk baru**.

1. Buat disk terkelola dengan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama disk | **az104-08-vm0-datadisk-0** |
    | Jenis penyimpanan | **SSD Premium** |
    | Ukuran (GiB| **1024** |

1. Kembali ke bilah **az104-08-vm0 - Disk**, Pada **Data disk** klik **+ Buat dan lampirkan disk baru**.

1. Buat disk terkelola dengan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya) dan Simpan perubahan:

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama disk | **az104-08-vm0-datadisk-1** |
    | Jenis penyimpanan | **SSD Premium** |
    | Ukuran (GiB)| **1024 GiB** |

1. Kembali ke bilah **az104-08-vm0 - Disk**, klik **Simpan**.

1. Pada bilah **az104-08-vm0**, di bagian **Operasi**, klik **Jalankan perintah**, dan dalam daftar perintah, klik **RunPowerShellScript**.

1. Pada bilah **Jalankan Skrip Perintah**, ketikkan berikut ini dan klik **Jalankan** untuk membuat drive Z: terdiri dari dua disk yang baru dipasang dengan tata letak sederhana dan penyediaan tetap:

   ```powershell
   New-StoragePool -FriendlyName storagepool1 -StorageSubsystemFriendlyName "Windows Storage*" -PhysicalDisks (Get-PhysicalDisk -CanPool $true)

   New-VirtualDisk -StoragePoolFriendlyName storagepool1 -FriendlyName virtualdisk1 -Size 64GB -ResiliencySettingName Simple -ProvisioningType Fixed

   Initialize-Disk -VirtualDisk (Get-VirtualDisk -FriendlyName virtualdisk1)

   New-Partition -DiskNumber 4 -UseMaximumSize -DriveLetter Z
   ```

    > **Catatan**: Tunggu konfirmasi bahwa perintah berhasil diselesaikan.

1. Di portal Microsoft Azure, cari dan pilih **Mesin virtual** dan, pada bilah **Mesin virtual**, klik **az104-08-vm1**.

1. Pada bilah **az104-08-vm1**, di bagian **Otomatisasi**, klik **Ekspor templat**.

1. Pada bilah **az104-08-vm1 - Ekspor templat**, klik **Sebarkan**.

1. Pada bilah **Penyebaran kustom**, klik **Edit templat**.

    >**Catatan**: Mengacuhkan pesan yang menyatakan **Grup sumber daya berada di lokasi yang tidak didukung oleh satu atau beberapa sumber daya dalam templat. Pilih grup** sumber daya yang berbeda. Pesan ini diharapkan dan dapat diabaikan dalam kasus ini.

1. Pada bilah **Edit templat**, di bagian yang menampilkan konten templat, ganti baris **30** `"vmSize": "Standard_D2s_v3"` dengan baris berikut):

   ```json
                    "vmSize": "Standard_DS1_v2"

   ```

    >**Catatan**: Bagian templat ini mendefinisikan ukuran komputer virtual Azure yang sama dengan yang Anda tentukan untuk komputer virtual pertama melalui portal Azure.

1. Pada bilah **Edit templat** , di bagian yang menampilkan konten templat, ganti baris **51** (`"dataDisks": [ ],`) dengan kode berikut :

   ```json
                    "dataDisks": [
                      {
                        "lun": 0,
                        "name": "az104-08-vm1-datadisk0",
                        "diskSizeGB": "1024",
                        "caching": "ReadOnly",
                        "createOption": "Empty"
                      },
                      {
                        "lun": 1,
                        "name": "az104-08-vm1-datadisk1",
                        "diskSizeGB": "1024",
                        "caching": "ReadOnly",
                        "createOption": "Empty"
                      }
                    ],
   ```

    >**Catatan**: Jika Anda menggunakan alat yang menempelkan kode secara sejalan menurut baris intellisense dapat menambahkan tanda kurung ekstra yang menyebabkan kesalahan validasi. Anda mungkin ingin menempelkan kode ke notepad terlebih dahulu lalu menempelkannya ke baris 49.

    >**Catatan**: Bagian templat ini membuat dua disk terkelola dan melampirkannya ke **az104-08-vm1**, mirip dengan konfigurasi penyimpanan komputer virtual pertama melalui portal Azure.


1. Klik **Simpan** dan, kembali pada bilah **Penyebaran kustom**, klik **Tinjau + Buat** dan, pada bilah **Tinjau + Buat**, klik **Buat**.

    >**Catatan**: Tunggu hingga penyebaran templat selesai. Anda dapat memantau progresnya dari bilah **Disk** mesin virtual **az104-08-vm1**. Operasi ini tidak akan lebih dari 3 menit.

1. Kembali ke bilah **az104-08-vm1**, di bagian **Operasi**, klik **Jalankan perintah**, dan dalam daftar perintah, klik **RunPowerShellScript**.

1. Pada bilah **Jalankan Skrip Perintah**, ketikkan berikut ini dan klik **Jalankan** untuk membuat drive Z: terdiri dari dua disk yang baru dipasang dengan tata letak sederhana dan penyediaan tetap:

   ```powershell
   New-StoragePool -FriendlyName storagepool1 -StorageSubsystemFriendlyName "Windows Storage*" -PhysicalDisks (Get-PhysicalDisk -CanPool $true)

   New-VirtualDisk -StoragePoolFriendlyName storagepool1 -FriendlyName virtualdisk1 -Size 2046GB -ResiliencySettingName Simple -ProvisioningType Fixed

   Initialize-Disk -VirtualDisk (Get-VirtualDisk -FriendlyName virtualdisk1)

   New-Partition -DiskNumber 4 -UseMaximumSize -DriveLetter Z
   ```

    > **Catatan**: Tunggu konfirmasi bahwa perintah berhasil diselesaikan.

## Tugas 4: Mendaftarkan penyedia sumber daya Microsoft.Insights dan Microsoft.AlertsManagement

1. Di portal Microsoft Azure, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**.

    >**Catatan**: Jika ini adalah pertama kalinya Anda memulai **Cloud Shell** dan Anda disajikan dengan **pesan Anda tidak memiliki penyimpanan yang dipasang** , pilih langganan yang Anda gunakan di lab ini, dan klik **Buat penyimpanan**.

1. Dari panel Cloud Shell, jalankan perintah berikut untuk mendaftarkan penyedia sumber daya Microsoft.Insights dan Microsoft.AlertsManagement.

   ```powershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.Insights

   Register-AzResourceProvider -ProviderNamespace Microsoft.AlertsManagement
   ```

## Tugas 5: Menyebarkan set skala komputer virtual Azure yang tangguh zona dengan menggunakan portal Azure

Dalam tugas ini, Anda akan menggunakan skala mesin virtual Azure yang ditetapkan di seluruh zona ketersediaan menggunakan portal Microsoft Azure.

1. Di portal Microsoft Azure, cari dan pilih **Kumpulan skala mesin virtual** dan, pada bilah **Kumpulan skala mesin virtual**, klik **+ Tambahkan** (atau **+ Buat**).

1. Pada tab **Dasar dari bilah **Buat set** skala komputer virtual, tentukan pengaturan berikut (biarkan orang lain dengan nilai defaultnya) dan klik **Berikutnya : Disk >****:

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | nama grup sumber daya baru **az104-08-rg02** |
    | Nama set skala komputer virtual | **az10408vmss0** |
    | Wilayah | pilih salah satu wilayah yang mendukung zona ketersediaan dan tempat Anda dapat menyediakan mesin virtual Azure yang berbeda dari yang Anda gunakan untuk menyebarkan mesin virtual sebelumnya di lab ini |
    | Zona ketersediaan | **Zona 1, 2, 3** |
    | Mode Orkestrasi | **Seragam** |
    | Gambar | **Pusat Data Windows Server 2019 - Gen2** |
    | Jalankan dengan diskon Azure Spot | **Tidak** |
    | Ukuran | **D2s_v3 standar** |
    | Nama Pengguna | **Mahasiswa** |
    | Kata sandi | **Berikan kata sandi yang aman**  |
    | Sudah memiliki lisensi Windows Server? | **Tidak Dicentang** |

    >**Catatan**: Untuk daftar wilayah Azure yang mendukung penyebaran komputer virtual Windows ke zona ketersediaan, lihat [Apa itu Zona Ketersediaan di Azure?](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview)

1. Pada tab **Disk dari bilah **Buat set** skala komputer virtual, terima nilai default dan klik **Berikutnya : Jaringan >****.

1. Pada tab **Jaringan** dari bilah **Buat set** skala komputer virtual, klik **tautan Buat jaringan** virtual di bawah **kotak teks Jaringan** virtual dan buat jaringan virtual baru dengan pengaturan berikut (biarkan orang lain dengan nilai defaultnya). 

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **az104-08-rg02-vnet** |
    | Rentang alamat | **10.82.0.0/20** |
    | Nama subnet | **subnet0** |
    | Rentang subnet | **10.82.0.0/24** |

    >**Catatan**: Setelah Anda membuat jaringan virtual baru dan kembali ke **tab Jaringan** dari **bilah Buat set** skala komputer virtual, **nilai Jaringan** virtual akan secara otomatis diatur ke **az104-08-rg02-vnet**.

1. Kembali ke tab **Jaringan** pada bilah **Buat kumpulan skala mesin virtual**, klik ikon **Edit antarmuka jaringan** di sebelah kanan entri antarmuka jaringan.

1. Pada bilah **Edit antarmuka jaringan**, di bagian **grup keamanan jaringan NIC**, klik **Lanjutan** dan klik **Buat baru** pada ** Konfigurasikan daftar dropdown grup keamanan jaringan**.

1. Pada bilah **Buat grup keamanan jaringan**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **az10408vmss0-nsg** |

1. Klik **Tambahkan aturan masuk** dan tambahkan aturan keamanan masuk dengan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Sumber | **Mana pun** |
    | Source port ranges | **\*** |
    | Tujuan | **Mana pun** |
    | Rentang port tujuan | **80** |
    | Protokol | **TCP** |
    | Tindakan
           | **Izinkan** |
    | Prioritas | **1010** |
    | Nama | **custom-allow-http** |

1. Klik **Tambahkan** dan, kembali pada bilah **Buat grup keamanan jaringan**, klik **OK**.

1. Kembali ke bilah **Edit antarmuka jaringan**, di bagian **Alamat IP Publik**, klik **Aktif** dan klik **OK**.

1. Kembali ke tab **Jaringan** dari bilah **Buat set** skala komputer virtual, di bawah bagian **Penyeimbangan** beban, tentukan yang berikut ini (biarkan yang lain dengan nilai defaultnya).

    | Pengaturan | Nilai |
    | --- | --- |
    | Opsi penyeimbangan muatan | **Penyeimbang beban Azure** |
    | Pilih load balancer | **Membuat penyeimbang beban** |
    
1.  Pada halaman **Buat load balancer** , tentukan nama load balancer dan ambil default. Klik **Buat** setelah selesai, lalu **Berikutnya : Penskalakan >**.
    
    | Pengaturan | Nilai |
    | --- | --- |
    | Nama load balancer | **az10408vmss0-lb** |

1. Pada tab **Penskalaan** dari bilah **Buat set** skala komputer virtual, tentukan pengaturan berikut (biarkan orang lain dengan nilai defaultnya) dan klik **Berikutnya : Manajemen >**:

    | Pengaturan | Nilai |
    | --- | --- |
    | Jumlah instans awal | **2** |
    | Kebijakan penskalaan | **Manual** |

1. Pada tab **Manajemen** pada bilah **Buat kumpulan skala mesin virtual**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Diagnostik boot | **Aktifkan dengan akun penyimpanan kustom** |
    | Akun penyimpanan diagnostik | menerima nilai default |

    >**Catatan**: Anda akan memerlukan nama akun penyimpanan ini di tugas berikutnya.

   Klik **Berikutnya : >** kesehatan:

1. Pada tab **Kesehatan** dari bilah **Buat set** skala komputer virtual, tinjau pengaturan default tanpa membuat perubahan apa pun dan klik **Berikutnya : >** Tingkat Lanjut.

1. Pada tab **Lanjutan** pada bilah **Buat kumpulan skala mesin virtual**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya) dan klik **Tinjau + buat**.

    | Pengaturan | Nilai |
    | --- | --- |
    | Algoritma penyebaran | **Penyebaran tetap (tidak disarankan dengan zona)** |

    >**Catatan**: **Pengaturan Penyebaran** maks saat ini tidak berfungsi.

1. Pada tab **Tinjau + buat** bilah **Buat kumpulan skala mesin virtual**, pastikan validasi lulus dan klik **Buat**.

    >**Catatan**: Tunggu hingga penyebaran set skala komputer virtual selesai. Proses ini memerlukan waktu sekitar 5 menit.

## Tugas 6: Mengonfigurasi set skala komputer virtual Azure dengan menggunakan ekstensi komputer virtual

Dalam tugas ini, Anda akan menginstal peran Windows Server Web Server pada contoh skala mesin virtual Azure yang Anda gunakan di tugas sebelumnya menggunakan ekstensi mesin virtual skrip kustom.

1. Di portal Microsoft Azure, cari dan pilih **Akun penyimpanan** dan, pada bilah **Akun penyimpanan**, klik entri yang mewakili akun penyimpanan diagnostik yang Anda buat di tugas sebelumnya.

1. Pada bilah akun penyimpanan, di bagian **Penyimpanan Data**, klik **Kontainer**, lalu klik **+ Kontainer**.

1. Pada bilah **Kontainer baru**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya) dan klik **Buat**:

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **skrip** |
    | Tingkat akses publik | **Pribadi (tidak ada akses anonim**) |

1. Kembali pada bilah akun penyimpanan yang menampilkan daftar kontainer, klik **skrip**.

1. Pada bilah **skrip**, klik **Unggah**.

1. Pada bilah **Unggah blob**, klik ikon folder, di kotak dialog **Buka**, navigasikan ke folder **\\Allfiles\\Labs\\08**, pilih **az104-08-install_IIS.ps1**, klik **Buka**, dan kembali ke bilah **Unggah blob**, klik **Unggah**.

1. Di portal Microsoft Azure, navigasikan kembali ke bilah **Kumpulan skala mesin virtual** dan klik **az10408vmss0**.

1. Pada bilah **az10408vmss0**, di bagian **Pengaturan**, klik **Ekstensi dan aplikasi**, dan klik **+ Tambahkan**.

1. Pada bilah **Sumber daya baru**, klik **Ekstensi Skrip Khusus**, lalu klik **Berikutnya**.

1. Dari bilah **Instal ekstensi**, **Telusuri** ke dan **Pilih** skrip **az104-08-install_IIS.ps1** yang diunggah ke kontainer **skrip** di akun penyimpanan sebelumnya dalam tugas ini, lalu klik **Buat**.

    >**Catatan**: Tunggu hingga penginstalan ekstensi selesai sebelum melanjutkan ke langkah berikutnya.

1. Di bagian **Pengaturan** pada bilah **az10408vmss0**, klik **Instans**, beri centang kotak di samping dua instans kumpulan skala mesin virtual, klik **Tingkatkan**, lalu saat diminta konfirmasi, klik **Ya**.

    >**Catatan**: Tunggu hingga peningkatan selesai sebelum melanjutkan ke langkah berikutnya.

1. Di portal Microsoft Azure, cari dan pilih **Penyeimbang muatan** dan, dalam daftar penyeimbang muatan, klik **az10408vmss0-lb**.

1. Pada bilah **az10408vmss0-lb**, catat nilai **Alamat IP Publik** yang ditetapkan ke frontend penyeimbang beban, buka tab browser baru, dan navigasikan ke alamat IP tersebut.

    >**Catatan**: Verifikasi bahwa halaman browser menampilkan nama salah satu instans set **skala komputer virtual Azure az10408vmss0**.

## Tugas 7: Menskalakan komputasi dan penyimpanan untuk set skala komputer virtual Azure

Dalam tugas ini, Anda akan mengubah ukuran instans kumpulan skala mesin virtual, mengonfigurasi pengaturan penskalaan otomatisnya, dan melampirkan disk ke dalamnya.

1. Di portal Microsoft Azure, cari dan pilih **kumpulan skala mesin virtual** dan pilih kumpulan skala **az10408vmss0**

1. Di bilah **az10408vmss0**, di bagian **Pengaturan**, klik **Ukuran**.

1. Dalam daftar ukuran yang tersedia, pilih **DS1_v2 Standar** dan klik **Ubah ukuran**.

1. Di bagian **Pengaturan**, klik **Instans**, beri centang kotak di samping dua instans kumpulan skala mesin virtual, klik **Tingkatkan**, lalu saat diminta konfirmasi, klik **Ya**.

1. Dalam daftar instans, klik entri yang mewakili instans pertama dan pada bilah instans kumpulan skala, catat **Lokasi**-nya (harus menjadi salah satu zona di wilayah target Azure tempat Anda menyebarkan kumpulan skala mesin virtual Azure).

1. Kembali ke bilah **az10408vmss0 - Instans**, klik entri yang mewakili instans kedua dan pada bilah instans kumpulan skala, catat **Lokasi**-nya (harus salah satu dari dua zona lainnya di wilayah target Azure tempat Anda menyebarkan kumpulan skala mesin virtual Azure).

1. Kembali ke bilah **az10408vmss0 - Instans**, dan di bagian **Pengaturan**, klik **Penskalaan**.

1. Pada bilah **az10408vmss0 - Penskalaan**, pilih opsi **Skala otomatis kustom** dan konfigurasikan penskalaan otomatis dengan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- |--- |
    | Mode skala | **Menskalakan berdasarkan metrik** |

1. Klik tautan **+ Tambahkan aturan** dan, pada bilah **Aturan skala**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- |--- |
    | Sumber metrik | **Sumber daya saat ini (az10480vmss0)** |
    | Agregasi waktu | **Tengah** |
    | Namespace metrik | **Host Mesin Virtual** |
    | Nama metrik | **Total Jaringan** |
    | Operator | **Lebih besar dari** |
    | Ambang batas metrik untuk memicu tindakan penskalaan | **10** |
    | Durasi (dalam menit) | **1** |
    | Statistik butir waktu | **Tengah** |
    | Operasi | **Tingkatkan jumlah sebesar** |
    | Jumlah Instans | **1** |
    | Pendinginan (menit) | **5**
           |

    >**Catatan**: Jelas nilai-nilai ini tidak mewakili konfigurasi yang realistis, karena tujuannya adalah untuk memicu autoscaling sesegera mungkin, tanpa periode tunggu yang diperpanjang.

1. Klik **Tambahkan** dan, kembali pada bilah **az10408vmss0 - Penskalaan**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- |--- |
    | Batas instans Minimum | **1** |
    | Batas instans Maksimum | **3**
           |
    | Batas instans Default | **1** |

1. Klik **Simpan**.

1. Di portal Microsoft Azure, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**.

1. Dari panel Cloud Shell, jalankan perintah berikut untuk mengidentifikasi alamat IP publik penyeimbang beban di depan kumpulan skala mesin virtual Azure **az10408vmss0**.

   ```powershell
   $rgName = 'az104-08-rg02'

   $lbpipName = 'az10408vmss0-lb-publicip'

   $pip = (Get-AzPublicIpAddress -ResourceGroupName $rgName -Name $lbpipName).IpAddress
   ```

1. Dari panel Cloud Shell, jalankan perintah berikut untuk memulai perulangan tidak terbatas yang mengirimkan permintaan HTTP ke situs web yang dihosting pada instans kumpulan skala mesin virtual Azure **az10408vmss0**.

   ```powershell
   while ($true) { Invoke-WebRequest -Uri "http://$pip" }
   ```

1. Kecilkan panel Cloud Shell tetapi jangan tutup, alihkan kembali ke bilah **az10408vmss0 - Instans** dan pantau jumlah instans.

    >**Catatan**: Anda mungkin perlu menunggu beberapa menit dan klik **Refresh**.

1. Setelah instans ketiga disediakan, navigasikan ke bilahnya untuk menentukan **Lokasi** (lokasinya harus berbeda dari dua zona pertama yang Anda identifikasi sebelumnya dalam tugas ini.

1. Tutup panel Cloud Shell.

1. Pada bilah **az10408vmss0**, di bagian **Pengaturan**, klik **Disk**, klik **+ Buat dan lampirkan disk baru**, dan lampirkan disk baru disk yang dikelola dengan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | LUN | **0** |
    | Jenis penyimpanan | **HDD Standar** |
    | Ukuran (GiB) | **32** |

1. Simpan perubahan, di bagian **Pengaturan** pada bilah **az10408vmss0**, klik **Instans**, pilih kotak centang di sebelah instans kumpulan skala mesin virtual, klik **Tingkatkan**, lalu saat dimintai konfirmasi, klik **Ya**.

    >**Catatan**: Disk yang terpasang pada langkah sebelumnya adalah disk mentah. Sebelum dapat digunakan, Anda perlu membuat partisi, membuat sistem file, dan memasangnya. Agar dapat melakukannya, Anda akan menggunakan ekstensi Skrip Kustom mesin virtual Azure. Pertama, Anda harus menghapus Ekstensi Skrip Kustom yang ada.

1. Di bagian **Pengaturan** blade **az10408vmss0**, klik **Ekstensi dan aplikasi**, klik **CustomScriptExtension**, lalu klik **Uninstall**.

    >**Catatan**: Tunggu hingga penghapusan instalasi selesai.

1. Di portal Microsoft Azure, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**.

1. Di toolbar panel Cloud Shell, klik ikon **Unggah/Unduh file**, di menu dropdown, klik **Unggah** dan unggah file **\\Allfiles\\Labs\\08\\az104-08-configure_VMSS_disks.ps1** ke dalam direktori beranda Cloud Shell.

1. Dari panel Cloud Shell, jalankan perintah berikut untuk menampilkan konten skrip:

   ```powershell
   Set-Location -Path $HOME

   Get-Content -Path ./az104-08-configure_VMSS_disks.ps1
   ```

    >**Catatan**: Skrip menginstal ekstensi skrip kustom yang mengonfigurasi disk yang terpasang.

1. Dari panel Cloud Shell, jalankan yang berikut ini untuk menjalankan skrip dan mengonfigurasi disk set skala komputer virtual Azure:

   ```powershell
   ./az104-08-configure_VMSS_disks.ps1
   ```

1. Tutup panel Cloud Shell.

1. Di bagian **Pengaturan** pada bilah **az10408vmss0**, klik **Instans**, beri centang kotak di samping instans kumpulan skala mesin virtual, klik **Tingkatkan**, lalu saat diminta konfirmasi, klik **Ya**.

## Membersihkan sumber daya

>**Catatan**: Ingatlah untuk menghapus sumber daya Azure yang baru dibuat yang tidak lagi Anda gunakan. Dengan menghapus sumber daya yang tidak digunakan, Anda tidak akan melihat biaya yang tak terduga.

>**Catatan**: Jangan khawatir jika sumber daya lab tidak dapat segera dihapus. Terkadang sumber daya memiliki dependensi dan membutuhkan waktu lebih lama untuk dihapus. Ini adalah tugas Administrator yang umum untuk memantau penggunaan sumber daya, jadi tinjau sumber daya Anda secara berkala di Portal untuk melihat bagaimana pembersihannya. 
1. Di portal Azure, buka sesi **PowerShell** dalam panel **Cloud Shell**.

1. Hapus az104-08-configure_VMSS_disks.ps1 dengan menjalankan perintah berikut:

   ```powershell
   rm ~\az104-08*
   ```

1. Buat daftar semua grup sumber daya yang dibuat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-08*'
   ```

1. Hapus semua grup sumber daya yang Anda buat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-08*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Catatan**: Perintah dijalankan secara asinkron (seperti yang ditentukan oleh parameter -AsJob), jadi sementara Anda akan dapat menjalankan perintah PowerShell lain segera setelah itu dalam sesi PowerShell yang sama, akan memakan waktu beberapa menit sebelum grup sumber daya benar-benar dihapus.

## Tinjau

Di lab ini, Anda telah:

+ Menyebarkan mesin virtual Azure dalam zona yang tahan menggunakan portal Azure dan templat Azure Resource Manager
+ Mesin virtual Azure yang dikonfigurasi menggunakan ekstensi mesin virtual
+ Komputasi dan penyimpanan berskala untuk mesin virtual Azure
+ Menyebarkan kumpulan skala mesin virtual Azure dalam zona yang tahan menggunakan portal Azure
+ Kumpulan skala mesin virtual Azure yang dikonfigurasi menggunakan ekstensi mesin virtual
+ Komputasi dan penyimpanan yang diskalakan untuk kumpulan skala mesin virtual Azure
