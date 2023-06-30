---
demo:
  title: 'Demonstrasi 10: Mengelola Perlindungan Data'
  module: Administer Data Protection
---

# 10 - Mengelola Perlindungan Data

## Mencadangkan Berbagi File Azure

Dalam demonstrasi ini, kita akan mengeksplorasi pencadangan berbagi file di portal Azure.

> **Catatan:** Demonstrasi ini memerlukan akun penyimpanan dengan berbagi file. 

**Referensi**: [Mencadangkan berbagi file Azure di portal Azure](https://docs.microsoft.com/azure/backup/backup-afs)

**Membuat vault Layanan Pemulihan**

1. Menggunakan portal Microsoft Azure.

1. Cari **vault Layanan Pemulihan** tertentu.

1. Buat **Vault Layanan Pemulihan**. Tinjau persyaratan bahwa vault berada di wilayah yang sama dengan berbagi file. 

1. Tunggu hingga vault dibuat. 

**Mengonfigurasi cadangan berbagi file**

1. Buka **Pusat pencadangan** dan buat instans **Backup** baru.

1. Tinjau dan diskusikan pilihan dalam drop-down **Jenis sumber data** . Pilih **File Azure (penyimpanan Azure)**. 

1. Pilih **vault** Anda.

1. **Lanjutkan** mengonfigurasi cadangan. Pilih akun penyimpanan dan berbagi file tertentu yang ingin Anda cadangkan.  

1. Di **detail Kebijakan** , klik **Edit kebijakan ini**. Diskusikan tujuan kebijakan pencadangan. Tinjau **jadwal pencadangan** dan **rentang retensi**.  

1. **Aktifkan pencadangan** untuk menyimpan perubahan Anda. 

1. Saat Anda memiliki waktu, tinjau cara **Memulihkan** **instans Cadangan**. Selain itu, cara memantau **pekerjaan Pencadangan** Anda. 

## Mencadangkan Azure Virtual Machines

Dalam demonstrasi ini, kami akan menjadwalkan pencadangan harian komputer virtual ke vault Layanan Pemulihan.

> **Catatan:** Demonstrasi ini membutuhkan komputer virtual dan vault layanan pemulihan.

**Referensi**: [Tutorial - Mencadangkan beberapa komputer virtual Azure](https://docs.microsoft.com/azure/backup/tutorial-backup-vm-at-scale)

1. Menggunakan portal Microsoft Azure.

1. Buka **Pusat pencadangan** dan buat instans **Backup** baru.

1. Pilih **Azure Virtual machines** sebagai **jenis Datasource** dan pilih vault.

1. Tinjau **DefaultPolicy**. Kebijakan default mencadangkan komputer virtual sekali sehari. Cadangan harian dipertahankan selama 30 hari. Rekam jepret pemulihan instan dipertahankan selama dua hari.

1. Gunakan **Aktifkan pencadangan** untuk menyimpan konfigurasi Anda.

1. Saat Anda punya waktu, tinjau cara **Mencadangkan sekarang**. Selain itu, cara meninjau **pekerjaan Pencadangan** Anda.  

