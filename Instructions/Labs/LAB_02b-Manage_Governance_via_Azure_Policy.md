---
lab:
  title: 02b - Mengelola Tata Kelola melalui Azure Policy
  module: Administer Governance and Compliance
---

# <a name="lab-02b---manage-governance-via-azure-policy"></a>Lab 02b - Mengelola Tata Kelola melalui Azure Policy
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Untuk meningkatkan pengelolaan sumber daya Azure di Contoso, Anda telah ditugaskan untuk menerapkan fungsionalitas berikut:

- menandai grup sumber daya yang hanya menyertakan sumber daya infrastruktur (seperti Cloud Shell akun penyimpanan)

- memastikan bahwa hanya sumber daya infrastruktur yang diberi tag dengan benar yang dapat ditambahkan ke grup sumber daya infrastruktur

- meremediasi sumber daya yang tidak patuh

**Catatan:** Tersedia **[simulasi lab interaktif](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%203)** yang memungkinkan Anda mengklik lab ini sesuai keinginan Anda. Anda mungkin menemukan sedikit perbedaan antara simulasi interaktif dan lab yang dihosting, tetapi konsep dan ide utama yang ditunjukkan sama. 

## <a name="objectives"></a>Tujuan

Di lab ini, kita akan:

+ Tugas 1: Membuat dan menetapkan tag melalui portal Microsoft Azure
+ Tugas 2: Menegakkan pemberian tag melalui Azure Policy
+ Tugas 3: Terapkan penandaan melalui kebijakan Azure

## <a name="estimated-timing-30-minutes"></a>Perkiraan waktu: 30 menit

## <a name="architecture-diagram"></a>Diagram arsitektur

![gambar](../media/lab02b.png)

## <a name="instructions"></a>Petunjuk

### <a name="exercise-1"></a>Latihan 1

#### <a name="task-1-assign-tags-via-the-azure-portal"></a>Tugas 1: Tetapkan tag melalui portal Microsoft Azure

Dalam tugas ini, Anda akan membuat dan menetapkan tag ke grup sumber daya Azure melalui portal Microsoft Azure.

1. Di portal Microsoft Azure, mulai sesi **PowerShell** dalam **Cloud Shell**.

    >**Catatan**: Jika ini pertama kalinya Anda memulai **Cloud Shell** dan Anda melihat pesan **Anda tidak memiliki penyimpanan yang terinstal**, pilih langganan yang Anda gunakan di lab ini, dan klik **Buat penyimpanan**. 

1. Dari panel Cloud Shell, jalankan yang berikut ini untuk mengidentifikasi nama akun penyimpanan yang digunakan oleh Cloud Shell:

   ```powershell
   df
   ```

1. Dalam output perintah, perhatikan bagian pertama dari jalur yang sepenuhnya memenuhi syarat yang menunjuk dudukan drive rumah Cloud Shell (ditandai di sini sebagai `xxxxxxxxxxxxxx`:

   ```
   //xxxxxxxxxxxxxx.file.core.windows.net/cloudshell   (..)  /usr/csuser/clouddrive
   ```

1. Di portal Microsoft Azure, cari dan pilih **akun Storage** dan, dalam daftar akun penyimpanan, klik entri yang mewakili akun penyimpanan yang Anda identifikasi di langkah sebelumnya.

1. Pada panel akun penyimpanan, klik tautan yang mewakili nama grup sumber daya yang berisi akun penyimpanan.

    **Catatan**: perhatikan grup sumber daya tempat akun penyimpanan berada, Anda akan membutuhkannya nanti di lab.

1. Pada panel grup sumber daya, klik **edit** di samping **Tag** untuk membuat tag baru.

1. Buat tag dengan pengaturan berikut dan Terapkan perubahan Anda:

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | **Peran** |
    | Nilai | **Infra** |

1. Navigasikan kembali ke panel akun penyimpanan. Tinjauan informasi **Gambaran Umum** dan perhatikan bahwa tag baru tidak ditetapkan secara otomatis ke akun penyimpanan. 

#### <a name="task-2-enforce-tagging-via-an-azure-policy"></a>Tugas 2: Menegakkan pemberian tag melalui Azure Policy

Dalam tugas ini, Anda akan menetapkan *bawaan Memerlukan tag dan nilainya pada kebijakan sumber daya* ke grup sumber daya dan mengevaluasi hasilnya. 

1. Di portal Microsoft Azure, telusuri dan pilih **Kebijakan**. 

1. Di bagian **Penulisan**, klik **Definisi**. Luangkan waktu sejenak untuk menelusuri daftar definisi kebijakan bawaan yang tersedia untuk Anda gunakan. Cantumkan semua kebijakan bawaan yang melibatkan penggunaan tag dengan memilih entri **Tag** (dan membatalkan pemilihan semua entri lainnya) di daftar menurun **Kategori**. 

1. Klik entri yang mewakili kebijakan bawaan **Memerlukan tag dan nilainya pada sumber daya** dan tinjauan definisinya.

1. Pada panel **Memerlukan tag dan nilainya pada definisi** kebijakan bawaan sumber daya, klik **Tetapkan**.

1. Tentukan **Cakupan** dengan mengklik tombol elipsis dan memilih nilai berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup Sumber Daya | nama grup sumber daya yang berisi akun Cloud Shell yang Anda identifikasi di tugas sebelumnya |

    >**Catatan**: Cakupan menentukan sumber daya atau grup sumber daya tempat penetapan kebijakan berlaku. Anda dapat menetapkan kebijakan pada tingkat grup manajemen, langganan, atau grup sumber daya. Anda juga memiliki opsi untuk menentukan pengecualian, seperti langganan individual, grup sumber daya, atau sumber daya (tergantung pada cakupan penugasan). 

1. Konfigurasikan properti **Dasar** penugasan dengan menentukan pengaturan berikut (biarkan yang lain dengan defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama penetapan | **Memerlukan tag Peran dengan nilai Infra**|
    | Deskripsi | **Memerlukan tag Peran dengan nilai Infra untuk semua sumber daya dalam grup sumber daya Cloud Shell**|
    | Pemberlakuan kebijakan | Aktif |

    >**Catatan**: **Nama tugas** secara otomatis diisi dengan nama kebijakan yang Anda pilih, tetapi Anda dapat mengubahnya. Anda juga dapat menambahkan **Deskripsi** opsional. **Ditetapkan oleh** diisi secara otomatis berdasarkan nama pengguna yang membuat penugasan. 

1. Klik **Berikutnya** dan atur **Parameter** ke nilai berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama Tag | **Peran** |
    | Nilai Tag | **Infra** |

1. Klik **Berikutnya** dan tinjauan tab **Perbaikan**. Biarkan kotak centang **Buat Identitas Terkelola** tidak dicentang. 

    >**Catatan**: Pengaturan ini dapat digunakan saat kebijakan atau inisiatif menyertakan efek **deployIfNotExists** atau **Modifikasi**.

1. Klik **Tinjauan + Buat**, lalu klik **Buat**.

    >**Catatan**: Sekarang Anda akan memverifikasi bahwa penetapan kebijakan baru berlaku dengan mencoba membuat akun Azure Storage lain di grup sumber daya tanpa secara eksplisit menambahkan tag yang diperlukan. 
    
    >**Catatan**: Mungkin perlu waktu antara 5 dan 15 menit agar kebijakan berlaku.

1. Navigasi kembali ke panel grup sumber daya yang menghosting akun penyimpanan yang digunakan untuk Cloud Shell home drive, yang Anda identifikasi di tugas sebelumnya.

1. Pada panel grup sumber daya, klik **+ Buat** lalu cari **Akun Storage**, dan klik **+ Buat**. 

1. Pada tab **Dasar** dari panel **Buat akun penyimpanan**, verifikasi bahwa Anda menggunakan Grup Sumber Daya tempat Policy diterapkan dan tentukan pengaturan berikut (biarkan orang lain dengan defaultnya), klik **Tinjauan + buat** lalu klik **Buat**:

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama akun penyimpanan | kombinasi unik global antara 3 dan 24 huruf kecil dan digit, dimulai dengan huruf |

1. Setelah membuat penyebaran, Anda akan melihat pesan **Penyebaran gagal** di daftar **Pemberitahuan** portal. Dari daftar **Pemberitahuan**, navigasikan ke ikhtisar penyebaran dan klik **Penyebaran gagal. Klik di sini untuk detail** pesan untuk mengidentifikasi alasan kegagalan. 

    >**Catatan**: Verifikasi apakah pesan kesalahan menyatakan bahwa penyebaran sumber daya tidak diizinkan oleh kebijakan. 

    >**Catatan**: Dengan mengklik tab **Kesalahan Mentah**, Anda dapat menemukan detail selengkapnya tentang kesalahan tersebut, termasuk nama definisi peran **Memerlukan tag Peran dengan nilai Infra**. Penyebaran gagal karena akun penyimpanan yang Anda coba buat tidak memiliki tag bernama **Peran** dengan nilainya yang diatur ke **Infra**.

#### <a name="task-3-apply-tagging-via-an-azure-policy"></a>Tugas 3: Menerapkan tag melalui kebijakan Azure

Dalam tugas ini, kita akan menggunakan definisi kebijakan yang berbeda untuk memulihkan sumber daya yang tidak patuh. 

1. Di portal Microsoft Azure, telusuri dan pilih **Kebijakan**. 

1. Di bagian **Penulisan**, klik **Tugas**. 

1. Dalam daftar penetapan, klik ikon elipsis di baris yang mewakili penetapan kebijakan **Memerlukan tag Peran dengan nilai Infra** dan gunakan item menu **Hapus penetapan** untuk menghapus penetapan.

1. Klik **Tetapkan kebijakan** dan tentukan **Cakupan** dengan mengeklik tombol elipsis dan memilih nilai berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup Sumber Daya | nama grup sumber daya yang berisi akun Cloud Shell yang Anda identifikasi di tugas pertama |

1. Untuk menentukan **Definisi kebijakan**, klik tombol elipsis lalu cari dan pilih **Warisi tag dari grup sumber daya jika hilang**.

1. Konfigurasikan properti **Dasar** yang tersisa dari penetapan dengan menetapkan setelan berikut (biarkan yang lain dengan default):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama penetapan | **Mewarisi tag Peran dan nilai Infra-nya dari grup sumber daya Cloud Shell jika hilang**|
    | Deskripsi | **Mewarisi tag Peran dan nilai Infra-nya dari grup sumber daya Cloud Shell jika hilang**|
    | Pemberlakuan kebijakan | Aktif |

1. Klik **Berikutnya** dan atur **Parameter** ke nilai berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama Tag | **Peran** |

1. Klik **Berikutnya** dan, pada tab **Perbaikan**, konfigurasikan setelan berikut (biarkan yang lain dengan default):

    | Pengaturan | Nilai |
    | --- | --- |
    | Buat tugas remediasi | diaktifkan |
    | Kebijakan yang akan diremediasi | **Mewarisi tag dari grup sumber daya jika tidak ada** |

    >**Catatan**: Definisi kebijakan ini mencakup efek **Modifikasi**.

1. Klik **Tinjauan + Buat**, lalu klik **Buat**.

    >**Catatan**: Untuk memverifikasi bahwa penetapan kebijakan baru berlaku, Anda akan membuat akun Azure Storage lain di grup sumber daya yang sama tanpa secara eksplisit menambahkan tag yang diperlukan. 
    
    >**Catatan**: Mungkin perlu waktu antara 5 dan 15 menit agar kebijakan berlaku.

1. Navigasikan kembali ke panel grup sumber daya yang menghosting akun penyimpanan yang digunakan untuk drive beranda Cloud Shell, yang Anda identifikasi di tugas pertama.

1. Pada panel grup sumber daya, klik **+ Buat** lalu cari **Akun Storage**, dan klik **+ Buat**. 

1. Pada tab **Dasar** pada panel **Buat akun penyimpanan**, verifikasi bahwa Anda menggunakan Grup Sumber Daya tempat Policy diterapkan dan tentukan setelan berikut (biarkan yang lain dengan default) dan klik **Tinjauan + buat**:

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama akun penyimpanan | kombinasi unik global antara 3 dan 24 huruf kecil dan digit, dimulai dengan huruf |

1. Verifikasi bahwa kali ini validasi lulus dan klik **Buat**.

1. Setelah akun penyimpanan baru disediakan, klik tombol **Buka sumber daya** dan, pada panel **Gambaran Umum** akun penyimpanan yang baru dibuat, perhatikan bahwa **Peran** tag dengan nilai **Infra** telah secara otomatis ditetapkan ke sumber daya.

#### <a name="task-4-clean-up-resources"></a>Tugas 4: Bersihkan sumber daya

   >**Catatan**: Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak digunakan memastikan Anda tidak akan melihat biaya tak terduga, meskipun perlu diingat bahwa kebijakan Azure tidak dikenakan biaya tambahan.
   
   >**Catatan**:  Jangan khawatir jika sumber daya lab tidak dapat segera dihapus. Terkadang sumber daya memiliki dependensi dan membutuhkan waktu lebih lama untuk dihapus. Ini adalah tugas Administrator yang umum untuk memantau penggunaan sumber daya, jadi tinjauan sumber daya Anda secara berkala di Portal untuk melihat bagaimana pembersihannya. 

1. Di portal, telusuri dan pilih **Kebijakan**.

1. Di bagian **Penulisan**, klik **Penugasan**, klik ikon elipsis di sebelah kanan tugas yang Anda buat di tugas sebelumnya dan klik **Hapus penugasan**. 

1. Di portal, telusuri dan pilih **Akun penyimpanan**.

1. Dalam daftar akun penyimpanan, pilih grup sumber daya yang sesuai dengan akun penyimpanan yang Anda buat di tugas terakhir lab ini. Pilih **Tag** dan klik **Hapus** (Tempat sampah di sebelah kanan) ke tag **Role:Infra** dan tekan **Terapkan**. 

1. Klik **Gambaran Umum** dan klik **Hapus** di bagian atas panel akun penyimpanan. Saat dimintai konfirmasi, di panel **Hapus akun penyimpanan**, ketik nama akun penyimpanan untuk mengonfirmasi dan klik **Hapus**. 

#### <a name="review"></a>Tinjau

Di lab ini, Anda telah:

- Membuat dan menetapkan tag melalui portal Microsoft Azure
- Menegakkan pemberian tag melalui Azure Policy
- Menerapkan pemberian tag melalui kebijakan Azure
