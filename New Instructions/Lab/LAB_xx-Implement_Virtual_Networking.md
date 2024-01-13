---
lab:
  title: 'Lab 04: Menerapkan Virtual Networking'
  module: Administer Virtual Networking
---

# Lab 04 - Menerapkan Virtual Network

## Persyaratan laboratorium

Lab ini memerlukan langganan Azure. Anda harus dapat membuat jaringan virtual dan subnet. 

## Perkiraan waktu: 20 menit

## Skenario laboratorium 

Organisasi global Anda berencana untuk menerapkan jaringan virtual. Jaringan ini berada di US Timur, Eropa Barat, dan Asia Tenggara. Tujuan langsungnya adalah untuk mengakomodasi semua sumber daya yang ada. Namun, organisasi berada dalam fase pertumbuhan dan ingin memastikan ada kapasitas tambahan untuk pertumbuhan tersebut.

Jaringan virtual **CoreServicesVnet** disebarkan di wilayah **US Timur**. Jaringan virtual ini memiliki jumlah sumber daya terbesar. Jaringan memiliki konektivitas ke jaringan lokal melalui koneksi VPN. Jaringan ini memiliki layanan web, database, dan sistem lain yang menjadi kunci operasi bisnis. Layanan bersama, seperti pengendali domain dan DNS terletak di sini. Sejumlah besar pertumbuhan diantisipasi, sehingga ruang alamat besar diperlukan untuk jaringan virtual ini.

Jaringan virtual **ManufacturingVnet** disebarkan di wilayah **Eropa Barat**, di dekat lokasi fasilitas manufaktur organisasi Anda. Jaringan virtual ini berisi sistem untuk pengoperasian fasilitas manufaktur. Organisasi ini mengantisipasi sejumlah besar perangkat internal yang terhubung agar sistem mereka mengambil data, seperti suhu, dan membutuhkan ruang alamat IP yang dapat diperluas.

Jaringan virtual **ResearchVnet** disebarkan di wilayah **Asia Tenggara**, di dekat lokasi tim penelitian dan pengembangan organisasi. Tim penelitian dan pengembangan menggunakan jaringan virtual ini. Tim ini memiliki seperangkat sumber daya kecil dan stabil yang diperkirakan tidak akan tumbuh. Tim membutuhkan sejumlah kecil alamat IP untuk beberapa komputer virtual untuk pekerjaan mereka.

## Simulasi lab interaktif

Simulasi **[](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Design%20and%20implement%20a%20virtual%20network%20in%20Azure)** lab interaktif tersedia untuk topik ini. Simulasi ini memungkinkan Anda mengklik skenario serupa dengan kecepatan Anda sendiri. Ada perbedaan antara simulasi interaktif dan lab yang dihosting ini, tetapi konsep inti dan ide yang ditunjukkan sama. Langganan Azure tidak diperlukan. 
## Tugas

+ Tugas 1: Membuat grup sumber daya.
+ Tugas 2: Buat jaringan virtual dan subnet CoreServicesVnet.
+ Tugas 3: Buat jaringan virtual dan subnet ManufacturingVnet.
+ Tugas 4: Buat jaringan virtual ResearchVnet dan subnet.
+ Tugas 5: Verifikasi pembuatan VNet dan Subnet.

## Diagram arsitektur
![Tata letak jaringan](../media/az104-lab04-diagram.png)

| **Virtual Network** | **Wilayah**   | **Ruang alamat jaringan virtual** | **Subnet**                | **Subnet**    |
| ------------------- | ------------ | --------------------------------- | ------------------------- | ------------- |
| CoreServicesVnet    | AS Timur      | 10.20.0.0/16                      |                           |               |
|                     |              |                                   | GatewaySubnet             | 10.20.0.0/27  |
|                     |              |                                   | SharedServicesSubnet      | 10.20.10.0/24 |
|                     |              |                                   | DatabaseSubnet            | 10.20.20.0/24 |
|                     |              |                                   | PublicWebServiceSubnet    | 10.20.30.0/24 |
| ManufacturingVnet   | Eropa Barat  | 10.30.0.0/16                      |                           |               |
|                     |              |                                   | ManufacturingSystemSubnet | 10.30.10.0/24 |
|                     |              |                                   | SensorSubnet1             | 10.30.20.0/24 |
|                     |              |                                   | SensorSubnet2             | 10.30.21.0/24 |
|                     |              |                                   | SensorSubnet3             | 10.30.22.0/24 |
| ResearchVnet        |Asia Tenggara| 10.40.0.0/16                      |                           |               |
|                     |              |                                   | ResearchSystemSubnet      | 10.40.0.0/24  |


Jaringan virtual dan subnet ini disusun dengan cara yang mengakomodasi sumber daya yang ada namun memungkinkan pertumbuhan yang diproyeksikan. Mari kita buat jaringan virtual dan subnet ini untuk meletakkan fondasi untuk infrastruktur jaringan kita.

>**Tahukah Anda?**: Ini adalah praktik yang baik untuk menghindari rentang alamat IP yang tumpang tindih untuk mengurangi masalah dan menyederhanakan pemecahan masalah. Tumpang tindih adalah masalah di seluruh jaringan, baik di cloud atau lokal. Banyak organisasi merancang skema alamat IP di seluruh perusahaan untuk menghindari tumpang tindih dan merencanakan pertumbuhan di masa mendatang.



## Tugas 1: Membuat grup sumber daya

### Buat grup sumber daya untuk semua sumber daya di lab ini. 

1. Masuk ke **portal Azure** - `http://portal.azure.com`.

1. Cari dan pilih **Grup** sumber daya, lalu pilih **+ Buat**.  

1. Buat grup sumber daya dengan pengaturan ini. 

| **Tab**         | **Opsi**                                 | **Nilai**            |
| --------------- | ------------------------------------------ | -------------------- |
| Dasar          | Grup sumber daya                             | `az104-rg4` |
|                 | Wilayah                                     | (US) **US Timur**     |
| Tag            | Perubahan tidak diperlukan                        |                      |
| Tinjau + buat | Tinjau pengaturan dan pilih **Buat** |                      |


1. Refresh halaman **Grup** sumber daya, dan verifikasi bahwa **az104-rg4** muncul dalam daftar.

## Tugas 2: Membuat jaringan virtual dan subnet CoreServicesVnet

Organisasi merencanakan sejumlah besar pertumbuhan untuk layanan inti. Dalam tugas ini, Anda membuat jaringan virtual dan subnet terkait untuk mengakomodasi sumber daya yang ada dan pertumbuhan yang direncanakan.

1. Cari dan pilih **Virtual Networks**.

    ![Beranda portal Azure, hasil bilah Pencarian Global untuk jaringan virtual.](../media/az104-lab04-vnet-search.png)

1. Pilih **Buat** pada halaman Jaringan virtual.  

    ![Buat wizard jaringan virtual.](../media/az104-lab04-createvnet.png)

3. Gunakan informasi dalam tabel berikut untuk membuat jaringan virtual CoreServicesVnet.  

| **Tab**      | **Opsi**         | **Nilai**            |
| ------------ | ------------------ | -------------------- |
| Dasar       | Grup Sumber Daya     | **az104-rg1** |
|              | Nama               | `CoreServicesVnet`     |
|              | Wilayah             | (US) **US Timur**         |
| Alamat IP | Ruang alamat IPv4 | `10.20.0.0/16`         |

    >**Note:** Remove or overwrite the default IP Address space.
    
![Konfigurasi alamat IP untuk penyebaran jaringan virtual azure](../media/az104-lab04-address-space.png)

 4. Buat subnet CoreServicesVnet. Untuk mulai membuat setiap subnet, pilih **+ Tambahkan subnet**. Untuk menyelesaikan setiap subnet, pilih **Tambahkan**.

| **Subnet**             | **Opsi**           | **Nilai**              |
| ---------------------- | -------------------- | ---------------------- |
| GatewaySubnet          | Nama subnet          | `GatewaySubnet`          |
|                        | Rentang alamat subnet | `10.20.0.0/27`           |
| SharedServicesSubnet   | Nama subnet          | `SharedServicesSubnet`   |
|                        | Rentang alamat subnet | `10.20.10.0/24`          |
| DatabaseSubnet         | Nama subnet          | `DatabaseSubnet`         |
|                        | Rentang alamat subnet | `10.20.20.0/24  `        |
| PublicWebServiceSubnet | Nama subnet          | `PublicWebServiceSubnet` |
|                        | Rentang alamat subnet | `10.20.30.0/24`          |

 1. Untuk menyelesaikan pembuatan CoreServicesVnet dan subnet terkait, pilih **Tinjau + buat**.

 1. Verifikasi konfigurasi Anda lulus validasi, lalu pilih **Buat**.
 
## Tugas 3: Membuat jaringan virtual dan subnet ManufacturingVnet

Dalam tugas ini, Anda terus membuat jaringan virtual tambahan dan subnet terkait. Organisasi mengantisipasi pertumbuhan untuk kantor manufaktur sehingga subnet berukuran untuk pertumbuhan yang diharapkan.

    >**Note**: If you need help, use the detailed steps in Task 2. 

| **Tab**      | **Opsi**         | **Nilai**             |
| ------------ | ------------------ | --------------------- |
| Dasar       | Grup Sumber Daya     | **az104-rg1**  |
|              | Nama               | `ManufacturingVnet`     |
|              | Wilayah             | (Eropa) **Eropa Barat**  |
| Alamat IP | Ruang alamat IPv4 | `10.30.0.0/16`          |



| **Subnet**                | **Opsi**           | **Nilai**                 |
| ------------------------- | -------------------- | ------------------------- |
| ManufacturingSystemSubnet | Nama subnet          | `ManufacturingSystemSubnet` |
|                           | Rentang alamat subnet | `10.30.10.0/24`             |
| SensorSubnet1             | Nama subnet          | `SensorSubnet1`             |
|                           | Rentang alamat subnet | `10.30.20.0/24`             |
| SensorSubnet2             | Nama subnet          | `SensorSubnet2`             |
|                           | Rentang alamat subnet | `10.30.21.0/24`             |
| SensorSubnet3             | Nama subnet          | `SensorSubnet3`             |
|                           | Rentang alamat subnet | `10.30.22.0/24`             |
 

## Tugas 4: Membuat jaringan dan subnet virtual ResearchVnet

Dalam tugas ini, Anda membuat jaringan virtual akhir dan subnet terkait. Organisasi tidak merencanakan pertumbuhan dan memiliki kebutuhan terbatas untuk kantor penelitian dan pengembangan.

>**Catatan**: Jika Anda memerlukan bantuan, gunakan langkah-langkah terperinci di Tugas 2. 

| **Tab**      | **Opsi**         | **Nilai**            |
| ------------ | ------------------ | -------------------- |
| Dasar       | Grup Sumber Daya     | **az104-rg1** |
|              | Nama               | `ResearchVnet`         |
|              | Wilayah             | **Asia Tenggara**       |
| Alamat IP | Ruang alamat IPv4 | `10.40.0.0/16`         |

| **Subnet**           | **Opsi**           | **Nilai**            |
| -------------------- | -------------------- | -------------------- |
| ResearchSystemSubnet | Nama subnet          | `ResearchSystemSubnet` |
|                      | Rentang alamat subnet | `10.40.0.0/24 `        |
 

## Tugas 5: Memverifikasi pembuatan VNet dan Subnet

Dalam tugas ini, Anda memvalidasi bahwa Anda memiliki semua jaringan virtual dan subnet yang diperlukan untuk memenuhi persyaratan organisasi.

1. Pada beranda portal Azure, cari dan pilih **Semua sumber daya**.

2. Pastikan bahwa CoreServicesVnet, ManufacturingVnet, dan ResearchVnet tercantum.

3. Pilih **CoreServicesVnet**. 

4. Di CoreServicesVnet, pada **Pengaturan**, pilih **Subnet**.

5. Dalam CoreServicesVnet | Subnet, verifikasi bahwa subnet yang Anda buat tercantum, dan rentang alamat IP sudah benar.

   ![Daftar subnet di CoreServicesVnet.](../media/az104-lab04-subnets.png)

6. Ulangi langkah 3 - 5 untuk setiap VNet. Ingatlah untuk mengubah pilihan jaringan virtual untuk memastikan Anda memverifikasi semuanya.

## Tinjau

Selamat! Anda telah berhasil membuat grup sumber daya, tiga jaringan virtual, dan subnet terkait.
