---
lab:
  title: 'Lab 02a: Mengelola Langganan dan RBAC'
  module: Administer Governance and Compliance
---

# Lab 02a - Mengelola Langganan dan RBAC

## Pengenalan lab

Di lab ini, Anda mempelajari tentang kontrol akses berbasis peran. Anda mempelajari cara menggunakan izin dan cakupan untuk mengontrol identitas tindakan apa yang dapat dan tidak dapat dilakukan. Anda juga mempelajari cara mempermudah manajemen langganan menggunakan grup manajemen. 

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur di lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkahnya ditulis menggunakan **US** Timur. 

## Perkiraan waktu: 30 menit

## Skenario lab

Untuk menyederhanakan manajemen sumber daya Azure di organisasi Anda, Anda telah ditugaskan untuk menerapkan fungsionalitas berikut:

- Membuat grup manajemen yang menyertakan semua langganan Azure Anda.

- Memberikan izin untuk mengirimkan permintaan dukungan untuk semua langganan di grup manajemen. Izin harus dibatasi hanya untuk: 

    - Membuat dan mengelola komputer virtual
    - Membuat tiket permintaan dukungan (tidak termasuk menambahkan penyedia Azure)


## Simulasi lab interaktif

Ada beberapa simulasi lab interaktif yang mungkin berguna bagi Anda untuk topik ini. Simulasi ini memungkinkan Anda mengklik skenario serupa dengan kecepatan Anda sendiri. Ada perbedaan antara simulasi interaktif dan lab ini, tetapi banyak konsep intinya sama. Langganan Azure tidak diperlukan. 

+ [Mengelola akses dengan RBAC](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2014). Tetapkan peran bawaan untuk pengguna dan pantau log aktivitas. 

+ [Mengelola langganan dan RBAC](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%202). Terapkan grup manajemen dan buat dan tetapkan peran RBAC kustom.

+ [Buka permintaan](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2022) dukungan. Tinjau opsi paket dukungan, lalu buat dan pantau permintaan dukungan, teknis atau penagihan.

## Diagram arsitektur

![Diagram tugas lab.](../media/az104-lab02a-architecture.png)

## Keterampilan pekerjaan

+ Tugas 1: Menerapkan grup manajemen.
+ Tugas 2: Tinjau dan tetapkan peran Azure bawaan.
+ Tugas 3: Buat peran RBAC kustom.
+ Tugas 4: Memantau penetapan peran dengan Log Aktivitas.

## Tugas 1: Menerapkan Grup Manajemen

Dalam tugas ini, Anda akan membuat dan mengonfigurasi grup manajemen. Grup manajemen digunakan untuk mengatur langganan secara logis. Langganan harus disegmentasi dan memungkinkan RBAC dan Azure Policy ditetapkan dan diwariskan ke grup manajemen dan langganan lainnya. Misalnya, jika organisasi Anda memiliki tim dukungan khusus untuk Eropa, Anda dapat mengatur langganan Eropa ke dalam grup manajemen untuk menyediakan akses staf dukungan ke langganan tersebut (tanpa menyediakan akses individual ke semua langganan). Dalam skenario kami, semua orang di Help Desk harus membuat permintaan dukungan di semua langganan. 

1. Masuk ke **portal Azure** - `https://portal.azure.com`.

1. Cari dan pilih `Microsoft Entra ID`.

1. Di bilah **Kelola** , pilih **Properti**.

1. **Tinjau area Manajemen akses untuk sumber daya** Azure. Pastikan Anda dapat mengelola akses ke semua langganan Azure dan grup manajemen di penyewa.
   
1. Cari dan pilih `Management groups`.

1. Pada panel **Grup manajemen**, klik **+ Buat**.

1. Buat grup manajemen dengan pengaturan berikut. Pilih **Kirim** saat Anda selesai. 

    | Pengaturan | Nilai |
    | --- | --- |
    | ID grup manajemen | `az104-mg1` (harus unik dalam direktori) |
    | Nama tampilan grup manajemen | `az104-mg1` |

1. **Refresh** halaman grup manajemen untuk memastikan grup manajemen baru Anda ditampilkan. Ini mungkin memakan waktu satu menit. 

   >**Catatan:** Apakah Anda melihat grup manajemen akar? Grup manajemen root ini dibangun ke dalam hierarki agar semua grup manajemen dan langganan berada di dalamnya. Grup manajemen akar ini memungkinkan kebijakan global dan penetapan peran Azure diterapkan di tingkat direktori. Setelah membuat grup manajemen, Anda akan menambahkan langganan apa pun yang harus disertakan dalam grup. 

## Tugas 2: Meninjau dan menetapkan peran Azure bawaan

Dalam tugas ini, Anda akan meninjau peran bawaan dan menetapkan peran Kontributor VM kepada anggota Staf Dukungan. Azure menyediakan sejumlah besar peran bawaan[](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles). 

1. Pilih **grup manajemen az104-mg1** .

1. Pilih bilah **Kontrol akses (IAM),** lalu tab **Peran** .

1. Gulir definisi peran bawaan yang tersedia. **Lihat** peran untuk mendapatkan informasi terperinci tentang **Izin**, **JSON**, dan **Penugasan**. Anda akan sering menggunakan *pemilik*, *kontributor*, dan *pembaca*. 

1. Pilih **+ Tambahkan**, dari menu drop-down, pilih **Tambahkan penetapan** peran. 

1. Pada bilah **Tambahkan penetapan** peran, cari dan pilih **Kontributor** Komputer Virtual. Peran Kontributor komputer virtual memungkinkan Anda mengelola komputer virtual, tetapi tidak mengakses sistem operasi mereka atau mengelola jaringan virtual dan akun penyimpanan yang terhubung dengannya. Ini adalah peran yang baik untuk Help Desk. Pilih **Selanjutnya**.

    >**Apakah Anda tahu?** Azure awalnya hanya **menyediakan model penyebaran Klasik** . Ini telah digantikan oleh **model penyebaran Azure Resource Manager** . Sebagai praktik terbaik, jangan gunakan sumber daya klasik. 

1. Pada tab **Anggota** , **Pilih Anggota**.

    >**Catatan:** Langkah berikutnya menetapkan peran ke **grup helpdesk** . Jika Anda tidak memiliki grup Help Desk, luangkan waktu satu menit untuk membuatnya.

1. Cari dan pilih `helpdesk` grup. Klik **Pilih**. 

1. Klik **Tinjau + tetapkan** dua kali untuk membuat penetapan peran.

1. Lanjutkan pada bilah **Kontrol akses (IAM).** Pada tab **Penetapan** peran, konfirmasikan **grup helpdesk** memiliki **peran Kontributor** Komputer Virtual. 

    >**Catatan:** Sebagai praktik terbaik, selalu tetapkan peran ke grup bukan individu. 

    >**Apakah Anda tahu?** Penugasan ini mungkin tidak benar-benar memberi Anda hak istimewa tambahan. Jika Anda sudah memiliki peran Pemilik, peran tersebut menyertakan semua izin yang terkait dengan peran Kontributor VM.
    
## Tugas 3: Membuat peran RBAC kustom

Dalam tugas ini, Anda akan membuat peran RBAC kustom. Peran kustom adalah bagian inti dari menerapkan prinsip hak istimewa paling sedikit untuk lingkungan. Peran bawaan mungkin memiliki terlalu banyak izin untuk skenario Anda. Dalam tugas ini kita akan membuat peran baru dan menghapus izin yang tidak diperlukan. Apakah Anda memiliki rencana untuk mengelola izin yang tumpang tindih?

1. Lanjutkan mengerjakan grup manajemen Anda. Di bilah **Kontrol akses (IAM),** pilih tab **Periksa akses** .

1. Dalam kotak **Buat peran** kustom, pilih **Tambahkan**.

1. Pada tab Dasar selesaikan konfigurasi.

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama peran kustom | `Custom Support Request` |
    | Deskripsi | ''Peran kontributor kustom untuk permintaan dukungan.' |

1. Untuk **Izin** garis besar, pilih **Kloning peran**. **Di menu drop-down Peran untuk mengkloning**, pilih **Kontributor** Permintaan Dukungan.

    ![Cuplikan layar mengkloning peran.](../media/az104-lab02a-clone-role.png)

1. Pilih **Berikutnya** untuk berpindah ke tab **Izin** , lalu pilih **+ Kecualikan izin**.

1. Di bidang pencarian penyedia sumber daya, masukkan `.Support` dan pilih **Microsoft.Support**.

1. Dalam daftar izin, letakkan kotak centang di samping **Lainnya: Mendaftarkan Penyedia** Sumber Daya Dukungan lalu pilih **Tambahkan**. Peran harus diperbarui untuk menyertakan izin ini sebagai *NotAction*.

    >**Catatan:** Penyedia sumber daya Azure adalah sekumpulan operasi REST yang memungkinkan fungsionalitas untuk layanan Azure tertentu. Kami tidak ingin Help Desk dapat memiliki kemampuan ini, sehingga dihapus dari peran kloning. Anda juga dapat memisahkan dan menambahkan kemampuan lain ke peran baru. 

1. Pada tab **Cakupan** yang dapat ditetapkan, pastikan grup manajemen Anda tercantum, lalu klik **Berikutnya**.

1. Tinjau JSON untuk *Tindakan*, *NotActions*, dan *AssignableScopes* yang disesuaikan dalam peran. 

1. Pilih **Tinjauan + Buat**, kemudian pilih **Buat**.

    >**Catatan:** Pada titik ini, Anda telah membuat peran kustom dan menetapkannya ke grup manajemen.  

## Tugas 4: Memantau penetapan peran dengan Log Aktivitas

Dalam tugas ini, Anda melihat log aktivitas untuk menentukan apakah ada yang telah membuat peran baru. 

1. Di portal, temukan **sumber daya az104-mg1** dan pilih **Log aktivitas**. Log aktivitas memberikan wawasan tentang peristiwa tingkat langganan. 

1. Tinjau aktivitas untuk penetapan peran. Log aktivitas dapat difilter untuk operasi tertentu. 

    ![Cuplikan layar halaman Log aktivitas dengan filter yang dikonfigurasi.](../media/az104-lab02a-searchactivitylog.png)

## Membersihkan sumber daya Anda

Jika Anda bekerja dengan **langganan** Anda sendiri membutuhkan waktu satu menit untuk menghapus sumber daya lab. Ini akan memastikan sumber daya dibebankan dan biaya diminimalkan. Cara term mudah untuk menghapus sumber daya lab adalah dengan menghapus grup sumber daya lab. 

+ Di portal Azure, pilih grup sumber daya, pilih **Hapus grup** sumber daya, **Masukkan nama** grup sumber daya, lalu klik **Hapus**.
+ Menggunakan Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Menggunakan CLI, `az group delete --name resourceGroupName`.
  
## Perluas pembelajaran Anda dengan Copilot

Copilot dapat membantu Anda mempelajari cara menggunakan alat pembuatan skrip Azure. Salinan juga dapat membantu di area yang tidak tercakup dalam lab atau di mana Anda memerlukan informasi lebih lanjut. Buka browser Edge dan pilih Copilot (kanan atas) atau navigasi ke *copilot.microsoft.com*. Luangkan beberapa menit untuk mencoba perintah ini.
+ Buat dua tabel yang menyoroti perintah PowerShell dan CLI penting untuk mendapatkan informasi tentang langganan organisasi di Azure dan jelaskan setiap perintah di kolom "Penjelasan". 
+ Apa format file JSON Azure RBAC?
+ Apa langkah-langkah dasar untuk membuat peran Azure RBAC kustom?
+ Apa perbedaan antara peran Azure RBAC dan peran ID Microsoft Entra?

## Pelajari lebih lanjut dengan pelatihan mandiri

+ [Amankan sumber daya Azure Anda dengan kontrol akses berbasis peran Azure (Azure RBAC)](https://learn.microsoft.com/training/modules/secure-azure-resources-with-rbac/). Gunakan Azure RBAC untuk mengelola akses ke sumber daya di Azure.
+ [Buat peran kustom untuk sumber daya Azure dengan kontrol akses berbasis peran (RBAC)](https://learn.microsoft.com/training/modules/create-custom-azure-roles-with-rbac/). Memahami struktur definisi peran untuk kontrol akses. Mengidentifikasi properti peran yang akan digunakan yang menentukan izin peran kustom Anda. Membuat peran kustom Azure dan menetapkan ke pengguna.

## Poin penting

Selamat atas penyelesaian lab. Berikut adalah takeaway utama untuk lab ini. 

+ Grup manajemen digunakan untuk mengatur langganan secara logis.
+ Grup manajemen akar bawaan mencakup semua grup manajemen dan langganan.
+ Azure memiliki banyak peran bawaan. Anda dapat menetapkan peran ini untuk mengontrol akses ke sumber daya.
+ Anda dapat membuat peran baru atau menyesuaikan peran yang ada.
+ Peran didefinisikan dalam file berformat JSON dan mencakup *Tindakan*, *NotActions*, dan *AssignableScopes*.
+ Anda dapat menggunakan Log Aktivitas untuk memantau penetapan peran.




