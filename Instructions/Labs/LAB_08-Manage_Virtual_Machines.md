---
lab:
  title: 08 - Mengelola Mesin Virtual
  module: Module 08 - Virtual Machines
ms.openlocfilehash: a710fbaf4ea888651012bad592d34667a190c68f
ms.sourcegitcommit: 6df80c7697689bcee3616cdd665da0a38cdce6cb
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 06/26/2022
ms.locfileid: "146587448"
---
# <a name="lab-08---manage-virtual-machines"></a>Lab 08 - Mengelola Mesin Virtual
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Anda ditugaskan untuk mengidentifikasi berbagai opsi untuk menyebarkan dan mengonfigurasi mesin virtual Azure. Pertama, Anda perlu menentukan opsi ketahanan dan skalabilitas komputasi dan penyimpanan yang berbeda yang dapat Anda terapkan saat menggunakan mesin virtual Azure. Selanjutnya, Anda perlu menyelidiki opsi ketahanan dan skalabilitas komputasi dan penyimpanan yang tersedia saat menggunakan kumpulan skala mesin virtual Azure. Anda juga ingin menjelajahi kemampuan untuk secara otomatis mengonfigurasi mesin virtual dan kumpulan skala mesin virtual menggunakan ekstensi Skrip Kustom Komputer Virtual Azure.

## <a name="objectives"></a>Tujuan

Di lab ini Anda akan:

+ Tugas 1: Sebarkan mesin virtual Azure dalam zona yang tahan menggunakan portal Azure dan templat Azure Resource Manager
+ Tugas 2: Konfigurasikan mesin virtual Azure menggunakan ekstensi mesin virtual
+ Tugas 3: Skalakan komputasi dan penyimpanan untuk mesin virtual Azure
+ Tugas 4: Daftarkan penyedia sumber daya Microsoft.Insights dan Microsoft.AlertsManagement
+ Tugas 5: Sebarkan kumpulan skala mesin virtual Azure dalam zona yang tahan menggunakan portal Microsoft Azure
+ Tugas 6: Konfigurasikan kumpulan skala mesin virtual Azure menggunakan ekstensi mesin virtual
+ Tugas 7: Skala komputasi dan penyimpanan untuk kumpulan skala mesin virtual Azure (opsional)

## <a name="estimated-timing-50-minutes"></a>Perkiraan waktu: 50 menit

## <a name="architecture-diagram"></a>Diagram arsitektur

![gambar](../media/lab08.png)


## <a name="instructions"></a>Instruksi

### <a name="exercise-1"></a>Latihan 1

#### <a name="task-1-deploy-zone-resilient-azure-virtual-machines-by-using-the-azure-portal-and-an-azure-resource-manager-template"></a>Tugas 1: Sebarkan mesin virtual Azure dalam zona yang tahan menggunakan portal Azure dan templat Azure Resource Manager

Dalam tugas ini, Anda akan menyebarkan mesin virtual Azure ke zona ketersediaan yang berbeda menggunakan portal Azure dan templat Azure Resource Manager.

1. Masuk ke [portal Microsoft Azure](http://portal.azure.com).

1. Di portal Microsoft Azure, cari dan pilih **Mesin virtual** dan, pada panel **Mesin virtual**, klik **+ Buat**, klik **+ Mesin virtual Azure**.

1. Pada tab **Dasar** pada panel **Buat mesin virtual**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang akan Anda gunakan di lab ini |
    | Grup sumber daya | nama grup sumber daya baru **az104-08-rg01** |
    | Nama mesin virtual | **az104-08-vm0** |
    | Wilayah | pilih salah satu wilayah yang mendukung zona ketersediaan dan lokasi Anda dapat menyediakan mesin virtual Azure |
    | Opsi ketersediaan | **Zona ketersediaan** |
    | Zona ketersediaan | **Zona 1** |
    | Gambar | **Pusta Data Windows Server 2019 - Gen1/Gen2** |
    | Instans Azure Spot | **Tidak** |
    | Ukuran | **Standar D2s v3** |
    | Nama Pengguna | **Siswa** |
    | Kata sandi | **Berikan kata sandi yang aman** |
    | Port masuk publik | **Tidak ada** |
    | Apakah Anda ingin menggunakan lisensi Windows Server yang sudah ada? | **Tidak Dicentang** |

1. Klik **Berikutnya: Disk >** dan, pada tab **Disk** bilah **Buat mesin virtual**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Jenis disk OS | **SSD Premium** |
    | Mengaktifkan kompatibilitas Ultra Disk | **Tidak Dicentang** |

1. Klik **Berikutnya: Jaringan >** dan, pada tab **Jaringan** bilah **Buat mesin virtual**, klik **Buat baru** pada **Jaringan virtual** kotak teks.

1. Pada bilah **Buat jaringan virtual**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **az104-08-rg01-vnet** |
    | Rentang alamat | **10.80.0.0/20** |
    | Nama subnet | **subnet0** |
    | Rentang subnet | **10.80.0.0/24** |

1. Klik **OK** dan, kembali ke tab **Jaringan** bilah **Buat mesin virtual**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Subnet | **subnet0** |
    | IP Publik | **default** |
    | Grup keamanan jaringan NIC | **dasar** |
    | Port masuk publik | **Tidak ada** |
    | Jaringan Dipercepat | **Nonaktif**
    | Tempatkan mesin virtual ini di belakang solusi penyeimbang muatan yang ada? | **Tidak Dicentang** |

1. Klik **Berikutnya: Manajemen >** dan, pada tab **Manajemen** bilah **Buat mesin virtual**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Diagnostik boot | **Aktifkan dengan akun penyimpanan kustom** |
    | Akun penyimpanan diagnostik | menerima nilai default |
    | Opsi orkestrasi patch | **Pembaruan secara manual** |  

    >**Catatan**: Jika perlu, pilih akun penyimpanan yang ada di daftar menurun atau buat akun penyimpanan baru. Catat nama akun penyimpanan. Anda akan menggunakannya dalam tugas berikutnya.

1. Klik **Berikutnya: Lanjutan >** , pada tab **Lanjutan** bilah **Buat mesin virtual**, tinjau pengaturan yang tersedia tanpa mengubahnya, dan klik **Tinjauan + Buat**.

1. Pada bilah **Tinjauan + Buat**, klik **Buat**.

1. Pada bilah penyebaran, klik **Templat**.

1. Tinjauan templat yang menunjukkan penyebaran yang sedang berlangsung dan klik **Sebarkan**.

    >**Catatan**: Anda akan menggunakan opsi ini untuk menyebarkan mesin virtual kedua dengan konfigurasi yang cocok kecuali untuk zona ketersediaan.

1. Pada bilah **Penyebaran kustom**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Grup Sumber Daya | **az104-08-rg01** |
    | Nama Antarmuka Jaringan | **az104-08-vm1-nic1** |
    | Nama Alamat IP Publik | **az104-08-vm1-ip** |
    | Nama Mesin Virtual, Name1 Mesin Virtual, Nama Komputer Mesin Virtual   | **az104-08-vm1** |
    | RG Mesin Virtual | **az104-08-rg01** |    
    | Nama Pengguna Admin | **Siswa** |
    | Kata Sandi Admin | **Berikan kata sandi yang aman**  |
    | Aktifkan Hotpatching | **salah** |
    | Zona | **2** |

    >**Catatan**: Anda perlu mengubah parameter yang sesuai dengan properti dari sumber daya berbeda yang Anda gunakan menggunakan templat, termasuk mesin virtual dan antarmuka jaringannya.

1. Klik **Tinjauan + Buat**, pada bilah **Tinjauan + Buat**, klik **Buat**.

    >**Catatan**: Tunggu hingga kedua penyebaran selesai sebelum Anda melanjutkan ke tugas berikutnya. Proses ini mungkin perlu waktu sekitar 5 menit.

#### <a name="task-2-configure-azure-virtual-machines-by-using-virtual-machine-extensions"></a>Tugas 2: Konfigurasikan mesin virtual Azure menggunakan ekstensi mesin virtual

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

1. Kembali pada bilah **Pasang ekstensi**, klik **Tinjauan + buat** dan, pada bilah **Tinjauan + buat** klik **Buat**.

1. Di portal Microsoft Azure, cari dan pilih **Mesin virtual** dan, pada bilah **Mesin virtual**, klik **az104-08-vm1**.

1. Pada bilah **az104-08-vm1**, di bagian **Otomatisasi**, klik **Ekspor templat**.

1. Pada bilah **az104-08-vm1 - Ekspor templat**, klik **Sebarkan**.

1. Pada bilah **Penyebaran kustom**, klik **Edit templat**.

    >**Catatan**: Abaikan pesan yang menyatakan **Grup sumber daya berada di lokasi yang tidak didukung oleh satu atau beberapa sumber daya di templat. Harap pilih grup sumber daya yang lain**. Pesan ini diharapkan dan dapat diabaikan dalam kasus ini.

1. Pada bilah **Edit templat**, di bagian yang menampilkan konten templat, masukkan kode berikut yang dimulai dengan baris **20** (tepat di bawah baris `"resources": [`):

   >**Catatan**: Jika Anda menggunakan alat yang menempelkan kode baris demi baris, intellisense dapat menambahkan tanda kurung tambahan yang menyebabkan kesalahan validasi. Anda mungkin ingin menempelkan kode ke notepad terlebih dahulu lalu menempelkannya ke baris 20.

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

   >**Catatan**: Bagian templat ini menentukan ekstensi skrip kustom mesin virtual Azure yang sama yang Anda terapkan sebelumnya ke mesin virtual pertama melalui Azure PowerShell.

1. Klik **Simpan** dan, kembali pada bilah **Templat kustom**, klik **Tinjauan + Buat** dan, pada bilah **Tinjauan + Buat**, klik **Buat**

    >**Catatan**: Tunggu hingga penyebaran templat selesai. Anda dapat memantau progresnya dari bilah **Ekstensi** pada mesin virtual **az104-08-vm0** dan **az104-08-vm1**. Operasi ini tidak akan lebih dari 3 menit.

1. Untuk memastikan bahwa konfigurasi berbasis ekstensi Skrip Kustom berhasil, navigasikan kembali pada bilah **az104-08-vm1**, di bagian **Operasi**, klik **Jalankan perintah**, dan dalam daftar perintah, klik **RunPowerShellScript**.

1. Pada bilah **Jalankan Skrip Perintah**, ketikkan berikut ini dan klik **Jalankan** untuk mengakses situs web yang di-hosting di **az104-08-vm0**:

   ```powershell
   Invoke-WebRequest -URI http://10.80.0.4 -UseBasicParsing
   ```

    >**Catatan**: Parameter **-UseBasicParsing** diperlukan untuk menghilangkan ketergantungan pada Internet Explorer untuk menyelesaikan eksekusi cmdlet

    >**Catatan**: Anda juga dapat terhubung ke **az104-08-vm0** dan menjalankan `Invoke-WebRequest -URI http://10.80.0.5 -UseBasicParsing` untuk mengakses situs web yang di-hosting di **az104-08-vm1**.

#### <a name="task-3-scale-compute-and-storage-for-azure-virtual-machines"></a>Tugas 3: Skalakan komputasi dan penyimpanan untuk mesin virtual Azure

Dalam tugas ini, Anda akan menskalakan komputasi untuk mesin virtual Azure dengan mengubah ukurannya dan menskalakan penyimpanannya dengan melampirkan dan mengonfigurasi disk datanya.

1. Di portal Microsoft Azure, cari dan pilih **Mesin virtual** dan, pada bilah **Mesin virtual**, klik **az104-08-vm0**.

1. Pada bilah mesin virtual **az104-08-vm0**, klik **Ukuran** dan atur ukuran mesin virtual ke **Standar DS1_v2** dan klik **Ubah ukuran**

    >**Catatan**: Pilih ukuran lain jika **Standar DS1_v2** tidak tersedia.

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

   New-VirtualDisk -StoragePoolFriendlyName storagepool1 -FriendlyName virtualdisk1 -Size 2046GB -ResiliencySettingName Simple -ProvisioningType Fixed

   Initialize-Disk -VirtualDisk (Get-VirtualDisk -FriendlyName virtualdisk1)

   New-Partition -DiskNumber 4 -UseMaximumSize -DriveLetter Z
   ```

    > **Catatan**: Tunggu konfirmasi bahwa perintah berhasil diselesaikan.

1. Di portal Microsoft Azure, cari dan pilih **Mesin virtual** dan, pada bilah **Mesin virtual**, klik **az104-08-vm1**.

1. Pada bilah **az104-08-vm1**, di bagian **Otomatisasi**, klik **Ekspor templat**.

1. Pada bilah **az104-08-vm1 - Ekspor templat**, klik **Sebarkan**.

1. Pada bilah **Penyebaran kustom**, klik **Edit templat**.

    >**Catatan**: Abaikan pesan yang menyatakan **Grup sumber daya berada di lokasi yang tidak didukung oleh satu atau beberapa sumber daya di templat. Harap pilih grup sumber daya yang lain**. Pesan ini diharapkan dan dapat diabaikan dalam kasus ini.

1. Pada bilah **Edit templat**, di bagian yang menampilkan konten templat, ganti baris **30** `"vmSize": "Standard_D2s_v3"` dengan baris berikut):

   ```json
                    "vmSize": "Standard_DS1_v2"

   ```

    >**Catatan**: Bagian templat ini menentukan ukuran mesin virtual Azure yang sama dengan yang Anda tentukan untuk mesin virtual pertama melalui portal Azure.

1. Pada bilah **Edit templat**, di bagian yang menampilkan isi templat, ganti baris **51** (baris `"dataDisks": [ ]`) dengan kode berikut :

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
                    ]
   ```

    >**Catatan**: Jika Anda menggunakan alat yang menempelkan kode baris demi baris, intellisense dapat menambahkan tanda kurung tambahan yang menyebabkan kesalahan validasi. Anda mungkin ingin menempelkan kode ke notepad terlebih dahulu lalu menempelkannya ke baris 49.

    >**Catatan**: Bagian templat ini membuat dua disk terkelola dan melampirkannya ke **az104-08-vm1**, serupa dengan konfigurasi penyimpanan mesin virtual pertama melalui portal Azure.


1. Klik **Simpan** dan, kembali pada bilah **Penyebaran kustom**, klik **Tinjauan + Buat** dan, pada bilah **Tinjauan + Buat**, klik **Buat**.

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

#### <a name="task-4-register-the-microsoftinsights-and-microsoftalertsmanagement-resource-providers"></a>Tugas 4: Daftarkan penyedia sumber daya Microsoft.Insights dan Microsoft.AlertsManagement

1. Di portal Microsoft Azure, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**.

    >**Catatan**: Jika ini pertama kalinya Anda memulai **Cloud Shell** dan Anda melihat pesan **Anda tidak memiliki penyimpanan yang terpasang**, pilih langganan yang Anda gunakan di lab ini, dan klik **Buat penyimpanan**.

1. Dari panel Cloud Shell, jalankan perintah berikut untuk mendaftarkan penyedia sumber daya Microsoft.Insights dan Microsoft.AlertsManagement.

   ```powershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.Insights

   Register-AzResourceProvider -ProviderNamespace Microsoft.AlertsManagement
   ```

#### <a name="task-5-deploy-zone-resilient-azure-virtual-machine-scale-sets-by-using-the-azure-portal"></a>Tugas 5: Sebarkan kumpulan skala mesin virtual Azure dalam zona yang tahan menggunakan portal Microsoft Azure

Dalam tugas ini, Anda akan menggunakan skala mesin virtual Azure yang ditetapkan di seluruh zona ketersediaan menggunakan portal Microsoft Azure.

1. Di portal Microsoft Azure, cari dan pilih **Kumpulan skala mesin virtual** dan, pada bilah **Kumpulan skala mesin virtual**, klik **+ Tambahkan** (atau **+ Buat**).

1. Pada tab **Dasar** bilah **Buat kumpulan skala mesin virtual**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya) dan klik **Berikutnya : Disk >** :

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | nama grup sumber daya baru **az104-08-rg02** |
    | Nama set skala mesin virtual | **az10408vmss0** |
    | Wilayah | pilih salah satu wilayah yang mendukung zona ketersediaan dan tempat Anda dapat menyediakan mesin virtual Azure yang berbeda dari yang Anda gunakan untuk menyebarkan mesin virtual sebelumnya di lab ini |
    | Zona ketersediaan | **Zona 1, 2, 3** |
    | Gambar | **Pusat Data Windows Server 2019 - Gen2** |
    | Instans Azure Spot | **Tidak** |
    | Ukuran | **D2s_v3 standar** |
    | Nama Pengguna | **Siswa** |
    | Kata sandi | **Berikan kata sandi yang aman**  |
    | Sudah memiliki lisensi Windows Server? | **Tidak Dicentang** |

    >**Catatan**: Untuk daftar wilayah Azure yang mendukung penyebaran mesin virtual Windows ke zona ketersediaan, baca [Apa itu Zona Ketersediaan di Azure?](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview)

1. Pada tab **Disk** pada bilah **Buat kumpulan skala mesin virtual**, terima nilai default dan klik **Berikutnya : Jaringan >** .

1. Pada tab **Jaringan** pada bilah **Buat kumpulan skala mesin virtual**, klik tautan **Buat jaringan virtual** pada kotak teks **Jaringan virtual** dan buat jaringan virtual baru dengan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **az104-08-rg02-vnet** |
    | Rentang alamat | **10.82.0.0/20** |
    | Nama subnet | **subnet0** |
    | Rentang subnet | **10.82.0.0/24** |

    >**Catatan**: Setelah Anda membuat jaringan virtual baru dan kembali ke tab **Jaringan** pada bilah **Buat kumpulan skala mesin virtual**, nilai **Jaringan virtual** akan secara otomatis diatur menjadi **az104-08-rg02-vnet**.

1. Kembali ke tab **Jaringan** pada bilah **Buat kumpulan skala mesin virtual**, klik ikon **Edit antarmuka jaringan** di sebelah kanan entri antarmuka jaringan.

1. Pada bilah **Edit antarmuka jaringan**, di bagian **grup keamanan jaringan NIC**, klik **Lanjutan** dan klik **Buat baru** pada  **Konfigurasikan daftar menurun grup keamanan jaringan**.

1. Pada bilah **Buat grup keamanan jaringan**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **az10408vmss0-nsg** |

1. Klik **Tambahkan aturan masuk** dan tambahkan aturan keamanan masuk dengan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Sumber | **Mana pun** |
    | Rentang port sumber | **\*** |
    | Tujuan | **Mana pun** |
    | Rentang port tujuan | **80** |
    | Protokol | **TCP** |
    | Tindakan | **Izinkan** |
    | Prioritas | **1010** |
    | Nama | **custom-allow-http** |

1. Klik **Tambahkan** dan, kembali pada bilah **Buat grup keamanan jaringan**, klik **OK**.

1. Kembali ke bilah **Edit antarmuka jaringan**, di bagian **Alamat IP Publik**, klik **Aktif** dan klik **OK**.

1. Kembali ke tab **Jaringan** pada bilah **Buat kumpulan skala mesin virtual**, pada bagian **Penyeimbang beban**, pastikan bahwa entri **Gunakan penyeimbang beban** dipilih dan tentukan **Pengaturan penyeimbang beban** berikut (biarkan opsi yang lain dengan nilai defaultnya) dan klik **Berikutnya : Penskalaan >** :

    | Pengaturan | Nilai |
    | --- | --- |
    | Opsi load balancer | **Azure load balancer** |
    | Memilih load balancer | **(baru) az10408vmss0-lb** |
    | Memilih kumpulan backend | **(baru) bepool** |

1. Pada tab **Penskalaan** bilah **Buat kumpulan skala mesin virtual**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya) dan klik **Berikutnya: Pengelolaan >** :

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

   Klik **Berikutnya : Kondisi >** :

1. Pada tab **Kondisi** bilah **Buat kumpulan skala mesin virtual**, tinjau pengaturan default tanpa membuat perubahan apa pun dan klik **Berikutnya : Lanjutan >** .

1. Pada tab **Lanjutan** pada bilah **Buat kumpulan skala mesin virtual**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya) dan klik **Tinjauan + buat**.

    | Pengaturan | Nilai |
    | --- | --- |
    | Algoritma penyebaran | **Penyebaran tetap (tidak disarankan dengan zona)** |

    >**Catatan**: Pengaturan **Penyebaran maksimum** saat ini tidak berfungsi.

1. Pada tab **Tinjauan + buat** bilah **Buat kumpulan skala mesin virtual**, pastikan validasi lulus dan klik **Buat**.

    >**Catatan**: Tunggu hingga penyebaran kumpulan skala mesin virtual selesai. Proses ini memerlukan waktu sekitar 5 menit.

#### <a name="task-6-configure-azure-virtual-machine-scale-sets-by-using-virtual-machine-extensions"></a>Tugas 6: Konfigurasikan kumpulan skala mesin virtual Azure menggunakan ekstensi mesin virtual

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

1. Pada bilah **az10408vmss0**, di bagian **Pengaturan**, klik **Ekstensi**, dan klik **+ Tambahkan**.

1. Pada bilah **Sumber daya baru**, klik **Ekstensi Skrip Khusus**, lalu klik **Berikutnya**.

1. Dari bilah **Instal ekstensi**, **Telusuri** ke dan **Pilih** skrip **az104-08-install_IIS.ps1** yang diunggah ke kontainer **skrip** di akun penyimpanan sebelumnya dalam tugas ini, lalu klik **Buat**.

    >**Catatan**: Tunggu hingga penginstalan ekstensi selesai sebelum melanjutkan ke langkah berikutnya.

1. Di bagian **Pengaturan** pada bilah **az10408vmss0**, klik **Instans**, beri centang kotak di samping dua instans kumpulan skala mesin virtual, klik **Tingkatkan**, lalu saat diminta konfirmasi, klik **Ya**.

    >**Catatan**: Tunggu hingga pemutakhiran selesai sebelum melanjutkan ke langkah berikutnya.

1. Di portal Microsoft Azure, cari dan pilih **Penyeimbang muatan** dan, dalam daftar penyeimbang muatan, klik **az10408vmss0-lb**.

1. Pada bilah **az10408vmss0-lb**, catat nilai **Alamat IP Publik** yang ditetapkan ke frontend penyeimbang beban, buka tab browser baru, dan navigasikan ke alamat IP tersebut.

    >**Catatan**: Pastikan halaman browser menampilkan nama salah satu contoh kumpulan skala mesin virtual Azure **az10408vmss0**.

#### <a name="task-7-scale-compute-and-storage-for-azure-virtual-machine-scale-sets"></a>Tugas 7: Skala komputasi dan penyimpanan untuk kumpulan skala mesin virtual Azure

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
    | Mode skala | **Skalakan berdasarkan metrik** |

1. Klik tautan **+ Tambahkan aturan** dan, pada bilah **Aturan skala**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- |--- |
    | Sumber metrik | **Sumber daya saat ini (az10480vmss0)** |
    | Agregasi waktu | **Rata-rata** |
    | Namespace layanan metrik | **Host Komputer Virtual** |
    | Nama metrik | **Total Jaringan Masuk** |
    | Operator | **Lebih besar dari** |
    | Ambang metrik untuk memicu tindakan skala | **10** |
    | Durasi (dalam menit) | **1** |
    | Statistik butir waktu | **Rata-rata** |
    | Operasi | **Tingkatkan jumlah sebesar** |
    | Jumlah instans | **1** |
    | Pendinginan (menit) | **5** |

    >**Catatan**: Jelas nilai-nilai ini tidak mewakili konfigurasi yang realistis, karena tujuannya adalah untuk memicu penskalaan otomatis sesegera mungkin, tanpa masa tunggu yang diperpanjang.

1. Klik **Tambahkan** dan, kembali pada bilah **az10408vmss0 - Penskalaan**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- |--- |
    | Batas instans Minimum | **1** |
    | Batas instans Maksimum | **3** |
    | Batas instans Default | **1** |

1. Klik **Simpan**.

1. Di portal Microsoft Azure, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**.

1. Dari panel Cloud Shell, jalankan perintah berikut untuk mengidentifikasi alamat IP publik penyeimbang beban di depan kumpulan skala mesin virtual Azure **az10408vmss0**.

   ```powershell
   $rgName = 'az104-08-rg02'

   $lbpipName = 'az10408vmss0-ip'

   $pip = (Get-AzPublicIpAddress -ResourceGroupName $rgName -Name $lbpipName).IpAddress
   ```

1. Dari panel Cloud Shell, jalankan perintah berikut untuk memulai perulangan tidak terbatas yang mengirimkan permintaan HTTP ke situs web yang dihosting pada instans kumpulan skala mesin virtual Azure **az10408vmss0**.

   ```powershell
   while ($true) { Invoke-WebRequest -Uri "http://$pip" }
   ```

1. Kecilkan panel Cloud Shell tetapi jangan tutup, alihkan kembali ke bilah **az10408vmss0 - Instans** dan pantau jumlah instans.

    >**Catatan**: Anda mungkin perlu menunggu beberapa menit dan mengeklik **Refresh**.

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

1. Di bagian **Pengaturan** pada bilah **az10408vmss0**, klik **Ekstensi**, klik **CustomScriptExtension**, lalu klik **Hapus instalan**.

    >**Catatan**: Tunggu hingga penghapusan instalan selesai.

1. Di portal Microsoft Azure, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**.

1. Di toolbar panel Cloud Shell, klik ikon **Unggah/Unduh file**, di menu dropdown, klik **Unggah** dan unggah file **\\Allfiles\\Labs\\08\\az104-08-configure_VMSS_disks.ps1** ke dalam direktori beranda Cloud Shell.

1. Dari panel Cloud Shell, jalankan perintah berikut untuk menampilkan konten skrip:

   ```powershell
   Set-Location -Path $HOME

   Get-Content -Path ./az104-08-configure_VMSS_disks.ps1
   ```

    >**Catatan**: Skrip menginstal ekstensi skrip kustom yang mengonfigurasi disk terlampir.

1. Dari panel Cloud Shell, jalankan perintah berikut untuk menjalankan skrip dan mengonfigurasi disk kumpulan skala mesin virtual Azure:

   ```powershell
   ./az104-08-configure_VMSS_disks.ps1
   ```

1. Tutup panel Cloud Shell.

1. Di bagian **Pengaturan** pada bilah **az10408vmss0**, klik **Instans**, beri centang kotak di samping instans kumpulan skala mesin virtual, klik **Tingkatkan**, lalu saat diminta konfirmasi, klik **Ya**.

#### <a name="clean-up-resources"></a>Membersihkan sumber daya

>**Catatan**: Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak digunakan akan memastikan bahwa Anda tidak akan melihat biaya yang tidak diharapkan.

>**Catatan**:  Jangan khawatir jika sumber daya lab tidak dapat segera dihapus. Terkadang sumber daya memiliki dependensi dan membutuhkan waktu lebih lama untuk dihapus. Ini adalah tugas Administrator yang umum untuk memantau penggunaan sumber daya, jadi tinjau sumber daya Anda secara berkala di Portal untuk melihat bagaimana pembersihannya. 
1. Di portal Microsoft Azure, buka sesi **PowerShell** dalam panel **Cloud Shell**.

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

    >**Catatan**: Perintah dijalankan secara asinkron (sebagaimana yang ditentukan oleh parameter -AsJob), jadi saat Anda akan dapat menjalankan perintah PowerShell lain langsung setelahnya dalam sesi PowerShell yang sama, proses ini akan memakan waktu beberapa menit sebelum grup sumber daya benar-benar dihapus.

#### <a name="review"></a>Tinjauan

Di lab ini, Anda telah:

+ Menyebarkan mesin virtual Azure dalam zona yang tahan menggunakan portal Azure dan templat Azure Resource Manager
+ Mesin virtual Azure yang dikonfigurasi menggunakan ekstensi mesin virtual
+ Komputasi dan penyimpanan berskala untuk mesin virtual Azure
+ Menyebarkan kumpulan skala mesin virtual Azure dalam zona yang tahan menggunakan portal Azure
+ Kumpulan skala mesin virtual Azure yang dikonfigurasi menggunakan ekstensi mesin virtual
+ Komputasi dan penyimpanan yang diskalakan untuk kumpulan skala mesin virtual Azure
