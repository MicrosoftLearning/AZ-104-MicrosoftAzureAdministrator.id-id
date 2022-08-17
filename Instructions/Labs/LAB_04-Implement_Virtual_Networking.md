---
lab:
  title: 04 - Menerapkan Jaringan Virtual
  module: Module 04 - Virtual Networking
ms.openlocfilehash: 383f88f2dddb48d498efb3d868330e4bba15c92b
ms.sourcegitcommit: be14e4ff5bc638e8aee13ec4b8be29525d404028
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 05/11/2022
ms.locfileid: "145198224"
---
# <a name="lab-04---implement-virtual-networking"></a>Lab 04 - Menerapkan Jaringan Virtual

# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Anda perlu menjelajahi kemampuan jaringan virtual Azure. Untuk memulai, Anda berencana membuat jaringan virtual di Azure yang akan menghosting beberapa mesin virtual Azure. Karena Anda bermaksud menerapkan segmentasi berbasis jaringan, Anda akan menerapkannya ke subnet yang berbeda dari jaringan virtual. Anda juga ingin memastikan bahwa alamat IP pribadi dan publik mereka tidak akan berubah seiring waktu. Untuk mematuhi persyaratan keamanan Contoso, Anda perlu melindungi titik akhir publik dari mesin virtual Azure yang dapat diakses dari Internet. Terakhir, Anda perlu menerapkan resolusi nama DNS untuk mesin virtual Azure baik di dalam jaringan virtual maupun dari Internet.

## <a name="objectives"></a>Tujuan

Di lab ini Anda akan:

+ Tugas 1: Membuat dan mengonfigurasikan jaringan virtual
+ Tugas 2: Menyebarkan mesin virtual ke dalam jaringan virtual
+ Tugas 3: Mengonfigurasi alamat IP pribadi dan publik dari VM Azure
+ Tugas 4: Mengonfigurasi kelompok keamanan jaringan
+ Tugas 5: Mengonfigurasi Azure DNS untuk resolusi nama internal
+ Tugas 6: Mengonfigurasi Azure DNS untuk resolusi nama eksternal

## <a name="estimated-timing-40-minutes"></a>Perkiraan waktu: 40 menit

## <a name="architecture-diagram"></a>Diagram arsitektur

![gambar](../media/lab04.png)

## <a name="instructions"></a>Instruksi

### <a name="exercise-1"></a>Latihan 1

#### <a name="task-1-create-and-configure-a-virtual-network"></a>Tugas 1: Membuat dan mengonfigurasikan jaringan virtual

Dalam tugas ini, Anda akan membuat jaringan virtual dengan beberapa subnet menggunakan portal Microsoft Azure

1. Masuk ke [portal Microsoft Azure](https://portal.azure.com).

1. Di portal Microsoft Azure, cari dan pilih **Jaringan virtual**, dan, pada panel **Jaringan virtual**, klik **+ Buat**.

1. Buat jaringan virtual dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang akan Anda gunakan di lab ini |
    | Grup Sumber Daya | nama grup sumber daya **baru** **az104-04-rg1** |
    | Nama | **az104-04-vnet1** |
    | Wilayah | nama wilayah Azure yang tersedia dalam langganan yang akan Anda gunakan di lab ini |

1. Klik **Berikutnya : Alamat IP** dan masukkan nilai berikut

    | Pengaturan | Nilai |
    | --- | --- |
    | Ruang alamat IPv4 | **10.40.0.0/20** |

1. Klik **+ Tambahkan subnet** masukkan nilai berikut lalu klik **Tambah**

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama subnet | **subnet0** |
    | Rentang alamat subnet | **10.40.0.0/24** |

1. Terima defaultnya dan klik **Tinjauan dan Buat**. Biarkan validasi berjalan, dan tekan **Buat** lagi untuk mengirimkan penyebaran Anda.

    >**Catatan:** Tunggu hingga jaringan virtual diprovisika. Ini seharusnya memakan waktu kurang dari satu menit.

1. Klik **Buka sumber daya**

1. Pada panel jaringan virtual **az104-04-vnet1**, klik **Subnet**, lalu klik **+ Subnet**.

1. Buat subnet dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **subnet1** |
    | Rentang alamat (blok CIDR) | **10.40.1.0/24** |
    | Grup keamanan jaringan | **Tidak ada** |
    | Tabel rute | **Tidak ada** |

1. Klik **Simpan**

#### <a name="task-2-deploy-virtual-machines-into-the-virtual-network"></a>Tugas 2: Menyebarkan mesin virtual ke dalam jaringan virtual

Dalam tugas ini, Anda akan menyebarkan mesin virtual Azure ke subnet yang berbeda dari jaringan virtual dengan menggunakan template ARM

1. Di portal Microsoft Azure, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**.

    >**Catatan**: Jika ini pertama kalinya Anda memulai **Cloud Shell** dan Anda melihat pesan **Anda tidak memiliki penyimpanan yang terpasang**, pilih langganan yang Anda gunakan di lab ini, dan klik **Buat penyimpanan**.

1. Di toolbar panel Cloud Shell, klik ikon **Unggah/Unduh file**, di menu menurun, klik **Unggah** dan unggah file **\\Allfiles\\Lab\\04\\az104-04-vms-loop-template.json** dan **\\Allfiles\\Lab\\04\\az104-04-vms-loop-parameters.json** ke dalam direktori beranda Cloud Shell.

    >**Catatan**: Anda mungkin perlu mengunggah setiap file secara terpisah.

1. Edit file Parameter, dan ubah kata sandi. Jika Anda memerlukan bantuan untuk mengedit file di Shell, mintalah bantuan instruktur Anda. Untuk praktik terbaik, rahasia, seperti kata sandi, harus disimpan lebih aman di Key Vault. 

1. Dari panel Cloud Shell, jalankan perintah berikut untuk menyebarkan dua mesin virtual menggunakan file template dan parameter:

   ```powershell
   $rgName = 'az104-04-rg1'

   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-04-vms-loop-template.json `
      -TemplateParameterFile $HOME/az104-04-vms-loop-parameters.json
   ```

    >**Catatan**: Metode penyebaran template ARM ini menggunakan Azure PowerShell. Anda dapat melakukan tugas yang sama dengan menjalankan perintah Azure CLI yang setara **az deployment create** (untuk informasi selengkapnya, lihat [Menyebarkan sumber daya dengan template Resource Manager dan Azure CLI](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/deploy-cli).

    >**Catatan**: Tunggu hingga penyebaran selesai sebelum melanjutkan ke tugas berikutnya. Proses ini memerlukan waktu sekitar 2 menit.

    >**Catatan**: Jika Anda mendapatkan kesalahan yang menyatakan ukuran VM tidak tersedia, mintalah bantuan instruktur Anda dan coba langkah-langkah ini:
    > 1. Klik tombol `{}` di CloudShell Anda, pilih **az104-04-vms-loop-parameters.json** dari panel sisi kiri dan catat nilai parameter `vmSize`.
    > 1. Periksa lokasi tempat grup sumber daya 'az104-04-rg1' disebarkan. Anda dapat menjalankan `az group show -n az104-04-rg1 --query location` di CloudShell Anda untuk mendapatkannya.
    > 1. Jalankan `az vm list-skus --location <Replace with your location> -o table --query "[? contains(name,'Standard_D2s')].name"` di CloudShell Anda. Jika tidak ada SKU yang terdaftar (yaitu tidak ada hasil), maka Anda tidak dapat menyebarkan mesin virtual D2S di wilayah tersebut. Anda perlu menemukan wilayah yang memungkinkan Anda menggunakan mesin virtual D2S. Setelah memilih lokasi yang sesuai, hapus grup sumber daya AZ104-04-rg1 dan mulai ulang lab.
    > 1. Ganti nilai parameter `vmSize` dengan salah satu nilai yang dikembalikan oleh perintah yang baru saja Anda jalankan.
    > 1. Sekarang sebarkan ulang template Anda dengan menjalankan perintah `New-AzResourceGroupDeployment` lagi. Anda dapat menekan tombol atas beberapa kali yang akan membawa ke perintah yang dieksekusi terakhir.

1. Tutup panel Cloud Shell.

#### <a name="task-3-configure-private-and-public-ip-addresses-of-azure-vms"></a>Tugas 3: Mengonfigurasi alamat IP pribadi dan publik dari VM Azure

Dalam tugas ini, Anda akan mengonfigurasi penetapan statis alamat IP publik dan pribadi yang ditetapkan ke antarmuka jaringan mesin virtual Azure.

   >**Catatan**: Alamat IP pribadi dan publik sebenarnya ditetapkan ke antarmuka jaringan, yang, pada gilirannya, dilampirkan ke mesin virtual Azure, namun, cukup umum untuk merujuk ke alamat IP yang ditetapkan ke VM Azure.

1. Di portal Microsoft Azure, cari dan pilih **Grup sumber daya**, dan, pada panel **Grup sumber daya**, klik **az104-04-rg1**.

1. Pada panel grup sumber daya **az104-04-rg1**, dalam daftar sumber dayanya, klik **az104-04-vnet1**.

1. Pada panel jaringan virtual **az104-04-vnet1**, tinjauan bagian **Perangkat yang terhubung** dan verifikasi bahwa ada dua antarmuka jaringan **az104-04-nic0** dan **az104-04-nic1** terpasang ke jaringan virtual.

1. Klik **az104-04-nic0** dan, pada panel **az104-04-nic0**, klik **Konfigurasi IP**.

    >**Catatan**: Verifikasi bahwa **ipconfig1** saat ini disiapkan dengan alamat IP privat dinamis.

1. Di daftar konfigurasi IP, klik **ipconfig1**.

1. Pada panel **ipconfig1**, di bagian **Pengaturan alamat IP publik**, pilih **Asosiasi**, klik **+ Buat baru**, tentukan pengaturan berikut, dan klik **Oke**:

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **az104-04-pip0** |
    | SKU | **Standard** |

1. Pada panel **ipconfig1**, atur **Penugasan** ke **Static**, biarkan nilai default **alamat IP** diatur ke **10.40.0.4**.

1. Kembali ke panel **ipconfig1**, simpan perubahannya. Pastikan untuk menunggu operasi penyimpanan selesai sebelum Anda melanjutkan ke langkah berikutnya.

1. Navigasikan kembali ke panel **az104-04-vnet1**

1. Klik **az104-04-nic1** dan, pada panel **az104-04-nic1**, klik **Konfigurasi IP**.

    >**Catatan**: Verifikasi bahwa **ipconfig1** saat ini disiapkan dengan alamat IP pribadi dinamis.

1. Di daftar konfigurasi IP, klik **ipconfig1**.

1. Pada panel **ipconfig1**, di bagian **Pengaturan alamat IP publik**, pilih **Asosiasi**, klik **+ Buat baru**, tentukan pengaturan berikut, dan klik **Oke**:

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **az104-04-pip1** |
    | SKU | **Standard** |

1. Pada panel **ipconfig1**, atur **Penugasan** ke **Static**, biarkan nilai default **alamat IP** diatur ke **10.40.1.4**.

1. Kembali ke panel **ipconfig1**, simpan perubahannya.

1. Navigasikan kembali ke panel grup sumber daya **az104-04-rg1**, dalam daftar sumber dayanya, klik **az104-04-vm0**, dan dari panel mesin virtual **az104-04-vm0** , perhatikan entri alamat IP publik.

1. Navigasikan kembali ke panel grup sumber daya **az104-04-rg1**, dalam daftar sumber dayanya, klik **az104-04-vm1**, dan dari panel mesin virtual **az104-04-vm1** , perhatikan entri alamat IP publik.

    >**Catatan**: Anda akan memerlukan kedua alamat IP dalam tugas terakhir lab ini.

#### <a name="task-4-configure-network-security-groups"></a>Tugas 4: Mengonfigurasi kelompok keamanan jaringan

Dalam tugas ini, Anda akan mengonfigurasi kelompok keamanan jaringan untuk mengizinkan konektivitas terbatas ke mesin virtual Azure.

1. Di portal Microsoft Azure, navigasikan kembali ke panel grup sumber daya **az104-04-rg1**, dan dalam daftar sumber dayanya, klik **az104-04-vm0**.

1. Pada panel ringkasan **az104-04-vm0**, klik **Hubungkan**, klik **RDP** di menu menurun, di panel **Hubungkan dengan RDP**, klik **Unduh File RDP** menggunakan alamat IP Publik dan ikuti petunjuk untuk memulai sesi Desktop Jarak Jauh.

1. Perhatikan bahwa upaya koneksi gagal.

    >**Catatan**: Hal ini diharapkan, karena alamat IP publik SKU Standar, secara default, mengharuskan antarmuka jaringan yang ditetapkan untuk dilindungi oleh kelompok keamanan jaringan. Untuk mengizinkan koneksi Desktop Jarak Jauh, Anda akan membuat kelompok keamanan jaringan yang secara eksplisit mengizinkan lalu lintas RDP masuk dari Internet dan menetapkannya ke antarmuka jaringan kedua mesin virtual.

1. Hentikan mesin virtual **az104-04-vm0** dan **az104-04-vm1**.

    >**Catatan**: Hal ini dilakukan untuk kemanfaatan lab. Jika mesin virtual berjalan saat kelompok keamanan jaringan dilampirkan ke antarmuka jaringan mereka, diperlukan waktu lebih dari 30 menit agar lampiran diterapkan. Setelah kelompok keamanan jaringan dibuat dan dilampirkan, mesin virtual akan dimulai ulang, dan lampiran akan segera diterapkan.

1. Di portal Microsoft Azure, cari dan pilih **Kelompok keamanan jaringan**, dan, pada panel **Kelompok keamanan jaringan**, klik **+ Buat**.

1. Buat kelompok keamanan jaringan dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup Sumber Daya | **az104-04-rg1** |
    | Nama | **az104-04-nsg01** |
    | Wilayah | nama wilayah Azure tempat Anda menerapkan semua sumber daya lainnya di lab ini |

1. Klik **Tinjauan dan Buat**. Biarkan validasi berjalan, dan tekan **Buat** untuk mengirimkan penyebaran Anda.

    >**Catatan**: Tunggu hingga penyebaran selesai. Proses ini memerlukan waktu sekitar 2 menit.

1. Pada panel penyebaran, klik **Buka sumber daya** untuk membuka panel kelompok keamanan jaringan **az104-04-nsg01**.

1. Pada panel kelompok keamanan jaringan **az104-04-nsg01**, di bagian **Pengaturan**, klik **Aturan keamanan masuk**.

1. Tambahkan aturan masuk dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Sumber | **Mana pun** |
    | Rentang port sumber | * |
    | Tujuan | **Mana pun** |
    | Layanan | **RDP** |
    | Tindakan | **Izinkan** |
    | Prioritas | **300** |
    | Nama | **AllowRDPInBound** |

1. Pada panel kelompok keamanan jaringan **az104-04-nsg01**, di bagian **Pengaturan**, klik **Antarmuka jaringan** lalu klik **+ Kaitkan**.

1. Kaitkan kelompok keamanan jaringan **az104-04-nsg01** dengan antarmuka jaringan **az104-04-nic0** dan **az104-04-nic1**.

    >**Catatan**: Mungkin diperlukan waktu hingga 5 menit agar aturan dari Kelompok Keamanan Jaringan yang baru dibuat diterapkan ke Kartu Antarmuka Jaringan.

1. Mulai mesin virtual **az104-04-vm0** dan **az104-04-vm1**.

1. Navigasikan kembali ke panel mesin virtual **az104-04-vm0**.

    >**Catatan**: Pada langkah selanjutnya, Anda akan memverifikasi bahwa Anda berhasil terhubung ke mesin virtual target.

1. Pada panel **az104-04-vm0**, klik **Hubungkan**, klik **RDP**, pada panel **Hubungkan dengan RDP**, klik **Unduh File RDP** menggunakan alamat IP Publik dan ikuti petunjuk untuk memulai sesi Desktop Jarak Jauh.

    >**Catatan**: Langkah ini mengacu pada menghubungkan melalui Desktop Jauh dari komputer Windows. Di Mac, Anda dapat menggunakan Klien Desktop Jauh dari Mac App Store dan di komputer Linux Anda dapat menggunakan perangkat lunak klien RDP sumber terbuka.

    >**Catatan**: Anda dapat mengabaikan permintaan peringatan apa pun saat menghubungkan ke mesin virtual target.

1. Saat diminta, masuk dengan pengguna dan kata sandi di file parameter.

    >**Catatan**: Biarkan sesi Desktop Jauh terbuka. Anda akan membutuhkannya di tugas berikutnya.

#### <a name="task-5-configure-azure-dns-for-internal-name-resolution"></a>Tugas 5: Mengonfigurasi Azure DNS untuk resolusi nama internal

Dalam tugas ini, Anda akan mengonfigurasi resolusi nama DNS dalam jaringan virtual dengan menggunakan zona DNS privat Azure.

1. Di portal Microsoft Azure, cari dan pilih **Zona DNS Pribadi** dan, pada panel **Zona DNS Pribadi**, klik **+ Buat**.

1. Buat zona DNS pribadi dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup Sumber Daya | **az104-04-rg1** |
    | Nama | **contoso.org** |

1. Klik **Tinjauan dan Buat**. Biarkan validasi berjalan, dan tekan **Buat** lagi untuk mengirimkan penyebaran Anda.

    >**Catatan**: Tunggu hingga zona DNS pribadi dibuat. Proses ini memerlukan waktu sekitar 2 menit.

1. Klik **Buka sumber daya** untuk membuka panel zona pribadi DNS **contoso.org**.

1. Pada panel zona DNS pribadi **contoso.org**, di bagian **Pengaturan**, klik **Tautan jaringan virtual**

1. Klik **+ Tambah** untuk membuat tautan jaringan virtual dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama tautan | **az104-04-vnet1-link** |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Jaringan virtual | **az104-04-vnet1** |
    | Aktifkan pendaftaran otomatis | diaktifkan |

1. Klik **OK**.

    >**Catatan:** Tunggu hingga tautan jaringan virtual dibuat. Ini akan memerlukan waktu kurang dari 1 menit.

1. Pada panel zona DNS pribadi **contoso.org**, di panel sisi, klik **Ringkasan**

1. Verifikasi bahwa catatan DNS untuk **az104-04-vm0** dan **az104-04-vm1** muncul dalam daftar kumpulan catatan sebagai **Terdaftar otomatis**.

    >**Catatan:** Anda mungkin perlu menunggu beberapa menit dan merefresh halaman jika kumpulan catatan tidak terdaftar.

1. Beralih ke sesi Desktop Jarak Jauh ke **az104-04-vm0**, klik kanan tombol **Mulai** dan, di menu klik kanan, klik **Windows PowerShell (Admin)** .

1. Di jendela konsol Windows PowerShell, jalankan perintah berikut ini untuk menguji resolusi nama internal di zona DNS pribadi yang baru dibuat:

   ```powershell
   nslookup az104-04-vm0.contoso.org
   nslookup az104-04-vm1.contoso.org
   ```

1. Verifikasi bahwa output perintah menyertakan alamat IP pribadi **az104-04-vm1** (**10.40.1.4**).

#### <a name="task-6-configure-azure-dns-for-external-name-resolution"></a>Tugas 6: Mengonfigurasi Azure DNS untuk resolusi nama eksternal

Dalam tugas ini, Anda akan mengonfigurasi resolusi nama DNS eksternal dengan menggunakan zona DNS publik Azure.

1. Di browser web, buka tab baru dan navigasikan ke <https://www.godaddy.com/domains/domain-name-search>.

1. Gunakan pencarian nama domain untuk mengidentifikasi nama domain yang tidak digunakan.

1. Di portal Microsoft Azure, cari dan pilih **zona DNS** dan, pada panel **zona DNS**, klik **+ Buat**.

1. Buat zona DNS dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup Sumber Daya | **az104-04-rg1** |
    | Nama | nama domain DNS yang Anda identifikasi sebelumnya dalam tugas ini |

1. Klik **Tinjauan dan Buat**. Biarkan validasi berjalan, dan tekan **Buat** lagi untuk mengirimkan penyebaran Anda.

    >**Catatan**: Tunggu hingga zona DNS dibuat. Proses ini memerlukan waktu sekitar 2 menit.

1. Klik **Buka sumber daya** untuk membuka panel zona DNS yang baru dibuat.

1. Pada panel zona DNS, klik **+ Kumpulan catatan**.

1. Tambahkan kumpulan catatan dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **az104-04-vm0** |
    | Jenis | **A** |
    | Kumpulan catatan alias | **Tidak** |
    | TTL | **1** |
    | Unit TTL | **Jam** |
    | Alamat IP | alamat IP publik **az104-04-vm0** yang Anda identifikasi dalam latihan ketiga lab ini |

1. Klik **OK**

1. Pada panel zona DNS, klik **+ Kumpulan catatan**.

1. Tambahkan kumpulan catatan dengan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **az104-04-vm1** |
    | Jenis | **A** |
    | Kumpulan catatan alias | **Tidak** |
    | TTL | **1** |
    | Unit TTL | **Jam** |
    | Alamat IP | alamat IP publik **az104-04-vm1** yang Anda identifikasi dalam latihan ketiga lab ini |

1. Klik **OK**

1. Pada panel zona DNS, catat nama entri **Name server 1**.

1. Di portal Azure, buka sesi **PowerShell** di **Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Dari panel Cloud Shell, jalankan perintah berikut untuk menguji resolusi nama eksternal dari rangkaian data DNS **az104-04-vm0** di zona DNS yang baru dibuat (ganti tempat penampung `[Name server 1]` dengan nama **Name server 1** yang Anda catat sebelumnya dalam tugas ini dan `[domain name]` tempat penampung dengan nama domain DNS yang Anda buat sebelumnya dalam tugas ini):

   ```powershell
   nslookup az104-04-vm0.[domain name] [Name server 1]
   ```

1. Verifikasi bahwa output perintah menyertakan alamat IP publik **az104-04-vm0**.

1. Dari panel Cloud Shell, jalankan perintah berikut untuk menguji resolusi nama eksternal dari rangkaian data DNS **az104-04-vm1** di zona DNS yang baru dibuat (ganti tempat penampung `[Name server 1]` dengan nama **Name server 1** yang Anda catat sebelumnya dalam tugas ini dan tempat penampung `[domain name]` dengan nama domain DNS yang Anda buat sebelumnya dalam tugas ini):

   ```powershell
   nslookup az104-04-vm1.[domain name] [Name server 1]
   ```

1. Verifikasi bahwa output perintah menyertakan alamat IP publik **az104-04-vm1**.

#### <a name="clean-up-resources"></a>Membersihkan sumber daya

 > **Catatan**: Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak digunakan akan memastikan bahwa Anda tidak akan melihat biaya yang tidak diharapkan.

 > **Catatan**:  Jangan khawatir jika sumber daya lab tidak dapat segera dihapus. Terkadang sumber daya memiliki dependensi dan membutuhkan waktu lebih lama untuk dihapus. Ini adalah tugas Administrator yang umum untuk memantau penggunaan sumber daya, jadi tinjauan sumber daya Anda secara berkala di Portal untuk melihat bagaimana pembersihannya. 

1. Di portal Microsoft Azure, buka sesi **PowerShell** dalam panel **Cloud Shell**.

1. Buat daftar semua grup sumber daya yang dibuat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-04*'
   ```

1. Hapus semua grup sumber daya yang Anda buat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```powershell
   Get-AzResourceGroup -Name 'az104-04*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Catatan**: Perintah dijalankan secara asinkron (sebagaimana yang ditentukan oleh parameter -AsJob), jadi saat Anda akan dapat menjalankan perintah PowerShell lain langsung setelahnya dalam sesi PowerShell yang sama, proses ini akan memakan waktu beberapa menit sebelum grup sumber daya benar-benar dihapus.

#### <a name="review"></a>Tinjauan

Di lab ini, Anda telah:

+ Membuat dan mengonfigurasi jaringan virtual
+ Menyebarkan mesin virtual ke dalam jaringan virtual
+ Alamat IP pribadi dan publik yang dikonfigurasi dari VM Azure
+ Kelompok keamanan jaringan yang dikonfigurasi
+ Azure DNS yang dikonfigurasi untuk resolusi nama internal
+ Azure DNS yang dikonfigurasi untuk resolusi nama eksternal
