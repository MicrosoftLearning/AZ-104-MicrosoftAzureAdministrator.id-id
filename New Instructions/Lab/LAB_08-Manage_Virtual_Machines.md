---
lab:
  title: 'Lab 08: Mengelola Komputer Virtual'
  module: Administer Virtual Machines
---

# Lab 08 - Kelola Virtual Machines

## Pengenalan lab

Di lab ini, Anda membandingkan penskalaan manual komputer virtual dengan penskalaan otomatis komputer virtual. Anda mempelajari cara mengonfigurasi dan mengubah ukuran satu komputer virtual. Anda mempelajari cara membuat set skala komputer virtual dan mengonfigurasi penskalakan otomatis.

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur di lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkahnya ditulis menggunakan US Timur.

## Perkiraan waktu: 50 menit

## Skenario lab

Organisasi Anda ingin menjelajahi penyebaran dan konfigurasi komputer virtual Azure. Pertama, Anda perlu menentukan opsi ketahanan dan skalabilitas komputasi dan penyimpanan yang berbeda yang dapat Anda terapkan saat menggunakan mesin virtual Azure. Selanjutnya, Anda perlu menyelidiki opsi ketahanan dan skalabilitas komputasi dan penyimpanan yang tersedia saat menggunakan kumpulan skala mesin virtual Azure.

## Simulasi lab interaktif

Ada simulasi lab interaktif yang mungkin berguna bagi Anda untuk topik ini. Simulasi ini memungkinkan Anda mengklik skenario serupa dengan kecepatan Anda sendiri. Ada perbedaan antara simulasi interaktif dan lab ini, tetapi banyak konsep intinya sama. Langganan Azure tidak diperlukan.

+ [Buat komputer virtual di portal](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%201). Buat komputer virtual, sambungkan dan instal peran server web. 
+ [Menyebarkan komputer virtual dengan templat](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%209). Jelajahi galeri Mulai Cepat dan temukan templat komputer virtual. Menyebarkan templat dan memverifikasi penyebarannya.
+ [Buat komputer virtual dengan PowerShell](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2010). Gunakan Azure PowerShell untuk menyebarkan komputer virtual. Tinjau rekomendasi Azure Advisor. 
+ [Buat komputer virtual dengan CLI](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2011). Gunakan CLI untuk menyebarkan komputer virtual. Tinjau rekomendasi Azure Advisor. 

## Tugas

+ Tugas 1: Menyebarkan komputer virtual Azure yang tangguh zona dengan menggunakan portal Azure
+ Tugas 2: Mengelola komputasi dan penskalaan penyimpanan untuk komputer virtual
+ Tugas 3: Menerapkan Azure Virtual Machine Scale Sets
+ Tugas 4: Menskalakan Set Skala Komputer Virtual Azure
+ Tugas 5: Membuat komputer virtual menggunakan Azure PowerShell (opsi 1)
+ Tugas 6: Membuat komputer virtual menggunakan CLI (opsi 2) 




## Tugas 1 dan 2: Diagram Arsitektur Azure Virtual Machines

![Diagram tugas arsitektur.](../media/az104-lab08a-architecture-diagram.png)

## Tugas 1: Menyebarkan komputer virtual Azure yang tangguh zona dengan menggunakan portal Azure

Dalam tugas ini, Anda akan menyebarkan dua komputer virtual Azure ke zona ketersediaan yang berbeda dengan menggunakan portal Azure. Zona ketersediaan menawarkan tingkat waktu aktif tertinggi SLA untuk komputer virtual pada 99,99%. Untuk mencapai SLA ini, Anda harus menyebarkan setidaknya dua komputer virtual di berbagai zona ketersediaan.

1. Masuk ke portal Azure - `https://portal.azure.com`.

1. Cari dan pilih `Virtual machines` dan, pada bilah **Komputer** virtual, klik **+ Buat**, lalu pilih di menu drop-down **+ komputer virtual** Azure.

1. Pada tab **Dasar dari bilah **Buat komputer** virtual, di menu drop-down Zona** ketersediaan, letakkan tanda centang di **samping **Zona 2**.** Ini harus memilih **Zona 1** dan **Zona 2**.

    >**Catatan**: Ini akan menyebarkan dua komputer virtual di wilayah yang dipilih, satu di setiap zona. Anda mencapai SLA waktu aktif 99,99% karena Anda memiliki setidaknya dua VM yang didistribusikan di setidaknya dua zona. Dalam skenario di mana Anda mungkin hanya memerlukan satu VM, ini adalah praktik terbaik untuk masih menyebarkan VM ke zona untuk memastikan bahwa disk dan sumber daya yang sesuai terletak di zona yang sama.

1. Pada tab Dasar, gunakan pengaturan berikut untuk menyelesaikan bidang (biarkan bidang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure Anda |
    | Grup sumber daya |  **az104-rg8** (Jika perlu, klik **Buat baru**) |
    | Nama komputer virtual | `az104-vm1` dan `az104-vm2` (Setelah memilih kedua zona ketersediaan, pilih **Edit nama** di bawah bidang nama VM.) |
    | Wilayah | **US Timur** |
    | Opsi ketersediaan | **Zona ketersediaan** |
    | Zona ketersediaan | **Zona 1, 2** (baca catatan tentang menggunakan set skala komputer virtual) |
    | Jenis keamanan | **Standard**
           |
    | Gambar | **Ubuntu Server 20.04 LTS - x64 Gen2** |
    | Instans Azure Spot | **Dicentang** |
    | Ukuran | **Standar D2s v3** |
    | Jenis autentikasi | **Password** |
    | Nama Pengguna | `localadmin` |
    | Kata sandi | **Berikan kata sandi yang aman** |
    | Port masuk publik | **Tidak** |
    | Apakah Anda ingin menggunakan lisensi Windows Server yang sudah ada? | **Tidak Dicentang** |

    ![Cuplikan layar halaman buat vm.](../media/az104-lab08-create-vm.png)

1. Klik **Berikutnya: Disk >** dan, pada tab **Disk** dari bilah **Buat komputer** virtual, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Jenis disk OS | **SSD Premium** |
    | Mengaktifkan kompatibilitas Ultra Disk | **Tidak Dicentang** |

1. Klik **Berikutnya: Jaringan >** mengambil default tetapi tidak menyediakan load balancer. 
   
    | Opsi penyeimbangan beban | **Tidak** |
    
1. Klik **Berikutnya: Manajemen >** dan, pada tab **Manajemen** dari bilah **Buat komputer** virtual, tentukan pengaturan berikut (biarkan orang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Opsi orkestrasi patch | **Azure diorkestrasi** |  

1. Klik **Berikutnya: Pemantauan >** dan, pada tab **Pemantauan** dari blade **Buat mesin virtual**, tentukan pengaturan berikut (biarkan yang lain diatur ke nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Diagnostik boot | **Nonaktifkan** |

1. Klik **Berikutnya: >** Tingkat Lanjut, pada tab **Tingkat Lanjut** dari bilah **Buat komputer** virtual, tinjau pengaturan yang tersedia tanpa memodifikasi salah satunya, dan klik **Tinjau + Buat**.

1. Pada bilah **Tinjau + Buat**, klik **Buat**.

    >**Catatan:** Pantau **pesan Pemberitahuan** dan tunggu hingga penyebaran selesai. 

## Tugas 2: Mengelola komputasi dan penskalaan penyimpanan untuk komputer virtual

Dalam tugas ini, Anda akan menskalakan komputasi untuk komputer virtual dengan menyesuaikan ukurannya ke SKU yang berbeda. Azure memberikan fleksibilitas dalam pemilihan ukuran VM sehingga Anda dapat menyesuaikan VM untuk jangka waktu tertentu jika membutuhkan lebih banyak komputasi (atau kurang) dan memori yang dialokasikan. Konsep ini diperluas ke disk, di mana Anda dapat memodifikasi performa disk, atau meningkatkan kapasitas yang dialokasikan.

1. Di portal Azure, cari dan pilih **az104-vm1**.

1. Pada bilah **komputer virtual az104-vm1** , klik **Ukuran** dan atur ukuran komputer virtual ke **DS1_v2** dan klik **Mengubah Ukuran**

    >**Catatan**: Pilih ukuran lain jika **DS1_v2** Standar tidak tersedia.

    ![Cuplikan layar mengubah ukuran komputer virtual.](../media/az104-lab08-resize-vm.png)

1. Pada bilah **komputer virtual az104-vm1** , klik **Disk**, Di bawah **Disk data** klik **+ Buat dan lampirkan disk** baru.

1. Buat disk terkelola dengan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama disk | `vm1-disk1` |
    | Jenis penyimpanan | **HDD Standar** |
    | Ukuran (GiB) | `32` |

1. Klik **Terapkan**.

1. Setelah disk dibuat, klik **Lepaskan**, lalu klik **Terapkan**.
    
    >**Catatan**: Anda mungkin perlu menggulir ke kanan untuk melihat ikon lepaskan**.

1. Dari portal Azure, cari dan pilih `Disks`.

1. Dari daftar disk, pilih **objek vm1-disk1** .

1. Dari vm1-disk1, pilih **Ukuran + performa**.

1. Dari Ukuran + performa, atur jenis penyimpanan ke **SSD** Standar, lalu klik **Simpan**.

    >**Catatan**: Anda tidak dapat mengubah jenis penyimpanan disk saat dilampirkan atau saat VM berjalan. 

1. Navigasi kembali ke **komputer virtual az104-vm1** dan pilih **Disk**.

1. Verifikasi disk sekarang **HDD** Standar.

## Tugas 3 dan 4: Diagram Arsitektur Set Skala Komputer Virtual Azure

![Diagram tugas arsitektur.](../media/az104-lab08b-architecture-diagram.png)

## Tugas 3: Menerapkan Azure Virtual Machine Scale Sets

Dalam tugas ini, Anda akan menyebarkan set skala komputer virtual Azure di seluruh zona ketersediaan. Dengan VM individual, Anda akan memerlukan otomatisasi lain untuk menyebarkan dan mengonfigurasi VM tambahan jika aplikasi Anda membutuhkan komputasi tambahan. Set Skala VM mengurangi overhead administratif otomatisasi dengan memungkinkan Anda mengonfigurasi metrik atau kondisi yang memungkinkan set skala untuk secara otomatis meningkatkan atau menurunkan jumlah VM dalam set.

1. Di portal Azure, cari dan pilih `Virtual machine scale sets` dan, pada bilah **Set** skala komputer virtual, klik **+ Buat**.

1. Pada tab **Dasar dari bilah **Buat set** skala komputer virtual, tentukan pengaturan berikut (biarkan orang lain dengan nilai defaultnya) dan klik **Berikutnya : Spot >****:

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure Anda  |
    | Grup sumber daya | **az104-rg8**  |
    | Nama set skala komputer virtual | `vmss1` |
    | Wilayah | **US** Timur (atau wilayah di dekat Anda) |
    | Zona ketersediaan | **Zona 1, 2, 3** |
    | Mode Orkestrasi | **Seragam** |
    | Jenis keamanan | **Standard**
           | 
    | Gambar | **Pusat Data Windows Server 2019 - x64 Gen2** |
    | Jalankan dengan diskon Azure Spot | **Tidak Dicentang** |
    | Ukuran | **D2s_v3 standar** |
    | Nama Pengguna | `localadmin` |
    | Kata sandi | **Berikan kata sandi yang aman**  |
    | Sudah memiliki lisensi Windows Server? | **Tidak Dicentang** |

    >**Catatan**: Untuk daftar wilayah Azure yang mendukung penyebaran komputer virtual Windows ke zona ketersediaan, lihat [Apa itu Zona Ketersediaan di Azure?](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview)

    ![Cuplikan layar halaman buat vmss. ](../media/az104-lab08-create-vmss.png)

1. Pada tab **Spot** , terima default dan pilih **Berikutnya: Disk >**.

1. Pada tab **Disk** , terima nilai default dan klik **Berikutnya : Jaringan >**.

1. Pada tab **Jaringan** , klik **tautan Buat jaringan** virtual di bawah **kotak teks Jaringan** virtual dan buat jaringan virtual baru dengan pengaturan berikut (biarkan orang lain dengan nilai defaultnya).  Setelah selesai, pilih **OK**. 

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | `vmss-vnet` |
    | Rentang alamat | `10.82.0.0/20` |
    | Nama subnet | `subnet0` |
    | Rentang subnet | `10.82.0.0/24` |

1. Di tab **Jaringan** , klik **ikon Edit antarmuka** jaringan di sebelah kanan entri antarmuka jaringan.

1. Pada bilah **Edit antarmuka jaringan**, di bagian **grup keamanan jaringan NIC**, klik **Lanjutan** dan klik **Buat baru** pada ** Konfigurasikan daftar dropdown grup keamanan jaringan**.

1. Pada bilah **Buat grup keamanan jaringan**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **vmss1-nsg** |

1. Klik **Tambahkan aturan masuk** dan tambahkan aturan keamanan masuk dengan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Sumber | **Mana pun** |
    | Source port ranges | * |
    | Tujuan | **Mana pun** |
    | Service | **HTTP** |
    | Tindakan
           | **Izinkan** |
    | Prioritas | **1010** |
    | Nama | `allow-http` |

1. Klik **Tambahkan** dan, kembali pada bilah **Buat grup keamanan jaringan**, klik **OK**.

1. Di bilah **Edit antarmuka** jaringan, di bagian **Alamat** IP publik, klik **Diaktifkan** dan klik **OK**.

1. Di tab **Jaringan** , di bawah bagian **Penyeimbangan** beban, tentukan yang berikut ini (biarkan orang lain dengan nilai defaultnya).

    | Pengaturan | Nilai |
    | --- | --- |
    | Opsi penyeimbangan muatan | **Penyeimbang beban Azure** |
    | Pilih load balancer | **Membuat penyeimbang beban** |
    
1.  Pada halaman **Buat load balancer** , tentukan nama load balancer dan ambil default. Klik **Buat** setelah selesai, lalu **Berikutnya : Penskalakan >**.
    
    | Pengaturan | Nilai |
    | --- | --- |
    | Nama load balancer | `vmss-lb` |

1. Pada tab **Penskalakan** , tentukan pengaturan berikut (biarkan orang lain dengan nilai defaultnya) dan klik **Berikutnya : Manajemen >**:

    | Pengaturan | Nilai |
    | --- | --- |
    | Jumlah instans awal | `2` |
    | Kebijakan penskalaan | **Manual** |

1. Pada tab **Manajemen** , tentukan pengaturan berikut (biarkan orang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Diagnostik boot | **Nonaktifkan** |
    
1. Klik **Berikutnya : >** kesehatan:

1. Pada tab **Kesehatan** , tinjau pengaturan default tanpa membuat perubahan apa pun dan klik **Berikutnya : >** Tingkat Lanjut.

1. Pada tab **Tingkat Lanjut** , klik **Tinjau + buat**.

1. Pada tab **Tinjau + buat** , pastikan validasi lulus dan klik **Buat**.

    >**Catatan**: Tunggu hingga penyebaran set skala komputer virtual selesai. Ini akan memakan waktu sekitar 5 menit.


## Tugas 4: Menskalakan Set Skala Komputer Virtual Azure

Dalam tugas ini, Anda menskalakan set skala komputer virtual menggunakan aturan skala kustom.

1. Pilih **Buka sumber daya** atau cari dan pilih **set skala vmss1** .

1. Pilih **Penskalaan** dari menu di sisi kiri dari jendela set skala.

1. **Perhatikan bahwa mode** Skala dapat berupa **Skala berdasarkan metrik atau **Skalakan ke jumlah** instans** tertentu. Dalam set skala dengan sejumlah kecil instans VM, meningkatkan atau mengurangi jumlah instans mungkin yang terbaik. Dalam set skala dengan sejumlah besar instans VM, penskalaan berdasarkan metrik mungkin lebih tepat.

1. Pilih tombol untuk **Skala otomatis kustom**. Lalu pilih **Tambahkan aturan**. 

### Aturan peluasan skala

1. Mari kita buat aturan peluasan skala yang secara otomatis meningkatkan jumlah instans VM. Aturan ini diskalakan ketika beban CPU rata-rata lebih besar dari 70% selama periode 10 menit. Ketika aturan memicu, jumlah instans VM bertambah sebanyak 20%. Klik **Tambahkan** setelah membuat pilihan Anda. 

    | Pengaturan | Nilai |
    | --- | --- |
    | Sumber metrik | **Sumber daya saat ini (vmss1)** |
    | Namespace metrik | **Host Mesin Virtual** |
    | Nama metrik | **Persentase CPU** (tinjau pilihan Anda yang lain) |
    | Operator | **Lebih besar dari** |
    | Ambang batas metrik untuk memicu tindakan penskalaan | **70** |
    | Durasi (menit) | **10** |
    | Statistik butir waktu | **Tengah** |
    | Operasi | **Tingkatkan persen menurut** (tinjau pilihan lain) |
    | Pendinginan (menit) | **5**
           |
    | Persentase | **20** |
    
    ![Cuplikan layar halaman tambahkan aturan penskalaan.](../media/az104-lab08-scale-rule.png)

### Menskalakan dalam aturan

1. Selama malam atau akhir pekan, permintaan dapat menurun sehingga penting untuk membuat aturan skala.

1. Mari kita buat aturan yang mengurangi jumlah instans VM dalam set skala. Jumlah instans harus berkurang ketika beban CPU rata-rata turun di bawah 30% selama periode 10 menit. Ketika aturan memicu, jumlah instans VM berkurang sebanyak 20%. Sesuaikan pengaturan, lalu pilih **Tambahkan**.

    | Pengaturan | Nilai |
    | --- | --- |
    | Operator | **Kurang dari** |
    | Ambang | **30** |
    | Operasi | **kurangi persen dengan** (tinjau pilihan Anda yang lain) |
    | Jumlah Instans | **20** |

### Mengatur batas instans

1. Saat aturan skala otomatis Anda diterapkan, batas instans memastikan bahwa Anda tidak menskalakan melebihi jumlah maksimum instans, atau menskalakan di luar jumlah instans minimum.

1. **Batas instans ditampilkan di **halaman Penskalakan**** setelah aturan. 

    | Pengaturan | Nilai |
    | --- | --- |
    | Minimum | **2** |
    | Maksimum | **10** |
    | Default | **2** |

1. Pastikan untuk **Menyimpan** perubahan Anda

1. Pada halaman **vmss1** , pilih **Instans**. Di sinilah Anda akan memantau jumlah instans komputer virtual. 

## Tugas 5: Membuat komputer virtual menggunakan Azure PowerShell (opsi 1)

1. Masuk ke portal Azure - `https://portal.azure.com`.

1. Gunakan menu untuk meluncurkan **sesi Cloud Shell** . Secara bergantian, navigasikan langsung ke `https://shell.azure.com`.

1. Jika perlu, konfigurasikan Cloud Shell. Pastikan untuk memilih **PowerShell**.

1. Jalankan perintah berikut untuk membuat komputer virtual. Saat diminta, berikan nama pengguna dan kata sandi untuk VM. Saat Anda menunggu, lihat [referensi perintah New-AzVM](https://learn.microsoft.com/powershell/module/az.compute/new-azvm?view=azps-11.1.0) untuk semua parameter yang terkait dengan pembuatan komputer virtual.

    ```powershell
    New-AzVm `
    -ResourceGroupName 'az104-rg8' `
    -Name 'myPSVM' `
    -Location 'East US' `
    -Image 'Win2019Datacenter' `
    -Zone '1' `
    -Size 'Standard_D2s_v3' 
    -Credential '(Get-Credential)' `

1. Once the command completes, use **Get-AzVM** to list the virtual machines in your resource group. 

    ```powershell
    Get-AzVM `
    -ResourceGroupName 'az104-rg8'
    -Status

1. Verify your new virtual machine is listed and the **Status** is **Running**.

1. Use **Stop-AzVM** to deallocate your virtual machine. Type **Yes** to confirm. 

    ```
    Stop-AzVM `
    -ResourceGroupName 'az104-rg8'
    -Name 'myPSVM' `

1. Gunakan Get-AzVM** dengan **parameter -Status** untuk memverifikasi bahwa komputer dibatalkan alokasinya****.**

    >**Apakah Anda tahu?** Saat Anda menggunakan Azure untuk menghentikan komputer virtual Anda, status dibatalkan alokasinya**. Ini berarti bahwa IP publik non-statis dirilis, dan Anda berhenti membayar biaya komputasi VM.

## Tugas 6: Membuat komputer virtual menggunakan CLI (opsi 2)

1. Masuk ke portal Azure - `https://portal.azure.com`.

1. Gunakan menu untuk meluncurkan **sesi Cloud Shell** . Secara bergantian, navigasikan langsung ke `https://shell.azure.com`.

1. Jika perlu, konfigurasikan Cloud Shell. Pastikan untuk memilih **Bash**.

1. Jalankan perintah berikut untuk membuat komputer virtual. Saat diminta, berikan nama pengguna dan kata sandi untuk VM. Saat Anda menunggu, lihat [referensi perintah az vm create](https://learn.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) untuk semua parameter yang terkait dengan pembuatan komputer virtual.


    ```sh
    az vm create --name myCLIVM --resource-group az104-rg8 --image Ubuntu2204 --admin-username localadmin --generate-ssh-keys 
    
1. Once the command completes, use **az vm show** to verify your machine was created.

    ```sh
    az vm show --name  myCLIVM --resource-group az104-rg8 --show-details

1. Verify the **powerState** is **VM Running**.

1. Use **az vm deallocate** to deallocate your virtual machine. Type **Yes** to confirm. 

    ```sh
    az vm deallocate --resource-group az104-rg8 --name myCLIVM

1. Use **az vm show** to ensure the **powerState** is **VM deallocated**.

    >**Did you know?** When you use Azure to stop your virtual machine, the status is *deallocated*. This means that any non-static public IPs are released, and you stop paying for the VM’s compute costs.

## Key takeaways

Congratulations on completing the lab. Here are the main takeaways for this lab. 

+ Azure virtual machines are on-demand, scalable computing resources.
+ Configuring Azure virtual machines includes choosing an operating system, size, storage and networking settings. 
+ Azure Virtual Machine Scale Sets let you create and manage a group of load balanced VMs.
+ The virtual machines in a Virtual Machine Scale Set are created from the same image and configuration. 
+ In a Virtual Machine Scale Set the number of VM instances can automatically increase or decrease in response to demand or a defined schedule.

## Cleanup your resources

If you are working with your own subscription take a minute to delete the lab resources. This will ensure resources are freed up and cost is minimized. The easiest way to delete the lab resources is to delete the lab resource group. 

+ In the Azure portal, select the resource group, select **Delete the resource group**, **Enter resource group name**, and then click **Delete**.
+ Using Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Using the CLI, `az group delete --name resourceGroupName`.


