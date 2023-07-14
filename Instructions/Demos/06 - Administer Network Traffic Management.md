---
demo:
  title: 'Demonstrasi 06: Mengelola Manajemen Lalu Lintas Jaringan'
  module: Administer Network Traffic Management
---


# 06 - Mengelola Manajemen Lalu Lintas Jaringan

## Konfigurasikan Azure Load Balancer

Dalam demonstrasi ini, kita akan mempelajari cara membuat load balancer publik. 

**Catatan:** Demonstrasi ini memerlukan jaringan virtual dengan setidaknya satu subnet. 

**Referensi**: [Mulai cepat: Membuat load balancer publik untuk menyeimbangkan beban VM menggunakan portal Azure](https://learn.microsoft.com/azure/load-balancer/quickstart-load-balancer-standard-public-portal)

**Tampilkan fitur bantuan portal yang saya pilih**

1. Akses portal Microsoft Azure.

1. Cari dan pilih **Penyeimbangan beban - bantu saya memilih**.

1. Gunakan wizard untuk menelusuri skenario yang berbeda.
   
**Membuat load balancer**

1. Lanjutkan di portal Azure.

1. Cari dan pilih **Load balancer**. **Buat** load balancer. 

1. Pada tab **Dasar** , diskusikan **SKU**, **Jenis**, dan **Tingkat**.

1. Pada tab **Konfigurasi IP Frontend** , diskusikan menggunakan alamat IP publik.

1. Pada tab **Kumpulan backend** , pilih jaringan virtual dengan rentang alamat IP.

1. Pada tab **Aturan masuk** , buat aturan penyeimbangan beban. Diskusikan parameter seperti **Protokol**, **Port**, **Pemeriksaan kesehatan**, dan **Persistensi sesi**. 


## Mengonfigurasi Azure Application Gateway

Dalam demonstrasi ini, kita akan mempelajari cara membuat Azure Application Gateway. 

**Catatan**: Untuk mempermudah, buat jaringan virtual dan subnet baru saat Anda melalui konfigurasi. 

**Referensi**: [Mulai cepat: Lalu lintas web langsung dengan Azure Application Gateway - portal Azure](https://learn.microsoft.com/azure/application-gateway/quick-create-portal)

**Membuat Azure Application Gateway**

1. Akses portal Microsoft Azure.

1. Cari dan pilih **Azure Application Gateway**.

1. **Buat** gateway baru.

1. Pada tab **Dasar** , diskusikan **jumlah Tingkatan**, **Autoscaling**, dan **Instans**.

1. Pada tab **Frontend,** diskusikan jenis alamat IP.

1. Pada tab **Backend,** diskusikan kapan harus menggunakan kumpulan backend kosong.

1. Pada tab **Konfigurasi** , diskusikan aturan perutean. Bandingkan dengan aturan load balancer.

1. Jelaskan bahwa setelah gateway dibuat, Anda kemudian akan menambahkan target backend dan pengujian. 
