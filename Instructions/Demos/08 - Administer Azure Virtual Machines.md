---
demo:
  title: 'Demonstrasi 08: Mengelola Azure Virtual Machines'
  module: Administer Azure Virtual Machines
---


# 08 - Mengelola Azure Virtual Machines

## Demonstrasi -- Membuat Komputer Virtual di portal

Dalam demonstrasi ini, kita akan membuat dan mengakses komputer virtual Azure di portal.

**Referensi**

[Mulai Cepat - Membuat VM Windows di portal Azure](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal)

[Mulai Cepat - Membuat VM Linux di portal Azure](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal)

[Koneksi ke komputer virtual dengan Bastion](https://learn.microsoft.com/azure/bastion/tutorial-create-host-portal#connect)

**Membuat komputer virtual**

**Catatan: **Langkah-langkah ini hanya mencakup beberapa parameter mesin virtual. Jangan ragu untuk menjelajahi dan mencakup area lain. Anda dapat membuat komputer virtual Windows atau Linux, tergantung audiens Anda.

1. Gunakan portal Azure.

1. Cari **Komputer virtual**. 

1. Buat komputer virtual dasar. Tinjau opsi ketersediaan, gambar, dan aturan masuk.

1. Diskusikan pentingnya membuat akun administrator yang aman.

1. Buat komputer virtual dan tunggu hingga sumber daya disebarkan.  

**Koneksi ke komputer virtual**

1. Ada beberapa cara untuk **Koneksi** ke komputer virtual. 

1. Untuk server Windows, Anda dapat menggunakan **RDP**, seperti yang diperlihatkan dalam Mulai Cepat. 

1. Untuk server Linux, Anda dapat **SSH**, seperti yang ditunjukkan di Mulai Cepat. 

1. Untuk salah satu server, Anda dapat tersambung dengan **layanan Bastion** (Mulai Cepat). Tinjau mengapa Bastion lebih disukai untuk RDP atau SSH. 

## Mengonfigurasi Ketersediaan Mesin Virtual

Dalam demonstrasi ini, kita akan menjelajahi opsi penskalaan mesin virtual.

**Referensi**

[Membuat komputer virtual dalam set skala menggunakan portal Azure](https://learn.microsoft.com/azure/virtual-machine-scale-sets/flexible-virtual-machine-scale-sets-portal)

1. Gunakan Portal Microsoft Azure.

1. Cari dan pilih **Virtual Machine Scale Sets**. 

1. Buat **Set** Skala Komputer Virtual. Tinjau tujuan set skala komputer virtual. Tinjau perbedaan antara **mode orkestrasi Uniform** dan **Flexible** . Jelaskan pilihan Anda dapat memengaruhi opsi penskalakan Anda. 

1. Pindah ke **tab Penskalakan** . 

1. Tinjau bagaimana **kebijakan** Skala** manual dan **Penyempurnaan skala digunakan. 

1. Ubah ke **kebijakan Penskalakan kustom** . 

1. Tinjau bagaimana **ambang CPU (%)** digunakan untuk meluaskan skala dan menskalakan dalam instans komputer virtual. 

