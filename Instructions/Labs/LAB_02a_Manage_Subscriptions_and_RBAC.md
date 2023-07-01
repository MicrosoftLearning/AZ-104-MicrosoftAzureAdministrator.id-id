---
lab:
  title: 'Lab 02a: Mengelola Langganan dan RBAC'
  module: Administer Governance and Compliance
---

# Lab 02a - Mengelola Langganan dan RBAC
# Panduan lab siswa

## Persyaratan laboratorium

Lab ini memerlukan izin untuk membuat pengguna Azure Active Directory (Azure AD), membuat peran Kontrol Akses Berbasis Peran (RBAC) Azure kustom, dan menetapkan peran ini ke pengguna Azure AD. Tidak semua hoster lab dapat menyediakan kemampuan ini. Tanyakan kepada instruktur Anda untuk ketersediaan lab ini.

## Skenario lab

Untuk meningkatkan pengelolaan sumber daya Azure di Contoso, Anda telah ditugaskan untuk menerapkan fungsionalitas berikut:

- membuat grup manajemen yang akan mencakup semua langganan Azure Contoso

- memberikan izin untuk mengirimkan permintaan dukungan untuk semua langganan dalam grup manajemen ke pengguna Azure Active Directory yang ditunjuk. Izin pengguna tersebut harus dibatasi hanya untuk: 

    - membuat tiket permintaan dukungan
    - melihat grup sumber daya

**Catatan:** Tersedia **[simulasi lab interaktif](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%202)** yang memungkinkan Anda mengklik lab ini sesuai keinginan Anda. Anda mungkin menemukan sedikit perbedaan antara simulasi interaktif dan lab yang dihosting, tetapi konsep dan ide utama yang ditunjukkan sama.

## Tujuan

Di lab ini Anda akan:

+ Tugas 1: Menerapkan Grup Manajemen
+ Tugas 2: Membuat peran RBAC kustom 
+ Tugas 3: Menetapkan peran RBAC


## Perkiraan waktu: 30 menit

## Diagram arsitektur

![gambar](../media/lab02a.png)


### Petunjuk

## Latihan 1

## Tugas 1: Menerapkan Grup Manajemen

Dalam tugas ini, Anda akan membuat dan mengonfigurasi grup manajemen. 

1. Masuk ke [**portal Microsoft Azure**](http://portal.azure.com).

1. Telusuri dan pilih **Grup manajemen** untuk menavigasi ke panel **Grup manajemen**.

1. Tinjauan pesan di bagian atas panel **Grup manajemen**. Jika Anda melihat pesan yang menyatakan **Anda terdaftar sebagai admin direktori tetapi tidak memiliki izin yang diperlukan untuk mengakses grup manajemen root**, lakukan urutan langkah-langkah berikut:

    1. Di portal Microsoft Azure, cari dan pilih **Azure Active Directory**.
    
    1.  Pada panel yang menampilkan properti penyewa Azure Active Directory Anda, di menu vertikal di sisi kiri, di bagian **Kelola**, pilih **Properti**.
    
    1.  Pada panel **Properti** penyewa Azure Active Directory Anda, di bagian **Pengelolaan akses untuk sumber daya Azure**, pilih **Ya** lalu pilih **Simpan**.
    
    1.  Navigasikan kembali ke panel **Grup manajemen**, dan pilih **Refresh**.

1. Pada panel **Grup manajemen**, klik **+ Buat**.

    >**Catatan**: Jika Anda belum pernah membuat Grup Manajemen, pilih **Mulai gunakan grup manajemen**

1. Buat grup manajemen dengan pengaturan berikut:

    | Pengaturan | Nilai |
    | --- | --- |
    | ID grup manajemen | **az104-02-mg1** |
    | Nama tampilan grup manajemen | **az104-02-mg1** |

1. Dalam daftar grup manajemen, klik entri yang mewakili grup manajemen yang baru dibuat.

1. Pada panel **az104-02-mg1**, klik **Langganan**. 

1. Pada panel **az104-02-mg1 \| Langganan**, klik **+ Tambahkan**, pada panel **Tambahkan langganan**, di **Langganan** daftar dropdown, pilih langganan yang Anda gunakan di lab ini dan klik **Simpan**.

    >**Catatan**: Pada panel **az104-02-mg1 \| Langganan**, salin ID langganan Azure Anda ke Clipboard. Anda akan membutuhkannya di tugas berikutnya.

## Tugas 2: Membuat peran RBAC kustom

Dalam tugas ini, Anda akan membuat definisi peran RBAC kustom.

1. Dari komputer lab, buka file **\\Allfiles\\Labs\\02\\az104-02a-customRoleDefinition.json** di Notepad dan tinjau isinya:

   ```json
   {
      "Name": "Support Request Contributor (Custom)",
      "IsCustom": true,
      "Description": "Allows to create support requests",
      "Actions": [
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Support/*"
      ],
      "NotActions": [
      ],
      "AssignableScopes": [
          "/providers/Microsoft.Management/managementGroups/az104-02-mg1",
          "/subscriptions/SUBSCRIPTION_ID"
      ]
   }
   ```
    > **Catatan**: Jika Anda tidak yakin di mana file disimpan secara lokal di lingkungan lab Anda, silakan tanyakan kepada instruktur Anda.

1. Ganti `SUBSCRIPTION_ID` tempat penampung di file JSON dengan ID langganan yang Anda salin ke Clipboard dan simpan perubahannya.

1. Di portal Azure, buka panel **Cloud Shell** dengan mengeklik ikon panel alat langsung di sebelah kanan kotak teks penelusuran.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**. 

    >**Catatan**: Jika ini pertama kalinya Anda memulai **Cloud Shell** dan Anda melihat pesan **Anda tidak memiliki penyimpanan yang terinstal**, pilih langganan yang Anda gunakan di lab ini, dan klik **Buat penyimpanan**. 

1. Di panel alat panel Cloud Shell, klik ikon **Unggah/Unduh file**, di menu dropdown klik **Unggah**, dan unggah file **\\Allfiles\\Lab\\02\\az104-02a-customRoleDefinition.json** ke dalam direktori beranda Cloud Shell.

1. Dari panel Cloud Shell, jalankan perintah berikut untuk membuat definisi peran kustom:

   ```powershell
   New-AzRoleDefinition -InputFile $HOME/az104-02a-customRoleDefinition.json
   ```

1. Tutup panel Cloud Shell.

## Tugas 3: Menetapkan peran RBAC

Dalam tugas ini, Anda akan membuat pengguna Azure Active Directory, menetapkan peran RBAC yang Anda buat di tugas sebelumnya kepada pengguna tersebut, dan memverifikasi bahwa pengguna dapat melakukan tugas yang ditentukan dalam definisi peran RBAC.

1. Di portal Azure, cari dan pilih **Azure Active Directory**, pada panel Azure Active Directory, klik **Pengguna**, lalu klik **+ Pengguna baru**.

1. Buat pengguna baru dengan pengaturan berikut (biarkan yang lain dengan default):

    | Pengaturan | Nilai |
    | --- | --- |
    | Nama pengguna | **az104-02-aaduser1**|
    | Nama | **az104-02-aaduser1**|
    | Izinkan saya membuat kata sandi | diaktifkan |
    | Kata sandi awal | **Berikan kata sandi yang aman** |

    >**Catatan**: **Salin ke clipboard** **Nama pengguna** lengkap. Anda akan membutuhkannya nanti di lab ini.

1. Di portal Azure, navigasikan kembali ke grup pengelolaan **az104-02-mg1** dan tampilkan **detailnya**.

1. Klik **Access Control (IAM)** , klik **+ Tambahkan** lalu **Tambahkan penetapan peran**. Pada tab **Peran**, cari **Kontributor Permintaan Dukungan (Kustom)** . 

    >**Catatan**: jika peran kustom Anda tidak terlihat, diperlukan waktu hingga 10 menit agar peran kustom muncul setelah dibuat.

1. Pilih **Peran** dan klik **Berikutnya**. Pada tab **Anggota**, klik **+ Pilih anggota** dan **pilih** akun pengguna az104-****************** *******.**********.onmicrosoft.com. Klik **Berikutnya** lalu **Tinjauan dan tetapkan**.

1. Buka jendela browser **InPrivate** dan masuk ke [portal Azure](https://portal.azure.com) menggunakan akun pengguna yang baru dibuat. Saat diminta untuk memperbarui kata sandi, ubah kata sandi untuk pengguna.

    >**Catatan**: Daripada mengetik nama pengguna, Anda dapat menempelkan konten Clipboard.

1. Di jendela browser **InPrivate**, di portal Azure, telusuri dan pilih **Grup sumber daya** untuk memverifikasi bahwa pengguna az104-02-aaduser1 dapat melihat semua grup sumber daya.

1. Di jendela browser **InPrivate**, di portal Azure, telusuri dan pilih **Semua sumber daya** untuk memverifikasi bahwa pengguna az104-02-aaduser1 tidak dapat melihat sumber daya apa pun.

1. Di jendela browser **InPrivate**, di portal Azure, telusuri dan pilih **Bantuan + dukungan** lalu klik **+ Buat permintaan dukungan**. 

1. Di jendela browser **InPrivate**, pada tab **Deskripsi/Ringkasan Masalah** dari **Bantuan + dukungan - Blade permintaan dukungan baru**, ketik **Batas layanan dan langganan** di bidang Ringkasan dan pilih jenis masalah **Batas layanan dan langganan (kuota)** . Perhatikan bahwa langganan yang Anda gunakan di lab ini tercantum dalam daftar dropdown **Langganan**.

    >**Catatan**: Kehadiran langganan yang Anda gunakan di lab ini dalam daftar menurun **Langganan** menunjukkan bahwa akun yang Anda gunakan memiliki izin yang diperlukan untuk membuat permintaan dukungan khusus langganan.

    >**Catatan**: Jika Anda tidak melihat opsi **Layanan dan batas langganan (kuota)** , keluar dari portal Azure dan masuk kembali.

1. Jangan lanjutkan dengan membuat permintaan dukungan. Sebagai gantinya, keluar sebagai pengguna az104-02-aaduser1 dari portal Azure dan tutup jendela browser InPrivate.

## Tugas 4: Membersihkan sumber daya

   >**Catatan**: Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak digunakan memastikan Anda tidak akan melihat biaya tak terduga, meskipun, sumber daya yang dibuat di lab ini tidak dikenakan biaya tambahan.

   >**Catatan**: Jangan khawatir jika sumber daya lab tidak dapat segera dihapus. Terkadang sumber daya memiliki dependensi dan membutuhkan waktu lebih lama untuk dihapus. Memantau penggunaan sumber daya adalah tugas Administrator yang umum, jadi tinjauan sumber daya Anda secara berkala di Portal untuk melihat bagaimana pembersihannya.

1. Di portal Azure, cari dan pilih **Azure Active Directory**, pada panel Azure Active Directory, klik **Pengguna**.

1. Pada panel **Pengguna - Semua pengguna**, klik **az104-02-aaduser1**.

1. Pada panel **az104-02-aaduser1 - Profile**, salin nilai atribut **Object ID**.

1. Di portal Azure, mulai sesi **PowerShell** dalam **Cloud Shell**.

1. Dari panel Cloud Shell, jalankan yang berikut ini untuk menghapus penetapan definisi peran khusus (ganti `[object_ID]` placeholder dengan nilai atribut **object ID** dari **az104-02-aaduser1** Akun pengguna Azure Active Directory yang Anda salin sebelumnya dalam tugas ini):

   ```powershell
   
    $scope = (Get-AzRoleDefinition -Name 'Support Request Contributor (Custom)').AssignableScopes | Where-Object {$_ -like '*managementgroup*'}
    
    Remove-AzRoleAssignment -ObjectId '[object_ID]' -RoleDefinitionName 'Support Request Contributor (Custom)' -Scope $scope
   ```

1. Dari panel Cloud Shell, jalankan perintah berikut untuk menghapus definisi peran kustom:

   ```powershell
   Remove-AzRoleDefinition -Name 'Support Request Contributor (Custom)' -Force
   ```

1. Di portal Azure, navigasikan kembali ke panel **Pengguna - Semua pengguna** dari **Azure Active Directory**, dan hapus akun pengguna **az104-02-aaduser1**.

1. Di portal Azure, navigasikan kembali ke panel **Grup manajemen**. 

1. Pada panel **Grup manajemen**, pilih ikon **elipsis** di samping langganan Anda di bawah grup pengelolaan **az104-02-mg1** dan pilih **Pindahkan** untuk memindahkan langganan ke **grup pengelolaan Penyewa Root**.

   >**Catatan**: Kemungkinan grup manajemen target adalah **grup manajemen Penyewa Root**, kecuali Anda membuat hierarki grup manajemen khusus sebelum menjalankan lab ini.
   
1. Pilih **Refresh** untuk memverifikasi bahwa langganan telah berhasil dipindahkan ke **grup pengelolaan Penyewa Root**.

1. Navigasikan kembali ke panel **Grup manajemen**, klik ikon **elipsis** di sebelah kanan grup manajemen **az104-02-mg1** dan klik **Hapus**.
  >**Catatan**: Jika Anda tidak dapat menghapus **grup manajemen Akar Penyewa**, kemungkinan **Langganan Azure** berada di bawah grup manajemen. Anda perlu memindahkan **Langganan Azure** dari **grup manajemen Akar Penyewa** lalu menghapus grup.

## Tinjau

Di lab ini, Anda telah:

- Grup Manajemen yang Diimplementasikan
- Membuat peran RBAC kustom 
- Peran RBAC yang ditetapkan
