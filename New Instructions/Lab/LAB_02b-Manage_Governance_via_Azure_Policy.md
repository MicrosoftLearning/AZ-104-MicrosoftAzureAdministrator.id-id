---
lab:
  title: 'Lab 02b: Mengelola Tata Kelola melalui Azure Policy'
  module: Administer Governance and Compliance
---

# Lab 02b - Mengelola Tata Kelola melalui Azure Policy

## Pengenalan lab

Di lab ini, Anda mempelajari cara menerapkan rencana tata kelola organisasi Anda. Anda mempelajari bagaimana kebijakan Azure dapat memastikan keputusan operasional diberlakukan di seluruh organisasi. Anda mempelajari cara menggunakan pemberian tag sumber daya untuk meningkatkan pelaporan. 

Lab ini memerlukan langganan Azure. Jenis langganan Anda dapat memengaruhi ketersediaan fitur di lab ini. Anda dapat mengubah wilayah, tetapi langkah-langkahnya ditulis menggunakan **US** Timur. 

## Perkiraan waktu: 30 menit

## Skenario lab

Jejak cloud organisasi Anda telah berkembang pesat dalam setahun terakhir. Selama audit baru-baru ini, Anda menemukan sejumlah besar sumber daya yang tidak memiliki pemilik, proyek, atau pusat biaya yang ditentukan. Untuk meningkatkan manajemen sumber daya Azure di organisasi Anda, Anda memutuskan untuk menerapkan fungsionalitas berikut:

- menerapkan tag sumber daya untuk melampirkan metadata penting ke sumber daya Azure

- menerapkan penggunaan tag sumber daya untuk sumber daya baru dengan menggunakan kebijakan Azure

- memperbarui sumber daya yang ada dengan tag sumber daya

## Simulasi lab interaktif

Ada beberapa simulasi lab interaktif yang mungkin berguna bagi Anda untuk topik ini. Simulasi ini memungkinkan Anda mengklik skenario serupa dengan kecepatan Anda sendiri. Ada perbedaan antara simulasi interaktif dan lab ini, tetapi banyak konsep intinya sama. Langganan Azure tidak diperlukan. 

+ [Membuat kebijakan](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2017) Azure. Buat kebijakan Azure yang membatasi sumber daya lokasi dapat ditemukan. Buat sumber daya baru dan pastikan kebijakan diberlakukan. 

+ [Mengelola tata kelola melalui kebijakan](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%203) Azure. Buat dan tetapkan tag melalui portal Azure. Buat kebijakan Azure yang memerlukan pemberian tag. Memulihkan sumber daya yang tidak sesuai.

## Diagram arsitektur

![Diagram arsitektur tugas.](../media/az104-lab02b-architecture-diagram.png)

## Tugas

+ Tugas 1: Buat dan tetapkan tag melalui portal Azure.
+ Tugas 2: Menerapkan pemberian tag melalui Azure Policy.
+ Tugas 3: Terapkan pemberian tag melalui Azure Policy.

## Tugas 1: Menetapkan tag melalui portal Azure

Dalam tugas ini, Anda akan membuat dan menetapkan tag ke grup sumber daya Azure melalui portal Microsoft Azure. Tag adalah komponen penting dari strategi tata kelola seperti yang diuraikan oleh Microsoft Well-Architected Framework dan Cloud Adoption Framework. Tag dapat memungkinkan Anda mengidentifikasi pemilik sumber daya, tanggal matahari terbenam, kontak grup, dan pasangan nama/nilai lainnya yang dianggap penting oleh organisasi Anda. Untuk latihan ini, Anda akan menetapkan tag yang mengidentifikasi peran sumber daya ('Infra' untuk 'Infrastruktur').

1. Masuk ke **portal Azure** - `https://portal.azure.com`.
      
1. Cari dan pilih **Grup sumber daya**.

1. Dari Grup sumber daya, pilih **+ Buat**.

1. Berikan nama `az104-rg2b` dan pastikan bahwa Wilayah diatur ke **US** Timur.

1. Pilih **Tinjauan + Buat**, kemudian pilih **Buat**.

1. Setelah grup sumber daya disebarkan, pilih **Buka grup** sumber daya, atau navigasikan ke grup sumber daya yang baru dibuat.

1. Pada bilah grup sumber daya, klik **Tag** di menu sebelah kiri dan buat tag baru.

1. Buat tag dengan pengaturan berikut dan terapkan perubahan Anda:

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama | `Role` |
    | Nilai | `Infra` |

1. Klik **Terapkan**. Anda sekarang telah menambahkan tag secara manual ke grup sumber daya. 

    ![Cuplikan layar halaman buat tag.](../media/az104-lab02b-manualtag.png)

## Tugas 2: Menerapkan pemberian tag melalui Azure Policy

Dalam tugas ini, Anda akan menetapkan *bawaan Memerlukan tag dan nilainya pada kebijakan sumber daya* ke grup sumber daya dan mengevaluasi hasilnya. Azure Policy dapat digunakan untuk menerapkan konfigurasi, dan dalam hal ini, tata kelola, ke sumber daya Azure Anda. 

1. Di portal Microsoft Azure, telusuri dan pilih **Kebijakan**. 

1. Di bagian **Penulisan**, klik **Definisi**. Luangkan waktu sejenak untuk menelusuri daftar definisi kebijakan bawaan yang tersedia untuk Anda gunakan. Ini mungkin juga membantu mencari `Require a tag`.

    ![Cuplikan layar definisi kebijakan.](../media/az104-lab02b-policytags.png)

1. Klik entri yang mewakili kebijakan bawaan **Memerlukan tag dan nilainya pada sumber daya** dan tinjau definisinya.

1. Pada panel **Memerlukan tag dan nilainya pada definisi** kebijakan bawaan sumber daya, klik **Tetapkan**.

1. Tentukan **Cakupan** dengan mengklik tombol elipsis dan memilih nilai berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | *langganan Anda* |
    | Grup Sumber Daya | **az-rg2b** |

    >**Catatan**: Cakupan menentukan sumber daya atau grup sumber daya tempat penetapan kebijakan berlaku. Anda dapat menetapkan kebijakan pada tingkat grup manajemen, langganan, atau grup sumber daya. Anda juga memiliki opsi untuk menentukan pengecualian, seperti langganan individual, grup sumber daya, atau sumber daya (tergantung pada cakupan penugasan).

1. Konfigurasikan properti **Dasar** penugasan dengan menentukan pengaturan berikut (biarkan yang lain dengan defaultnya):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama penetapan | `Require Cost Center tag with Default value`|
    | Deskripsi | `Require Cost Center tag with default value for all resources in the resource group`|
    | Pemberlakuan kebijakan | Diaktifkan |

    >**Catatan**: Nama **** Penugasan secara otomatis diisi dengan nama kebijakan yang Anda pilih, tetapi Anda dapat mengubahnya. **Deskripsi** bersifat opsional. **Ditetapkan oleh** diisi secara otomatis berdasarkan nama pengguna yang membuat penugasan. 

1. Klik **Berikutnya** dua kali dan atur **Parameter** ke nilai berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama Tag | `Cost Center` |
    | Nilai Tag | `Default` |

1. Klik **Berikutnya** dan tinjau tab **Remediasi** . Biarkan kotak **centang Buat Identitas** Terkelola tidak dicentang. 

    >**Catatan**: Pengaturan ini dapat digunakan saat kebijakan atau inisiatif menyertakan **efek deployIfNotExists** atau **Modifikasi** .

1. Klik **Tinjau + Buat**, lalu klik **Buat**.

    >**Catatan**: Sekarang Anda akan memverifikasi bahwa penetapan kebijakan baru berlaku dengan mencoba membuat akun Azure Storage lain di grup sumber daya tanpa secara eksplisit menambahkan tag yang diperlukan. 
    
    >**Catatan**: Mungkin diperlukan waktu antara 5 dan 15 menit agar kebijakan diterapkan.

1. Navigasi kembali ke bilah grup sumber daya yang Anda buat di tugas sebelumnya, dan pilih bilah **Tag** .

1. Pada panel grup sumber daya, klik **+ Buat** lalu cari **Akun Storage**, dan klik **+ Buat**. 

1. Pada tab **Dasar** dari bilah **Buat akun** penyimpanan, verifikasi bahwa Anda menggunakan grup sumber daya tempat kebijakan diterapkan dan tentukan pengaturan berikut (biarkan orang lain dengan defaultnya), klik **Tinjau** lalu klik **Buat**:

    | Pengaturan | Nilai |
    | --- | --- |
    | Grup sumber daya | **az104-rg2b** |
    | Nama akun penyimpanan | *kombinasi unik global antara 3 dan 24 huruf kecil dan digit, dimulai dengan huruf* |

    >**Catatan**: Anda mungkin menerima **Validasi gagal. Klik di sini untuk kesalahan detail** . Jika demikian, klik pesan kesalahan untuk mengidentifikasi alasan kegagalan dan melewati langkah berikutnya. 

1. Setelah membuat penyebaran, Anda akan melihat pesan **Penyebaran gagal** di daftar **Pemberitahuan** portal. Dari **daftar Pemberitahuan** , navigasikan ke gambaran umum penyebaran dan klik **Penyebaran gagal. Klik di sini untuk pesan detail** untuk mengidentifikasi alasan kegagalan tersebut. 

    ![Cuplikan layar kesalahan kebijakan yang tidak diizinkan.](../media/az104-lab02b-policyerror.png) 

    >**Catatan**: Verifikasi apakah pesan kesalahan menyatakan bahwa penyebaran sumber daya tidak diizinkan oleh kebijakan. 

    >**Catatan**: Dengan mengklik tab **Kesalahan** Mentah, Anda dapat menemukan detail selengkapnya tentang kesalahan, termasuk nama definisi **peran Memerlukan tag Pusat Biaya dengan nilai** Default. Penyebaran gagal karena akun penyimpanan yang Anda coba buat tidak memiliki tag bernama **Pusat** Biaya dengan nilainya diatur ke **Default**.

## Tugas 3: Menerapkan pemberian tag melalui kebijakan Azure

Dalam tugas ini, kita akan menggunakan definisi kebijakan baru untuk memulihkan sumber daya yang tidak sesuai. Ini akan menggunakan tugas remediasi sebagai bagian dari kebijakan untuk memodifikasi sumber daya yang ada agar sesuai dengan kebijakan. Dalam skenario ini, kita akan membuat sumber daya anak dari grup sumber daya mewarisi **tag Peran** yang ditentukan pada grup sumber daya.

1. Di portal Microsoft Azure, telusuri dan pilih **Kebijakan**. 

1. Di bagian **Authoring**, klik **Tugas**. 

1. Dalam daftar tugas, klik ikon elipsis di baris yang mewakili **tag Memerlukan Pusat Biaya dengan penetapan kebijakan nilai** Default dan gunakan **item menu Hapus penugasan** untuk menghapus penugasan.

1. Klik **Tetapkan kebijakan** dan tentukan **Cakupan** dengan mengeklik tombol elipsis dan memilih nilai berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup Sumber Daya | nama grup sumber daya yang berisi akun Cloud Shell yang Anda identifikasi di tugas pertama |

    ![Cuplikan layar halaman cakupan kebijakan. ](../media/az104-lab02b-policyscope2.png) 

1. Untuk menentukan **Definisi kebijakan**, klik tombol elipsis lalu cari dan pilih `Inherit a tag from the resource group if missing`.

1. Konfigurasikan properti **Dasar** yang tersisa dari penetapan dengan menetapkan setelan berikut (biarkan yang lain dengan default):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama penetapan | **Mewarisi tag Peran dan nilai Infra-nya dari grup sumber daya jika hilang**|
    | Deskripsi | **Mewarisi tag Peran dan nilai Infra-nya dari grup sumber daya jika hilang**|
    | Pemberlakuan kebijakan | Diaktifkan |

1. Klik **Berikutnya** dua kali dan atur **Parameter** ke nilai berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama Tag | `Role` |

1. Klik **Berikutnya** dan, pada tab **Perbaikan**, konfigurasikan setelan berikut (biarkan yang lain dengan default):

    | Pengaturan | Nilai |
    | --- | --- |
    | Buat tugas remediasi | diaktifkan |
    | Kebijakan yang akan diremediasi | **Mewarisi tag dari grup sumber daya jika tidak ada** |

    >**Catatan**: Definisi kebijakan ini mencakup **efek Ubah** .

    ![Cuplikan layar halaman remediasi kebijakan. ](../media/az104-lab02b-policyremediation.png) 

1. Klik **Tinjau + Buat**, lalu klik **Buat**.

    >**Catatan**: Untuk memverifikasi bahwa penetapan kebijakan baru berlaku, Anda akan membuat akun penyimpanan Azure lain di grup sumber daya yang sama tanpa secara eksplisit menambahkan tag yang diperlukan. 
    
    >**Catatan**: Mungkin diperlukan waktu antara 5 dan 15 menit agar kebijakan diterapkan.

1. Navigasikan kembali ke bilah grup sumber daya yang Anda buat di tugas pertama.

1. Pada panel grup sumber daya, klik **+ Buat** lalu cari **Akun Storage**, dan klik **+ Buat**. 

1. Pada tab **Dasar dari bilah **Buat akun** penyimpanan, verifikasi bahwa Anda menggunakan Grup Sumber Daya tempat Kebijakan diterapkan dan tentukan pengaturan berikut (biarkan orang lain dengan defaultnya) dan klik **Tinjau**:**

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama akun penyimpanan | *kombinasi unik global antara 3 dan 24 huruf kecil dan digit, dimulai dengan huruf* |

1. Verifikasi bahwa kali ini validasi lulus dan klik **Buat**.

1. Setelah akun penyimpanan baru disediakan, klik **tombol Buka sumber daya** . Pada bilah **Gambaran Umum**, perhatikan bahwa Peran** tag **dengan nilai **Infra** telah ditetapkan secara otomatis ke sumber daya.

## Poin penting

Selamat atas penyelesaian lab. Berikut adalah takeaway utama untuk lab ini. 

+ Tag Azure adalah metadata yang terdiri dari pasangan kunci-nilai. Tag menjelaskan sumber daya tertentu di lingkungan Anda. Secara khusus, pemberian tag di Azure memungkinkan Anda memberi label sumber daya Anda secara logis.
+ Azure Policy menetapkan konvensi untuk sumber daya. Definisi kebijakan menjelaskan kondisi kepatuhan sumber daya dan efek yang harus diambil jika kondisi terpenuhi. Kondisi membandingkan bidang properti sumber daya atau nilai dengan nilai yang diperlukan. Ada banyak definisi kebijakan bawaan.
+ Fitur tugas remediasi Azure Policy digunakan untuk membawa sumber daya ke kepatuhan berdasarkan definisi dan penugasan. Sumber daya yang tidak sesuai dengan penetapan definisi modifikasi atau deployIfNotExist, dapat dibawa ke kepatuhan menggunakan tugas remediasi.

## Membersihkan sumber daya Anda

Jika Anda bekerja dengan langganan Anda sendiri membutuhkan waktu satu menit untuk menghapus sumber daya lab. Ini akan memastikan sumber daya dibebankan dan biaya diminimalkan. Cara term mudah untuk menghapus sumber daya lab adalah dengan menghapus grup sumber daya lab. 

+ Di portal Azure, pilih grup sumber daya, pilih **Hapus grup** sumber daya, **Masukkan nama** grup sumber daya, lalu klik **Hapus**.

+ Menggunakan Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.

+ Menggunakan CLI, `az group delete --name resourceGroupName`.
