---
lab:
  title: 'Lab 01: Mengelola Identitas Azure Active Directory'
  module: Administer Identity
---

# Lab 01 - Mengelola Identitas Azure Active Directory

# Panduan lab siswa

## Skenario laboratorium

Untuk mengizinkan pengguna Contoso melakukan autentikasi menggunakan Azure AD, Anda telah ditugaskan untuk memprovisikan akun pengguna dan grup. Keanggotaan grup harus diperbarui secara otomatis berdasarkan jabatan pengguna. Anda juga perlu membuat penyewa Azure AD uji coba dengan akun pengguna uji coba dan memberi akun tersebut izin terbatas ke sumber daya dalam langganan Azure Contoso.

**Catatan:** Tersedia **[simulasi lab interaktif](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%201)** yang memungkinkan Anda mengklik lab ini sesuai keinginan Anda. Anda mungkin menemukan sedikit perbedaan antara simulasi interaktif dan lab yang dihosting, tetapi konsep dan ide utama yang ditunjukkan sama.

## Tujuan

Di lab ini Anda akan:

+ Tugas 1: Membuat dan mengonfigurasi pengguna Microsoft Azure Active Directory
+ Tugas 2: Membuat grup Azure ACTIVE Directory dengan keanggotaan yang ditetapkan dan dinamis
+ Tugas 3: Membuat penyewa Azure Active Directory (AD) (Opsional - masalah lingkungan lab)
+ Tugas 4: Mengelola pengguna tamu Azure Active Directory (Opsional - masalah lingkungan lab)

## Perkiraan waktu: 30 menit

## Diagram arsitektur
![gambar](../media/lab01.png)

### Petunjuk

## Latihan 1

## Tugas 1: Membuat dan mengonfigurasi pengguna Microsoft Azure Active Directory

Dalam tugas ini, Anda akan membuat dan mengonfigurasi pengguna Azure AD.

>**Catatan**: Jika sebelumnya Anda telah menggunakan lisensi Uji Coba untuk Azure AD Premium pada Penyewa Azure AD ini, Anda akan memerlukan Penyewa Azure AD baru dan melakukan Tugas 2 setelah Tugas 3 di penyewa Azure AD baru tersebut.

1. Masuk ke [portal Azure](https://portal.azure.com).

1. Di portal Microsoft Azure, cari dan pilih **Microsoft Azure Active Directory**.

1. Pada panel Azure Active Directory, gulir ke bawah ke bagian **Kelola**, klik **Pengaturan pengguna**, dan tinjau opsi konfigurasi yang tersedia.

1. Pada panel Azure Active Directory, di bagian **Kelola**, klik **Pengguna**, lalu klik akun pengguna Anda untuk menampilkan pengaturan **Profilnya**. 

1. Klik **Edit properti**, lalu di tab **Pengaturan**, atur **Lokasi** penggunaan ke **Amerika Serikat** dan klik **Simpan** untuk menerapkan perubahan.

    >**Catatan**: Ini diperlukan untuk menetapkan lisensi Azure AD Premium P2 ke akun pengguna Anda nanti di lab ini.

1. Navigasi kembali ke panel **Pengguna - Semua pengguna**, lalu klik **+ Pengguna baru**.

1. Buat pengguna baru dengan pengaturan berikut (biarkan yang lain dengan default):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama principal pengguna | **az104-01a-aaduser1** |
    | Nama tampilan | **az104-01a-aaduser1** |
    | Buat Kata Sandi secara otomatis | batal pilih |
    | Kata sandi awal | **Berikan kata sandi yang aman** |
    | Jabatan pekerjaan (tab Properti) | **Administrator Cloud** |
    | Departemen (tab Properti) | **TI** |
    | Lokasi penggunaan (tab Properti) | **Amerika Serikat** |

    >**Catatan**: **Salin ke clipboard** Nama** Prinsipal Pengguna lengkap **(nama pengguna plus domain). Anda akan membutuhkannya nanti dalam tugas ini.

1. Dalam daftar pengguna, klik akun pengguna yang baru dibuat untuk menampilkan panelnya.

1. Tinjau opsi yang tersedia di bagian **Kelola** dan perhatikan bahwa Anda dapat mengidentifikasi peran Azure AD yang ditetapkan ke akun pengguna serta izin akun pengguna ke sumber daya Azure.

1. Di bagian **Kelola**, klik **Peran yang ditetapkan**, lalu klik tombol **+ Tambahkan tugas** dan tetapkan peran **Administrator pengguna** ke **az104 -01a-aaduser1**.

    >**Catatan**: Anda juga memiliki opsi untuk menetapkan peran Azure ACTIVE Directory saat menyediakan pengguna baru.

1. Buka jendela browser **InPrivate** dan masuk ke [portal Azure](https://portal.azure.com) menggunakan akun pengguna yang baru dibuat. Ketika diminta untuk memperbarui kata sandi, ubah kata sandi menjadi kata sandi aman yang Anda pilih. 

    >**Catatan**: Daripada mengetik nama pengguna (termasuk nama domain), Anda dapat menempelkan konten Clipboard.

1. Di jendela browser **InPrivate**, di portal Microsoft Azure, cari dan pilih **Azure Active Directory**.

    >**Catatan**: Meskipun akun pengguna ini dapat mengakses penyewa Azure Active Directory, akun tersebut tidak memiliki akses apa pun ke sumber daya Azure. Ini diharapkan, karena akses tersebut perlu diberikan secara eksplisit dengan menggunakan Azure Role-Based Access Control. 

1. Di jendela browser **InPrivate**, pada panel Azure AD, gulir ke bawah ke bagian **Kelola**, klik **Pengaturan pengguna**, dan perhatikan bahwa Anda tidak memiliki izin untuk mengubah opsi konfigurasi apa pun.

1. Di jendela browser **InPrivate**, pada panel Azure AD, di bagian **Kelola**, klik **Pengguna**, lalu klik **+ Pengguna baru**.

1. Buat pengguna baru dengan pengaturan berikut (biarkan yang lain dengan default):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama principal pengguna | **az104-01a-aaduser2** |
    | Nama tampilan | **az104-01a-aaduser2** |
    | Buat Kata Sandi secara otomatis | batal pilih  |
    | Kata sandi awal | **Berikan kata sandi yang aman** |
    | Judul pekerjaan | **Administrator Sistem** |
    | Departemen | **TI** |
    | Lokasi penggunaan | **Amerika Serikat** |
    
1. Keluar sebagai pengguna az104-01a-aaduser1 dari portal Microsoft Azure dan tutup jendela browser InPrivate.

## Tugas 2: Membuat grup Azure ACTIVE Directory dengan keanggotaan yang ditetapkan dan dinamis

Dalam tugas ini, Anda akan membuat grup Azure Active Directory dengan keanggotaan yang ditetapkan dan dinamis.

1. Kembali ke portal Microsoft Azure tempat Anda masuk dengan **akun pengguna** Anda, navigasikan kembali ke panel **Gambaran Umum** penyewa Azure AD dan, di bagian **Kelola**, klik **Lisensi**.

    >**Catatan**: Lisensi Azure AD Premium P1 atau P2 diperlukan untuk menerapkan grup dinamis.

1. Di bagian **Kelola**, klik **Semua produk**.

1. Klik **+ Coba/Beli** dan aktifkan uji coba gratis Azure AD Premium P2.

1. Refresh jendela browser untuk memverifikasi bahwa aktivasi berhasil. 

    >**Catatan**: Dibutuhkan waktu hingga 10 menit agar lisensi diaktifkan. Lanjutkan menyegarkan halaman hingga halaman muncul. Jangan lanjutkan sampai lisensi diaktifkan.

1. Dari bilah **Lisensi - Semua produk** , pilih **entri Microsoft Entra ID P2** , dan tetapkan semua opsi lisensi ke akun pengguna Anda dan dua akun pengguna yang baru dibuat.

1. Di portal Microsoft Azure, navigasikan kembali ke panel penyewa Azure AD dan klik **Grup**.

1. Gunakan tombol **+ Grup baru** untuk membuat grup baru dengan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Jenis grup | **Keamanan** |
    | Nama grup | **Administrator Cloud TI** |
    | Deskripsi grup | **Administrator cloud TI Contoso** |
    | Jenis keanggotaan | **Pengguna Dinamis** |

    >**Catatan**: Jika **daftar drop-down Jenis** keanggotaan berwarna abu-abu, tunggu beberapa menit dan refresh halaman browser.

1. Klik **Tambahkan kueri dinamis**.

1. Pada tab **Konfigurasi Aturan** dari panel **Aturan keanggotaan dinamis**, buat aturan baru dengan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Properti | **jobTitle** |
    | Operator | **Sama dengan** |
    | Nilai | **Administrator Cloud** |

1. Simpan aturan dengan mengklik **+Tambahkan ekspresi** dan **Simpan**. Kembali ke bilah **Grup Baru**, klik **Buat**. 

1. Kembali ke panel **Grup - Semua grup** penyewa Azure AD, klik tombol **+ Grup baru** dan buat grup baru dengan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Jenis grup | **Keamanan** |
    | Nama grup | **Administrator Sistem TI** |
    | Deskripsi grup | **Administrator sistem TI Contoso** |
    | Jenis keanggotaan | **Pengguna Dinamis** |

1. Klik **Tambahkan kueri dinamis**.

1. Pada tab **Konfigurasi Aturan** dari panel **Aturan keanggotaan dinamis**, buat aturan baru dengan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Properti | **jobTitle** |
    | Operator | **Sama dengan** |
    | Nilai | **Administrator Sistem** |

1. Simpan aturan dengan mengklik **+Tambahkan ekspresi** dan **Simpan**. Kembali ke bilah **Grup Baru**, klik **Buat**. 

1. Kembali ke panel **Grup - Semua grup** penyewa Azure AD, klik tombol **+ Grup baru**, dan buat grup baru dengan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Jenis grup | **Keamanan** |
    | Nama grup | **Administrator Lab TI** |
    | Deskripsi grup | **Administrator Lab TI Contoso** |
    | Jenis keanggotaan | **Ditetapkan** |
    
1. Klik **Tidak ada anggota yang dipilih**.

1. Dari panel **Tambahkan anggota**, cari dan pilih **grup Administrator Cloud TI** dan **Administrator Sistem TI** dan, kembali ke panel **Grup Baru**, klik **Buat**.

1. Kembali ke panel **Grup - Semua grup**, klik entri yang mewakili grup **Administrator Cloud TI** dan, lalu tampilkan panel **Anggotanya**. Verifikasi bahwa **az104-01a-aaduser1** muncul dalam daftar anggota grup.

    >**Catatan**: Anda mungkin mengalami penundaan dengan pembaruan grup keanggotaan dinamis. Untuk mempercepat pembaruan, navigasikan ke panel grup, tampilkan panel **Aturan keanggotaan Dinamis**, **Edit** aturan yang tercantum dalam kotak teks **Sintaks aturan** dengan menambahkan spasi kosong di akhir, dan **Simpan** perubahan.

1. Navigasikan kembali ke panel **Grup - Semua grup**, klik entri yang mewakili grup **IT System Administrators**, lalu tampilkan panel **Anggota**. Verifikasi bahwa **az104-01a-aaduser2** muncul dalam daftar anggota grup.

## Tugas 3: Membuat penyewa Azure Active Directory (AD) (Opsional - Masalah lingkungan lab)

Dalam tugas ini, Anda akan membuat penyewa Azure AD baru.
    
1. Di portal Microsoft Azure, cari dan pilih **Microsoft Azure Active Directory**.

    >**Catatan**: Ada masalah yang diketahui dengan verifikasi Captcha di lingkungan lab. Jika Anda menerima kesalahan **Pembuatan gagal. Terlalu banyak permintaan, silakan coba nanti**, lakukan hal berikut:<br>
    - Coba pembuatan beberapa kali.<br>
    - Periksa bagian **Kelola penyewa** untuk memastikan penyewa tidak dibuat di latar belakang. <br>
    - Buka jendela InPrivate** baru **dan gunakan Portal Microsoft Azure dan coba buat penyewa dari sana.<br>
     Ajukan masalah dengan pelatih, lalu gunakan **[simulasi](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%201)** lab interaktif untuk melihat langkah-langkahnya. <br>
    - Anda dapat mencoba tugas ini nanti, tetapi membuat penyewa tidak diperlukan di lab lain. 

1. Klik **Kelola penyewa**, lalu pada layar berikutnya, klik **+ Buat**, dan tentukan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Jenis direktori | **Azure Active Directory** |
    
1. Klik **Berikutnya : Konfigurasi**

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama organisasi | **Lab Contoso** |
    | Nama domain awal | semua nama DNS valid yang terdiri dari huruf kecil dan digit serta dimulai dengan huruf | 
    | Negara/Wilayah | **Amerika Serikat** |

   > **Catatan**: Nama **** domain awal tidak boleh menjadi nama yang sah yang berpotensi cocok dengan organisasi Anda atau yang lain. Tanda centang hijau di kotak teks **Nama domain awal** akan menunjukkan bahwa nama domain yang Anda ketikkan valid dan unik.

1. Klik **Tinjau + buat** lalu klik **Buat**.

1. Tampilkan bilah penyewa Azure AD yang baru dibuat dengan menggunakan **tautan Klik di sini untuk menavigasi ke penyewa baru Anda: Contoso Lab** atau **tombol Direktori + Langganan** (langsung di sebelah kanan tombol Cloud Shell) di toolbar portal Azure.

## Tugas 4: Mengelola pengguna tamu Microsoft Azure ACTIVE Directory.

Dalam tugas ini, Anda akan membuat Azure AD pengguna tamu dan memberi mereka akses ke sumber daya dalam langganan Azure.

1. Di portal Microsoft Azure menampilkan penyewa Contoso Lab Azure AD, di bagian **Kelola**, klik **Pengguna**, lalu klik **+ Pengguna baru**.

1. Buat pengguna baru dengan pengaturan berikut (biarkan yang lain dengan default):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama principal pengguna | **az104-01b-aaduser1** |
    | Nama tampilan | **az104-01b-aaduser1** |
    | Buat Kata Sandi secara otomatis | batal pilih  |
    | Kata sandi awal | **Berikan kata sandi yang aman** |
    | Judul pekerjaan | **Administrator Sistem** |
    | Departemen | **TI** |

1. Klik profil yang baru dibuat.

    >**Catatan**: **Salin ke clipboard** Nama** Prinsipal Pengguna lengkap **(nama pengguna plus domain). Anda akan membutuhkannya nanti dalam tugas ini.

1. Kembali ke penyewa pertama yang Anda buat sebelumnya. Lakukan ini dengan menggunakan tombol **Direktori + Langganan** (langsung di sebelah kanan tombol Cloud Shell) di toolbar portal Azure.

1. Arahkan kembali ke blade **Pengguna - Semua pengguna**, lalu klik **+ Undang pengguna eksternal**.

1. Undang pengguna tamu baru dengan pengaturan berikut (biarkan yang lain dengan default):

    | Pengaturan | Nilai |
    | --- | --- |
    | email | Nama Prinsipal Pengguna yang Anda salin sebelumnya dalam tugas ini |
    | Nama Tampilan (tab Properti)  | **az104-01b-aaduser1** |
    | Jabatan pekerjaan (tab Properti) | **Administrator Lab** |
    | Departemen (tab Properti) | **TI** |
    | Lokasi penggunaan (tab Properti) | **Amerika Serikat** |

1. Klik **Undang**. 

1. Kembali ke panel **Pengguna - Semua pengguna**, klik entri yang mewakili akun pengguna tamu yang baru dibuat.

1. Pada panel **az104-01b-aaduser1 - Profil**, klik **Grup**.

1. Klik **+ Tambahkan keanggotaan** dan tambahkan akun pengguna tamu ke grup **Administrator Lab TI**.


## Tugas 5: Menghapus sumber daya

> **Catatan**: Ingatlah untuk menghapus sumber daya Azure yang baru dibuat yang tidak lagi Anda gunakan. Menghapus sumber daya yang tidak terpakai memastikan Anda tidak akan dikenakan biaya tidak terduga. Meskipun, dalam hal ini, tidak ada biaya tambahan yang terkait dengan penyewa Azure Active Directory dan objeknya, Anda mungkin ingin mempertimbangkan untuk menghapus akun pengguna, akun grup, dan penyewa Azure Active Directory yang Anda buat di lab ini.

 > **Catatan**: Jangan khawatir jika sumber daya lab tidak dapat segera dihapus. Terkadang sumber daya memiliki dependensi dan membutuhkan waktu lebih lama untuk dihapus. Ini adalah tugas Administrator yang umum untuk memantau penggunaan sumber daya, jadi tinjau sumber daya Anda secara berkala di Portal untuk melihat bagaimana pembersihannya. 

1. Di **Portal Microsoft Azure**, cari **Azure Active Directory** di bar pencarian. Dalam **Azure Active Directory** di bawah **Kelola** pilih **Lisensi**. Setelah berada di **Lisensi** di bawah **Kelola** pilih **Semua Produk** lalu pilih **item Azure Active Directory Premium P2** dalam daftar. Lanjutkan dengan memilih **Pengguna Berlisensi**. Pilih akun pengguna **az104-01a-aaduser1** dan **az104-01a-aaduser2** tempat Anda menetapkan lisensi di lab ini, klik **Hapus lisensi**, dan, saat diminta untuk mengonfirmasi, klik **Ya**.

1. Di portal Microsoft Azure, navigasikan ke panel **Pengguna - Semua pengguna**, klik entri yang mewakili akun pengguna tamu **az104-01b-aaduser1**, di **az104-01b-aaduser1 - Profil** panel klik **Hapus**, dan, saat diminta untuk mengonfirmasi, klik **OK**.

1. Ulangi urutan langkah yang sama untuk menghapus akun pengguna yang tersisa yang Anda buat di lab ini.

1. Navigasi ke panel **Grup - Semua grup**, pilih grup yang Anda buat di lab ini, klik **Hapus**, dan, saat diminta untuk mengonfirmasi, klik **OK**.

1. Di portal Azure, tampilkan panel penyewa Azure AD Lab Contoso dengan menggunakan tombol **Direktori + Langganan** (langsung di sebelah kanan tombol Cloud Shell) di toolbar portal Microsoft Azure.

1. Navigasikan ke panel **Pengguna - Semua pengguna**, klik entri yang mewakili akun pengguna **az104-01b-aaduser1**, pada panel **az104-01b-aaduser1 - Profil** klik **Hapus**, dan, saat diminta untuk mengonfirmasi, klik **OK**.

1. Navigasi ke panel **Contoso Lab - Gambaran Umum** penyewa Contoso Lab Azure AD, klik **Kelola penyewa** lalu pada layar berikutnya, pilih kotak di samping **Contoso Lab**, klik **Hapus**, pada **Hapus penyewa 'Contoso Labs'?** panel, klik tautan **Dapatkan izin untuk menghapus sumber daya Azure**, pada panel **Properti** Azure Active Directory, setel **Pengelolaan akses untuk sumber daya Azure** ke **Ya ** dan klik **Simpan**.

1. Navigasi kembali ke panel **Hapus penyewa 'Contoso Lab'** dan klik **Refresh**, klik **Hapus**.

> **Catatan**: Jika penyewa memiliki lisensi uji coba, Maka Anda harus menunggu kedaluwarsa lisensi uji coba sebelum Anda dapat menghapus penyewa. Ini tidak akan dikenakan biaya tambahan.

#### Tinjau

Di lab ini, Anda telah:

- Membuat dan mengonfigurasi pengguna Azure AD
- Membuat grup Azure AD dengan keanggotaan yang ditetapkan dan dinamis
- Membuat penyewa Azure Active Directory (AD)
- Mengelola Pengguna tamu Azure AD 
