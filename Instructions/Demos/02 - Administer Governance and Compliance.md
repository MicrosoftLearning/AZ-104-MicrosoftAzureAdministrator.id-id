---
demo:
  title: 'Demonstrasi 02: Mengelola Tata Kelola dan Kepatuhan'
  module: Administer Governance and Compliance
---

# 02 - Mengelola Tata Kelola dan Kepatuhan

## Mengonfigurasi Langganan

Area ini tidak memiliki demonstrasi formal. 

**Referensi**: [Membuat langganan Azure tambahan](https://docs.microsoft.com/azure/cost-management-billing/manage/create-subscription)

## Mengonfigurasi kebijakan Azure

Dalam demonstrasi ini, kita akan bekerja dengan kebijakan Azure.

**Referensi**: [Tutorial: Membangun kebijakan untuk menegakkan kepatuhan - Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Menetapkan kebijakan**

1.  Akses portal Microsoft Azure.

2.  Cari dan pilih **Kebijakan**.

3.  Pilih **Penugasan** lalu **Tetapkan Kebijakan**.

5.  Diskusikan **Cakupan** yang menentukan sumber daya atau pengelompokan sumber daya tempat penetapan kebijakan diberlakukan.

6.   **Pilih elipsis Definisi kebijakan** untuk membuka daftar definisi yang tersedia. Luangkan waktu untuk meninjau definisi kebijakan bawaan.

7.  Cari dan pilih **kebijakan Lokasi yang** diizinkan. Kebijakan ini memungkinkan Anda membatasi lokasi yang dapat ditentukan oleh organisasi Anda saat menggunakan sumber daya.

8.  Pindahkan tab **Parameter** dan gunakan menu drop-down pilih satu atau beberapa lokasi yang diizinkan.

9.  Klik **Tinjau + buat** lalu **Buat** untuk membuat kebijakan.

**Membuat dan menetapkan definisi inisiatif**

1.  Kembali ke halaman Azure Policy dan pilih **Definisi** di bawah Penulisan.

2.  Pilih **Definisi** Inisiatif di bagian atas halaman.

3.  Berikan **Nama** dan **Deskripsi**.

4.  **Buat Kategori baru** .

5.  Dari panel **kanan Tambahkan **kebijakan Lokasi yang **** diizinkan.

6.  Tambahkan satu kebijakan tambahan yang Anda pilih.

7.  **Simpan** perubahan Anda lalu **Tetapkan** definisi inisiatif Anda ke langganan Anda.

**Periksa kepatuhan**

1.  Kembali ke halaman layanan Azure Policy.

2.  Pilih **Kepatuhan**.

3.  Tinjau status kebijakan dan definisi Anda.

**Periksa tugas remediasi**

1.  Kembali ke halaman layanan Azure Policy.

2.  Pilih **Remediasi**.

3.  Tinjau tugas remediasi apa pun yang tercantum.

4. Saat Anda memiliki waktu, hapus kebijakan dan inisiatif. 

## Mengonfigurasi Kontrol Akses Berbasis Peran

Dalam demonstrasi ini, kita akan belajar tentang tugas peran.

**Referensi**: [Tutorial: Memberikan akses pengguna ke sumber daya Azure menggunakan portal Azure - Azure RBAC](https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal)

**Referensi**: [Mulai Cepat - Memeriksa akses untuk pengguna ke sumber daya Azure - Azure RBAC](https://docs.microsoft.com/azure/role-based-access-control/check-access)

**Temukan lokasi bilah Kontrol Akses**

1.  Akses portal Microsoft Azure dan pilih grup sumber daya. Catat grup sumber daya apa yang Anda gunakan.

2.  Pilih bilah **Kontrol Akses (IAM)**  .

3.  Bilah ini akan tersedia untuk banyak sumber daya yang berbeda sehingga Anda dapat mengontrol izin.

**Meninjau izin peran**

1.  Pilih tab **Peran** (atas).

1.  Tinjau peran bawaan dalam jumlah besar yang tersedia.

1.  Klik dua kali peran, lalu pilih **Izin** (atas).

1.  Lanjutkan menelusuri peran hingga Anda dapat melihat tindakan Baca, Tulis, dan Hapus ** untuk peran tersebut**.

1.  Kembali ke bilah **Access Control (IAM).** 

**Tambahkan penetapan peran**

1.  Buat pengguna atau pilih pengguna yang sudah ada.

1.  Pilih **Tambahkan penetapan** peran dan pilih peran. Misalnya, *pemilik*.

1.  Pilih **Periksa akses**.

1.  Tinjau izin pengguna.

1.  Perhatikan bahwa Anda dapat **Menolak penugasan**.
