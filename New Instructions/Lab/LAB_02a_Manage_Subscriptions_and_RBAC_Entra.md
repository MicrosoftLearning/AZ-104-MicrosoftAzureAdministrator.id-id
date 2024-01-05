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

- Memberikan izin untuk mengirimkan permintaan dukungan untuk semua langganan dalam grup manajemen kepada pengguna yang ditunjuk. Izin pengguna tersebut harus dibatasi hanya untuk: 

    - Membuat tiket permintaan dukungan
    - Menampilkan grup sumber daya

## Skenario lab interaktif

Ada beberapa simulasi lab interaktif yang mungkin berguna bagi Anda untuk topik ini. Simulasi ini memungkinkan Anda mengklik skenario serupa dengan kecepatan Anda sendiri. Ada perbedaan antara simulasi interaktif dan lab ini, tetapi banyak konsep intinya sama. Langganan Azure tidak diperlukan. 

+ [Mengelola akses dengan RBAC](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2014). Tetapkan peran bawaan untuk pengguna dan pantau log aktivitas. 

+ [Mengelola langganan dan RBAC](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%202). Terapkan grup manajemen dan buat dan tetapkan peran RBAC kustom.

+ [Buka permintaan](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2022) dukungan. Tinjau opsi paket dukungan, lalu buat dan pantau permintaan dukungan, teknis atau penagihan.

## Diagram arsitektur

![Diagram tugas lab.](../media/az104-lab02a-architecture.png)

## Tugas

+ Tugas 1: Menerapkan grup manajemen.
+ Tugas 2: Tinjau dan tetapkan peran Azure bawaan.
+ Tugas 3: Buat peran RBAC kustom untuk staf dukungan.
+ Tugas 4: Uji peran kustom untuk memastikan memiliki izin yang benar
+ Tugas 5: Pantau penetapan peran dengan Log Aktivitas.

## Tugas 1: Menerapkan Grup Manajemen

Dalam tugas ini, Anda akan membuat dan mengonfigurasi grup manajemen. Grup manajemen digunakan untuk mengatur langganan secara logis. Langganan harus disegmentasi dan memungkinkan RBAC dan Azure Policy ditetapkan dan diwariskan ke grup manajemen dan langganan lainnya. Misalnya, jika organisasi Anda memiliki tim dukungan khusus untuk Eropa, Anda dapat mengatur langganan Eropa ke dalam grup manajemen untuk menyediakan akses staf dukungan ke langganan tersebut (tanpa menyediakan akses individual ke semua langganan). Dalam skenario kami, semua orang di Help Desk harus membuat permintaan dukungan di semua langganan. 

1. Masuk ke **portal Azure** - `https://portal.azure.com`.

1. Cari dan pilih `Management groups`.

1. Tinjau pesan di bagian atas panel **Grup manajemen**. Jika Anda melihat pesan yang menyatakan **Anda terdaftar sebagai admin direktori tetapi tidak memiliki izin yang diperlukan untuk mengakses grup** manajemen akar, lakukan urutan langkah-langkah berikut:

    + Di portal Azure, cari dan pilih **ID** Microsoft Entra.
    
    + Pada bilah yang menampilkan properti penyewa Anda, di menu vertikal di sisi kiri, di bagian **Kelola** , pilih **Properti**.
    
    + Pada bilah **Properti** penyewa Anda, di bagian **Manajemen akses untuk sumber daya** Azure, pilih **Ya** lalu pilih **Simpan**.
    
    + Navigasikan kembali ke bilah **Grup** manajemen dan pilih **Refresh**.

1. Pada panel **Grup manajemen**, klik **+ Buat**.

1. Buat grup manajemen dengan pengaturan berikut. Pilih **Kirim** saat Anda selesai. 

    | Pengaturan | Nilai |
    | --- | --- |
    | ID grup manajemen | `az104-mg1` |
    | Nama tampilan grup manajemen | `az104-mg1` |

1. Dalam skenario ini, semua langganan sekarang akan ditambahkan ke grup manajemen. RBAC kemudian akan diterapkan ke grup manajemen dan dilingkupkan ke Help Desk. 

## Tugas 2: Meninjau dan menetapkan peran Azure bawaan

Dalam tugas ini, Anda akan meninjau peran bawaan dan menetapkan peran Kontributor VM ke akun pengguna Anda. Azure menyediakan sejumlah besar peran bawaan[](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles).

1. Di portal, cari dan **grup manajemen az104-mg1** .

1. Pilih bilah **Kontrol akses (IAM),** lalu tab **Peran** .

1. Gulir definisi peran yang tersedia. **Lihat** peran untuk mendapatkan informasi terperinci tentang **Izin**, **JSON**, dan **Penugasan**. 

1. Pilih **+ Tambahkan**, dari menu drop-down, pilih **Tambahkan penetapan** peran. 

1. Pada bilah **Tambahkan penetapan peran**, tentukan pengaturan berikut dan klik **Berikutnya** setelah setiap langkah:

    | Pengaturan | Nilai |
    | --- | --- |
    | Pilih peran ini | **Kontributor Komputer Virtual** |
    | Tetapkan akses ke (di bawah panel Anggota) | **Pengguna, grup, atau perwakilan layanan** |
    | Pilih (+Pilih Anggota) | *akun* pengguna Anda (ditampilkan di sudut kanan atas portal) |

4. Klik **Tinjau + tetapkan** dua kali untuk membuat penetapan peran.

    >**Catatan:** Peran kontributor komputer virtual memungkinkan Anda mengelola komputer virtual, tetapi tidak mengakses sistem operasi mereka atau mengelola jaringan virtual dan akun penyimpanan yang terhubung dengannya.

    >**Catatan:** Penugasan ini mungkin tidak benar-benar memberi Anda provilese tambahan. Jika Anda sudah memiliki peran Pemilik, peran ini menyertakan semua hak istimewa yang terkait dengan peran Kontributor.


## Tugas 3: Membuat peran RBAC kustom untuk staf dukungan

Dalam tugas ini, Anda akan membuat peran RBAC kustom. Peran kustom adalah bagian inti dari menerapkan prinsip hak istimewa paling sedikit untuk lingkungan. Peran bawaan mungkin memiliki terlalu banyak izin untuk organisasi Anda. Dalam tugas ini kita akan membuat peran baru dan menghapus izin yang tidak diperlukan.

1. Di portal, cari dan pilih **grup manajemen az104-mg1** .

1. Pilih bilah **Kontrol akses (IAM),** lalu tab **Periksa akses** .

 1. Dalam kotak **Buat peran** kustom, pilih **Tambahkan**.

1. Pada tab Dasar dari **Buat peran** kustom, berikan nama `Custom Support Request`. Di bidang Deskripsi, masukkan `A custom contributor role for support requests.` 

1. Di bidang Izin garis besar, pilih **Kloning peran**. Di menu drop-down Peran untuk mengkloning, pilih **Kontributor** Permintaan Dukungan.

    ![Cuplikan layar mengkloning peran.](../media/az104-lab02a-clone-role.png)

1. Pilih tab **Izin** , lalu pilih **+ Kecualikan izin**.

1. Di bidang pencarian penyedia sumber daya, masukkan `.Support` dan pilih **Microsoft.Support**.

1. Dalam daftar izin, letakkan kotak centang di samping **Lainnya: Mendaftarkan Penyedia** Sumber Daya Dukungan lalu pilih **Tambahkan**. Peran harus diperbarui untuk menyertakan izin ini sebagai *NotAction*.

    >**Catatan:** Penyedia sumber daya Azure adalah sekumpulan operasi REST yang memungkinkan fungsionalitas untuk layanan Azure tertentu. Kami tidak ingin bantuan meja untuk dapat memiliki kemampuan ini, sehingga sedang dihapus dari peran. 

1. Pilih tab **Cakupan** yang dapat ditetapkan. Pilih **ikon Hapus** pada baris untuk langganan.

1. Pilih **+ Tambahkan cakupan** yang dapat ditetapkan. **Pilih grup manajemen az104-mg1**, lalu klik **Pilih**.

1. Pilih tab **JSON** . Tinjau JSON untuk *Tindakan*, *NotActions*, dan *AssignableScopes* yang disesuaikan dalam peran. 

1. **Pilih Tinjau + Buat**, lalu pilih **Buat**.

    >**Catatan:** Pada titik ini, Anda telah membuat peran kustom. Langkah Anda selanjutnya adalah menetapkan peran ke pengguna Help Desk. 

Tugas 4: Tetapkan dan uji peran RBAC kustom.

Dalam tugas ini, Anda menambahkan peran kustom ke pengguna uji dan mengonfirmasi izin mereka. 

1. Di portal Azure, cari dan pilih **ID** Microsoft Entra, lalu pilih bilah **Pengguna**.

    >**Catatan**: Tugas ini memerlukan akun pengguna untuk pengujian. Untuk lab ini kami akan menggunakan, **helpdesk-user1**. Silakan luangkan waktu satu menit untuk mengidentifikasi pengguna uji. Jika perlu, Anda dapat **Menambahkan** pengguna baru. Jika Anda membuat pengguna baru, wajibkan kata sandi diatur saat mereka masuk. 

1. Sebelum melanjutkan, pastikan Anda memiliki **Nama** prinsipal pengguna untuk akun pengujian Anda. Anda akan memerlukan ini untuk masuk ke portal. Gunakan ikon untuk menyalin informasi ini ke clipboard. 

1. Di portal Azure, navigasikan kembali ke **grup manajemen az104-mg1**.

1. Klik **Access Control (IAM)**, klik **+ Tambahkan** lalu **Tambahkan penetapan peran**. 

1. Pada tab **Peran** , cari `Custom Support Request`. 

    >**Catatan**: jika peran kustom Anda tidak terlihat, diperlukan waktu hingga 5 menit agar peran kustom muncul setelah pembuatan. **Refresh** halaman. 

1. Pilih **Peran** dan klik **Berikutnya**. Pada tab **Anggota** , klik **+ Pilih anggota** dan **pilih** akun **pengguna hellpdesk-user1**.  

1. Pilih **Tinjau + tetapkan** dua kali.

    >**Catatan:** Pada titik ini, Anda memiliki akun pengguna Help Desk dengan hak istimewa khusus untuk membuat tiket dukungan. Langkah Anda selanjutnya adalah menguji akun.
    
## Tugas 4: Uji peran kustom untuk memastikan memiliki izin yang benar

1. **Buka jendela browser InPrivate** dan navigasikan ke portal Azure di `https://portal.azure.com`.

1. Berikan nama prinsip pengguna untuk helpdesk-user1. Saat diminta untuk memperbarui kata sandi, ubah kata sandi untuk pengguna.

1. Di jendela **browser InPrivate**, di portal Azure, cari dan pilih **Grup** sumber daya untuk memverifikasi bahwa pengguna Help Desk dapat melihat grup sumber daya.

1. Di jendela **browser InPrivate**, di portal Azure, cari dan pilih **Semua sumber daya** untuk memverifikasi bahwa pengguna Help Desk tidak dapat melihat sumber daya individual apa pun.

1. Di jendela browser **InPrivate**, di portal Azure, telusuri dan pilih **Bantuan + dukungan** lalu klik **+ Buat permintaan dukungan**. 

    >**Catatan**: Banyak organisasi memilih untuk menyediakan semua akses administrator cloud untuk membuka kasus dukungan. Ini memungkinkan administrator untuk menyelesaikan kasus dukungan lebih cepat.

1. Di jendela browser **InPrivate**, pada tab **Deskripsi/Ringkasan Masalah** dari **Bantuan + dukungan - Blade permintaan dukungan baru**, ketik **Batas layanan dan langganan** di bidang Ringkasan dan pilih jenis masalah **Batas layanan dan langganan (kuota)**. Perhatikan bahwa langganan yang Anda gunakan di lab ini tercantum dalam daftar dropdown **Langganan**.

    >**Catatan**: Karena peran ditetapkan ke grup manajemen, semua langganan harus tersedia untuk dek bantuan. Jika Anda tidak melihat opsi **Layanan dan batas langganan (kuota)**, keluar dari portal Azure dan masuk kembali.

1. Luangkan beberapa menit untuk menjelajahi pembuatan **permintaan** dukungan Baru, tetapi jangan lanjutkan dengan membuat permintaan dukungan. Sebagai gantinya, keluar sebagai pengguna Help Desk dari portal Azure dan tutup jendela browser InPrivate.

    >**Catatan:** Anda sekarang telah memverifikasi pengguna staf dukungan yang memiliki izin yang benar. Pada titik ini Anda akan membuat grup staf dukungan dan menambahkan anggota. 

## Tugas 5: Memantau penetapan peran dengan Log Aktivitas

Dalam tugas ini, Anda melihat log aktivitas untuk menentukan apakah ada yang telah membuat peran baru. 

1. Kembali ke portal dan di **sumber daya az104-mg1** pilih **Log aktivitas**.

2. Pilih **Tambahkan filter**, pilih **Operasi**, lalu Buat penetapan **** peran.

    ![Cuplikan layar halaman Log aktivitas dengan filter yang dikonfigurasi.](../media/az104-lab02a-searchactivitylog.png)

3. Verifikasi log Aktivitas memperlihatkan aktivitas pembuatan peran. 

## Poin penting

Selamat atas penyelesaian lab. Berikut adalah takeaway utama untuk lab ini. 

+ Grup manajemen digunakan untuk mengatur langganan secara logis.
+ Azure memiliki banyak peran bawaan. Anda dapat menetapkan peran ini untuk mengontrol akses ke sumber daya.
+ Anda dapat membuat peran baru atau menyesuaikan peran yang ada.
+ Peran didefinisikan dalam file berformat JSON dan mencakup *Tindakan*, *NotActions*, dan *AssignableScopes*.
+ Anda dapat menggunakan Log Aktivitas untuk memantau penetapan peran. 

## Membersihkan sumber daya Anda

Jika Anda bekerja dengan langganan Anda sendiri membutuhkan waktu satu menit untuk menghapus sumber daya lab. Ini akan memastikan sumber daya dibebankan dan biaya diminimalkan. Cara term mudah untuk menghapus sumber daya lab adalah dengan menghapus grup sumber daya lab. 

+ Di portal Azure, pilih grup sumber daya, pilih **Hapus grup** sumber daya, **Masukkan nama** grup sumber daya, lalu klik **Hapus**.
+ Menggunakan Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Menggunakan CLI, `az group delete --name resourceGroupName`.


