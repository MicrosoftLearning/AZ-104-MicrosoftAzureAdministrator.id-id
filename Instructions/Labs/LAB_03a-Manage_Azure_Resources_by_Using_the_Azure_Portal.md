---
lab:
  title: 03a - Kelola sumber daya Azure dengan Menggunakan Portal Azure
  module: Module 03 - Azure Administration
ms.openlocfilehash: 020f28742779dab36777e2ae7b8bddb43ebb46be
ms.sourcegitcommit: be14e4ff5bc638e8aee13ec4b8be29525d404028
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 05/11/2022
ms.locfileid: "145198186"
---
# <a name="lab-03a---manage-azure-resources-by-using-the-azure-portal"></a>Lab 03a - Kelola sumber daya Azure dengan Menggunakan Portal Azure
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Anda perlu menjelajahi kemampuan administrasi Azure dasar yang terkait dengan penyediaan sumber daya dan mengaturnya berdasarkan grup sumber daya, termasuk memindahkan sumber daya di antara grup sumber daya. Anda juga ingin menjelajahi opsi untuk melindungi sumber daya disk agar tidak terhapus secara tidak sengaja, sambil tetap memungkinkan untuk memodifikasi karakteristik dan ukuran kinerjanya.

## <a name="objectives"></a>Tujuan

Di lab ini, kami akan:

+ Tugas 1: Buat grup sumber daya dan sebarkan sumber daya ke grup sumber daya
+ Tugas 2: Pindahkan sumber daya antar kelompok sumber daya
+ Tugas 3: Menerapkan dan menguji kunci sumber daya

## <a name="estimated-timing-20-minutes"></a>Perkiraan waktu: 20 menit

## <a name="architecture-diagram"></a>Diagram arsitektur

![gambar](../media/lab03a.png)

## <a name="instructions"></a>Instruksi

### <a name="exercise-1"></a>Latihan 1

#### <a name="task-1-create-resource-groups-and-deploy-resources-to-resource-groups"></a>Tugas 1: Buat grup sumber daya dan sebarkan sumber daya ke grup sumber daya

Dalam tugas ini, Anda akan menggunakan portal Azure untuk membuat grup sumber daya dan membuat disk di grup sumber daya.

1. Masuk ke [**portal Microsoft Azure**](http://portal.azure.com).

1. Di portal Azure, cari dan pilih **Disk**, klik **+ Buat** dan tentukan pengaturan berikut:

    |Pengaturan|Nilai|
    |---|---|
    |Langganan| nama langganan Azure tempat Anda membuat grup sumber daya |
    |Grup Sumber Daya| nama grup sumber daya baru **az104-03a-rg1** |
    |Nama disk| **az104-03a-disk1** |
    |Wilayah| **(AS) AS Timur** |
    |Zona ketersediaan| **Tidak ada** |
    |Jenis sumber| **Tidak ada** |

    >**Catatan**: Saat membuat sumber daya, Anda memiliki opsi untuk membuat grup sumber daya baru atau menggunakan yang sudah ada.

1. Ubah jenis dan ukuran disk masing-masing menjadi **HDD Standar** dan **32 GiB**.

1. Klik **Tinjau + Buat**, lalu klik **Buat**.

    >**Catatan**: Tunggu hingga disk dibuat. Ini seharusnya memakan waktu kurang dari satu menit.

#### <a name="task-2-move-resources-between-resource-groups"></a>Tugas 2: Pindahkan sumber daya antar kelompok sumber daya 

Dalam tugas ini, kami akan memindahkan sumber daya disk yang Anda buat di tugas sebelumnya ke grup sumber daya baru. 

1. Cari dan pilih **Grup sumber daya**. 

1. Pada panel **Grup sumber daya**, klik entri yang mewakili grup sumber daya **az104-03a-rg1** yang Anda buat di tugas sebelumnya.

1. Dari bilah **Gambaran Umum** grup sumber daya, dalam daftar sumber daya grup sumber daya, pilih entri yang mewakili disk yang baru dibuat, klik **Pindahkan** di toolbar, dan di menu drop-down, pilih **Pindahkan ke grup sumber daya lain**.

    >**Catatan**: Metode ini memungkinkan Anda untuk memindahkan beberapa sumber daya secara bersamaan. 

1. Di bawah kotak teks **Grup sumber daya**, klik **Buat baru** lalu ketik **az104-03a-rg2** di kotak teks. Pada tab Ulasan, centang kotak **Saya memahami bahwa alat dan skrip yang terkait dengan sumber daya yang dipindahkan tidak akan berfungsi sampai saya memperbaruinya untuk menggunakan ID sumber daya baru**, dan klik **Pindahkan**.

    >**Catatan**: Jangan menunggu pemindahan selesai tetapi lanjutkan ke tugas berikutnya. Perpindahan ini mungkin memakan waktu sekitar 10 menit. Anda dapat menentukan bahwa operasi telah selesai dengan memantau entri log aktivitas sumber atau grup sumber daya target. Tinjau kembali langkah ini setelah Anda menyelesaikan tugas berikutnya.

#### <a name="task-3-implement-resource-locks"></a>Tugas 3: Terapkan kunci sumber daya

Dalam tugas ini, Anda akan menerapkan kunci sumber daya ke grup sumber daya Azure yang berisi sumber daya disk.

1. Di portal Azure, cari dan pilih **Disk**, klik **+ Buat** dan tentukan pengaturan berikut:

    |Pengaturan|Nilai|
    |---|---|
    |Langganan| nama langganan yang Anda gunakan di lab ini |
    |Grup Sumber Daya| klik **buat baru** grup sumber daya dan beri nama **az104-03a-rg3** |
    |Nama disk| **az104-03a-disk2** |
    |Wilayah| nama wilayah Azure tempat Anda membuat grup sumber daya lain di lab ini |
    |Zona ketersediaan| **Tidak ada** |
    |Jenis sumber| **Tidak ada** |

1. Atur jenis dan ukuran disk masing-masing ke **HDD Standar** dan **32 GiB**.

1. Klik **Tinjau + Buat**, lalu klik **Buat**.

1. Klik **Buka sumber daya**.

1. Pada laman Gambaran Umum Disk, klik nama grup sumber daya, **az104-03a-rg3**.

1. Pada panel grup sumber daya **az104-03a-rg3**, klik **Kunci** lalu **+ Tambahkan** dan tentukan pengaturan berikut:

    |Pengaturan|Nilai|
    |---|---|
    |Nama kunci| **az104-03a-delete-lock** |
    |Jenis kunci| **Hapus** |
    
1. Klik **OK**    

1. Pada panel grup sumber daya **az104-03a-rg3**, klik **Gambaran Umum**, dalam daftar sumber daya grup sumber daya, pilih entri yang mewakili disk yang Anda buat sebelumnya dalam tugas ini, dan klik **Hapus** di toolbar. 

1. Saat diminta **Apakah Anda ingin menghapus semua sumber daya yang dipilih?** , di kotak teks **Konfirmasi hapus**, ketik **ya** dan klik **Hapus**.

1. Anda akan melihat pesan kesalahan, yang memberitahukan tentang operasi penghapusan yang gagal. 

    >**Catatan**: Seperti yang dinyatakan oleh pesan kesalahan, ini diharapkan karena kunci hapus diterapkan pada tingkat grup sumber daya.

1. Navigasikan kembali ke daftar sumber daya grup sumber daya **az104-03a-rg3** dan klik entri yang mewakili sumber daya **az104-03a-disk2**. 

1. Pada panel **az104-03a-disk2**, di bagian **Pengaturan**, klik **Ukuran + kinerja**, atur jenis dan ukuran disk ke **SSD Premium** dan **64 GiB**, masing-masing, dan klik **Ubah ukuran** untuk menerapkan perubahan. Verifikasi bahwa perubahan berhasil.

    >**Catatan**: Hal ini diharapkan, karena kunci tingkat grup sumber daya hanya berlaku untuk operasi penghapusan. 

#### <a name="clean-up-resources"></a>Membersihkan sumber daya

   >**Catatan**: Jangan hapus sumber daya yang Anda terapkan di lab ini. Anda akan menggunakannya di lab berikutnya dari modul ini. Hapus hanya kunci sumber daya yang Anda buat di lab ini.

1. Navigasikan ke panel grup sumber daya **az104-03a-rg3**, tampilkan panel **Kunci**, dan lepaskan kunci **az104-03a-delete-lock** dengan mengeklik **Hapus** tautan di sisi kanan entri kunci **Hapus**.

#### <a name="review"></a>Tinjau

Di lab ini, Anda telah:

- Membuat grup sumber daya dan menyebarkan sumber daya ke grup sumber daya
- Memindahkan sumber daya antar kelompok sumber daya
- Kunci sumber daya yang diterapkan dan diuji
