---
demo:
  title: 'Demonstrasi 08: Mengelola Azure Virtual Machines'
  module: Administer Azure Virtual Machines
---


# 08 - Mengelola Azure Virtual Machines

## Demonstrasi -- Buat Virtual Machines di portal

Dalam demonstrasi ini, kita akan membuat dan mengakses komputer virtual Azure di portal.

**Referensi**

[Mulai cepat - Membuat VM Windows di portal Microsoft Azure](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal)

[Mulai cepat - Membuat VM Linux di portal Microsoft Azure](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal)

[Menyambungkan ke komputer virtual dengan Bastion](https://learn.microsoft.com/azure/bastion/tutorial-create-host-portal#connect)

**Membuat komputer virtual**

**Catatan:** Langkah-langkah ini hanya mencakup beberapa parameter komputer virtual. Jangan ragu untuk menjelajahi dan mencakup area lain.  Anda dapat membuat komputer virtual Windows atau Linux, tergantung audiens Anda.

1. Menggunakan portal Microsoft Azure.

1. Cari **Komputer virtual**. 

1. Buat komputer virtual dasar. Tinjau opsi ketersediaan, gambar, dan aturan masuk.

1. Diskusikan pentingnya membuat akun administrator yang aman.

1. Buat komputer virtual dan tunggu hingga sumber daya disebarkan.  

**Menyambungkan ke komputer virtual**

1. Ada beberapa cara untuk **Menyambungkan** ke komputer virtual. 

1. Untuk server Windows, Anda dapat menggunakan **RDP**, seperti yang ditunjukkan dalam tutorial. Anda dapat melangkah lebih jauh dengan menginstal server web dan melihat halaman selamat datang IIS. 

1. Untuk server Linux, Anda dapat **menggunakan SSH**, seperti yang ditunjukkan dalam tutorial. Anda dapat melangkah lebih jauh dengan menginstal server web dan melihat server web dalam tindakan.

1. Untuk salah satu server, Anda dapat terhubung dengan layanan **Bastion** (tutorial). Tinjau mengapa Bastion lebih disukai daripada RDP atau SSH. 

## Mengonfigurasi Ketersediaan Virtual Machines

Dalam demonstrasi ini, kita akan menjelajahi opsi penskalaan mesin virtual.

**Referensi**

[Membuat mesin virtual dalam set skala menggunakan portal Microsoft Azure](https://learn.microsoft.com/azure/virtual-machine-scale-sets/flexible-virtual-machine-scale-sets-portal)

1. Gunakan Portal Microsoft Azure.

1. Cari dan pilih **Virtual Machine Scale Sets**. 

1. Buat **Virtual Machine Scale Sets**. Tinjau tujuan set skala komputer virtual. Tinjau perbedaan antara mode orkestrasi **Uniform** dan **Flexible** . Jelaskan pilihan Anda dapat memengaruhi opsi penskalakan Anda. 

1. Pindah ke tab **Penskalakan** . 

1. Tinjau cara kebijakan **Skala** manual dan **Penyempurnaan** skala digunakan. 

1. Ubah ke Kebijakan **penskalakan kustom** . 

1. Tinjau bagaimana **ambang CPU (%)** digunakan untuk meluaskan skala dan menskalakan dalam instans komputer virtual. 

