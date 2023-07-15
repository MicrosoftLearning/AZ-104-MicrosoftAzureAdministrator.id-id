---
demo:
  title: 'Demonstrasi 04: Mengelola Virtual Networking'
  module: Administer Virtual Networking
---

# 04 - Mengelola Virtual Networking

## Mengonfigurasi Jaringan Virtual

Dalam demonstrasi ini, Anda akan membuat jaringan virtual.

**Referensi**: [Mulai Cepat: Membuat jaringan virtual - portal Azure](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## Membuat jaringan virtual di portal

1.  Masuk ke portal Azure dan cari **Virtual Networks**.

1.  Buat jaringan virtual, menjelaskan pengaturan dasar saat Anda pergi. Pastikan setidaknya satu subnet dibuat. 

1.  Verifikasi bahwa jaringan virtual Anda telah dibuat.

## Mengonfigurasi Kelompok Keamanan Jaringan

Dalam demonstrasi ini, Anda akan menjelajahi NSG dan titik akhir layanan.

**Referensi**: [Membatasi akses ke sumber daya PaaS - tutorial - portal Azure](https://docs.microsoft.com/azure/virtual-network/tutorial-restrict-network-access-to-resources)

**Buat grup keamanan jaringan**

1. Akses portal Microsoft Azure.

1. Cari dan pilih **Kelompok Keamanan Jaringan**.

1. Buat NSG yang menjelaskan pengaturan saat Anda pergi. 
 
1. Tunggu hingga NSG baru disebarkan.

**Menjelajahi aturan masuk dan keluar**

1. Pilih NSG baru Anda.

1. Diskusikan bagaimana NSG dapat dikaitkan dengan subnet atau antarmuka jaringan.

1. Diskusikan tujuan aturan masuk dan keluar.  

1. Tinjau aturan masuk dan keluar default. 

1. Buat aturan baru, menjelaskan pengaturan saat Anda pergi. Secara khusus membahas pemilihan layanan (seperti HTTPS) dan pengaturan prioritas. 

## Mengonfigurasi Azure DNS

Dalam demonstrasi ini, Anda akan menjelajahi Azure DNS.

**Referensi**: [Tutorial: Menghosting domain dan subdomain Anda - Azure DNS](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns)


**Membuat zona DNS**

1. Akses portal Microsoft Azure.

1. Cari layanan **zona** DNS.

1. Buat **zona DNS** dan jelaskan tujuan zona tersebut. Untuk nama, Anda dapat menggunakan contoso.internal.com.

1.  Tunggu hingga zona DNS dibuat. Anda mungkin perlu **Merefresh** halaman.

**Menambahkan rangkaian data DNS**

**Referensi**: [Tutorial: Membuat catatan alias untuk merujuk ke rekaman sumber daya zona](https://learn.microsoft.com/azure/dns/tutorial-alias-rr)

1. Setelah zona Anda dibuat, pilih **+Kumpulan Catatan**.

1. Gunakan drop-down **Jenis** untuk menampilkan berbagai jenis rekaman. Tinjau bagaimana berbagai jenis rekaman digunakan. Perhatikan bagaimana informasi rekaman berubah saat Anda memilih jenis rekaman yang berbeda.

1. Buat rekaman **A** sebagai contoh. 

