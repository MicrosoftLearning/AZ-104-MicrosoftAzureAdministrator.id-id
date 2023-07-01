---
demo:
  title: 'Demonstrasi 05: Mengelola Konektivitas Antarsitus'
  module: Administer Intersite Connectivity
---

# 05 - Mengelola Konektivitas Antarsitus

## Mengonfigurasi Peering VNet

**Catatan:** Untuk demonstrasi ini, Anda memerlukan dua jaringan virtual.

**Referensi**: [Menyambungkan jaringan virtual dengan peering VNet - tutorial](https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal)

**Konfigurasi peering VNet di jaringan virtual pertama**

1. Di  **portal Azure**, pilih jaringan virtual pertama. Tinjau nilai peering. 

1. Di bawah **Pengaturan**, pilih **Peering** dan **+ Tambahkan** peering baru.

1. Konfigurasikan peering jaringan virtual kedua. Gunakan ikon informasi untuk meninjau pengaturan yang berbeda. 

1. Saat peering selesai, tinjau **status Peering**. 

**Konfirmasi peering VNet di jaringan virtual kedua**

1. Di  **portal Azure**, pilih jaringan virtual kedua

1. Di bawah **Pengaturan**, pilih **Peering**.

1. Perhatikan bahwa peering telah dibuat secara otomatis.  Perhatikan bahwa **** Status   **Peering**Tersambung.

1. Di lab, siswa akan membuat peering dan menguji koneksi antara komputer virtual. 



