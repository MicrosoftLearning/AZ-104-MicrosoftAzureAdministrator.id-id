---
lab:
  title: 'Lab 09c: Menerapkan Azure Kubernetes Service'
  module: Administer PaaS Compute Options
---

# Lab 09c - Menerapkan Azure Kubernetes Service
# Panduan lab siswa

## Skenario laboratorium

Contoso memiliki sejumlah aplikasi multi-tingkat yang tidak cocok untuk dijalankan dengan menggunakan Azure Container Instances. Untuk menentukan apakah mereka dapat dijalankan sebagai beban kerja dalam kontainer, Anda ingin mengevaluasi menggunakan Kubernetes sebagai orkestra kontainer. Untuk lebih meminimalkan overhead manajemen, Anda ingin menguji Azure Kubernetes Service, termasuk pengalaman penyebaran yang disederhanakan dan kemampuan penskalaannya.

**Catatan:** Tersedia **[simulasi lab interaktif](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2015)** yang memungkinkan Anda mengklik lab ini sesuai keinginan Anda. Anda mungkin menemukan sedikit perbedaan antara simulasi interaktif dan lab yang dihosting, tetapi konsep dan ide utama yang ditunjukkan sama. 

## Tujuan

Di lab ini Anda akan:

+ Tugas 1: Daftarkan penyedia sumber daya Microsoft.Kubernetes dan Microsoft.KubernetesConfiguration.
+ Tugas 2: Menyebarkan kluster Azure Kubernetes Service
+ Tugas 3: Menyebarkan pod ke dalam kluster Azure Kubernetes Service
+ Tugas 4: Menskalakan beban kerja kontainer di kluster layanan Azure Kubernetes

## Perkiraan waktu: 40 menit

## Diagram arsitektur

![gambar](../media/lab09c.png)

### Petunjuk

## Latihan 1

## Tugas 1: Daftarkan penyedia sumber daya Microsoft.Kubernetes dan Microsoft.KubernetesConfiguration.

Dalam tugas ini, Anda akan mendaftarkan penyedia sumber daya yang diperlukan untuk menyebarkan kluster Azure Kubernetes Services.

1. Masuk ke [portal Azure](https://portal.azure.com).

1. Di portal Microsoft Azure, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Jika diminta untuk memilih **Bash** atau **PowerShell**, pilih **PowerShell**.

    >**Catatan**: Jika ini adalah pertama kalinya Anda memulai **Cloud Shell** dan Anda disajikan dengan **pesan Anda tidak memiliki penyimpanan yang dipasang** , pilih langganan yang Anda gunakan di lab ini, dan klik **Buat penyimpanan**.

1. Dari panel Cloud Shell, jalankan perintah berikut untuk mendaftarkan penyedia sumber daya Microsoft.Kubernetes dan Microsoft.KubernetesConfiguration.

   ```powershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.Kubernetes

   Register-AzResourceProvider -ProviderNamespace Microsoft.KubernetesConfiguration
   ```

1. Tutup panel Cloud Shell.

## Tugas 2: Menyebarkan kluster Azure Kubernetes Service

Dalam tugas ini, Anda akan menyebarkan kluster Azure Kubernetes Services menggunakan portal Microsoft Azure.

1. Di portal Microsoft Azure, cari lokasi **Layanan Kubernetes** lalu, pada bilah **Layanan Kubernetes**, klik **+ Buat**, lalu klik **+ Buat Kluster Kubernetes**.

1. Pada tab **Dasar-dasar** bilah **Buat kluster Kubernetes**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | ---- | ---- |
    | Langganan | nama langganan Azure yang Anda gunakan di lab ini |
    | Grup sumber daya | nama grup sumber daya baru **az104-09c-rg1** |
    | Konfigurasi preset kluster | **Dev/Test ($)** |
    | Nama kluster Arc Kube | **az104-9c-aks1** |
    | Wilayah | nama wilayah tempat Anda dapat menyediakan kluster Kubernetes |
    | Zona ketersediaan | **Tidak ada** (hapus centang semua kotak) |
    | Versi Kube | menerima default |
    | Ketersediaan server API | menerima default |
    | Ukuran simpul | menerima default |
    | Metode skala | **Manual** |
    | Jumlah simpul | **1** |

1. Klik **Berikutnya: Kumpulan Simpul >** dan, pada **tab Kumpulan** Simpul dari **bilah Buat kluster** Kubernetes, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | ---- | ---- |
    | Mengaktifkan simpul virtual | **Dinonaktifkan** (bawaan) |

1. Klik **Berikutnya: Akses >** dan, pada tab **Akses** pada bilah **Buat kluster Kubernetes**, biarkan pengaturan diatur ke nilai defaultnya:

    | Pengaturan | Nilai |
    | ---- | ---- |
    | Identitas sumber daya | **Identitas terkelola yang ditetapkan sistem** |
    | Metode autentikasi | **Akun lokal dengan RBAC Kubernetes** |

1. Klik **Berikutnya: Jaringan >** dan, pada **tab Jaringan** dari **bilah Buat kluster** Kubernetes, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | ---- | ---- |
    | Konfigurasi jaringan | **kubenet** |
    | Awalan nama DNS | **awalan DNS yang valid dan unik secara global** |

1. Klik **Berikutnya: Integrasi >**, pada tab **Integrasi** dari **bilah Buat kluster** Kubernetes, tentukan pengaturan berikut (biarkan orang lain dengan nilai defaultnya):

    | Pengaturan | Nilai |
    | ---- | ---- |
    | Pemantauan kontainer | **Nonaktifkan** |
    | Mengaktifkan aturan pemberitahuan yang direkomendasikan | **Hapus centang** |
    
1.  Klik **Tinjau + buat**, pastikan validasi lulus dan klik **Buat**.

    >**Catatan**: Dalam skenario produksi, Anda ingin mengaktifkan pemantauan. Monitoring dinonaktifkan dalam kasus ini karena tidak tercakup dalam lab.

    >**Catatan**: Tunggu hingga penyebaran selesai. Ini akan memakan waktu sekitar 10 menit.

## Tugas 3: Menyebarkan pod ke dalam kluster Azure Kubernetes Service

Dalam tugas ini, Anda akan menyebarkan pod ke dalam kluster Azure Kubernetes Service.

1. Pada bilah penyebaran, klik tautan **Buka sumber daya**.

1. Pada bilah layanan **az104-9c-aks1** Kubernetes, di bagian **Pengaturan**, klik **Kumpulan simpul**.

1. Pada bilah **az104-9c-aks1 - Kumpulan simpul**, verifikasi bahwa kluster terdiri dari kumpulan tunggal dengan satu simpul.

1. Di portal Microsoft Azure, buka **Azure Cloud Shell** dengan mengeklik ikon di kanan atas Portal Azure.

1. Alihkan **Azure Cloud Shell** ke **Bash** (latar belakang hitam).

1. Dari panel Cloud Shell, jalankan perintah berikut untuk mengambil kredensial guna mengakses kluster AKS:

    ```sh
    RESOURCE_GROUP='az104-09c-rg1'

    AKS_CLUSTER='az104-9c-aks1'

    az aks get-credentials --resource-group $RESOURCE_GROUP --name $AKS_CLUSTER
    ```

1. Dari panel **Cloud Shell**, jalankan perintah berikut untuk memverifikasi konektivitas ke kluster AKS:

    ```sh
    kubectl get nodes
    ```

1. Di panel **Cloud Shell**, tinjau output dan verifikasi bahwa satu simpul yang terdiri dari kluster saat ini melaporkan status **Siap**.

1. Dari panel **Cloud Shell**, jalankan perintah berikut untuk menerapkan gambar **nginx** dari Docker Hub:

    ```sh
    kubectl create deployment nginx-deployment --image=nginx
    ```

    > **Catatan**: Pastikan untuk menggunakan huruf kecil saat mengetik nama penyebaran (nginx-deployment)

1. Dari panel **Cloud Shell**, jalankan perintah berikut untuk memverifikasi bahwa pod Kubernetes telah dibuat:

    ```sh
    kubectl get pods
    ```

1. Dari panel **Cloud Shell**, jalankan perintah berikut untuk mengidentifikasi status penyebaran:

    ```sh
    kubectl get deployment
    ```

1. Dari panel **Cloud Shell**, jalankan perintah berikut untuk membuat pod tersedia dari Internet:

    ```sh
    kubectl expose deployment nginx-deployment --port=80 --type=LoadBalancer
    ```

1. Dari panel **Cloud Shell**, jalankan perintah berikut untuk mengidentifikasi apakah alamat IP publik telah disediakan:

    ```sh
    kubectl get service
    ```

1. Jalankan kembali perintah hingga nilai di kolom **EXTERNAL-IP** untuk entri **nginx-deployment** berubah dari **\<pending\>** menjadi alamat IP publik. Catat alamat IP publik di kolom **EXTERNAL-IP** untuk **nginx-deployment**.

1. Buka jendela browser dan arahkan ke alamat IP yang Anda peroleh di langkah sebelumnya. Verifikasi bahwa halaman browser menampilkan **Selamat datang di nginx!** pesan.

## Tugas 4: Menskalakan beban kerja kontainer di kluster layanan Azure Kubernetes

Dalam tugas ini, Anda akan menskalakan secara horizontal jumlah pod dan kemudian jumlah simpul kluster.

1. Dari panel **Cloud Shell**, dan jalankan perintah berikut untuk menskalakan penyebaran dengan menambah jumlah pod menjadi 2:

    ```sh
    kubectl scale --replicas=2 deployment/nginx-deployment
    ```

1. Dari panel **Cloud Shell**, jalankan perintah berikut untuk memverifikasi hasil penskalaan penyebaran:

    ```sh
    kubectl get pods
    ```

    > **Catatan**: Tinjau output perintah dan verifikasi bahwa jumlah pod meningkat menjadi 2.

1. Dari panel **Cloud Shell**, jalankan perintah berikut untuk menskalakan kluster dengan menambah jumlah simpul menjadi 2:

    ```sh
    RESOURCE_GROUP='az104-09c-rg1'

    AKS_CLUSTER='az104-9c-aks1'

    az aks scale --resource-group $RESOURCE_GROUP --name $AKS_CLUSTER --node-count 2
    ```

    > **Catatan**: Tunggu hingga provisi node tambahan selesai. Ini mungkin memakan waktu sekitar 3 menit. Jika gagal, jalankan kembali perintah `az aks scale`.

1. Dari panel **Cloud Shell**, jalankan perintah berikut untuk memverifikasi hasil penskalaan kluster:

    ```sh
    kubectl get nodes
    ```

    > **Catatan**: Tinjau output perintah dan verifikasi bahwa jumlah simpul meningkat menjadi 2.

1. Dari panel **Cloud Shell**, jalankan perintah berikut untuk menskalakan penyebaran:

    ```sh
    kubectl scale --replicas=10 deployment/nginx-deployment
    ```

1. Dari panel **Cloud Shell**, jalankan perintah berikut untuk memverifikasi hasil penskalaan penyebaran:

    ```sh
    kubectl get pods
    ```

    > **Catatan**: Tinjau output perintah dan verifikasi bahwa jumlah pod meningkat menjadi 10.

1. Dari panel **Cloud Shell**, jalankan perintah berikut untuk meninjau distribusi pod di seluruh simpul kluster:

    ```sh
    kubectl get pod -o=custom-columns=NODE:.spec.nodeName,POD:.metadata.name
    ```

    > **Catatan**: Tinjau output perintah dan verifikasi bahwa pod didistribusikan di kedua node.

1. Dari panel **Cloud Shell**, jalankan perintah berikut untuk menghapus penyebaran:

    ```sh
    kubectl delete deployment nginx-deployment
    ```

1. Tutup panel **Cloud Shell**.

## Membersihkan sumber daya

>**Catatan**: Ingatlah untuk menghapus sumber daya Azure yang baru dibuat yang tidak lagi Anda gunakan. Dengan menghapus sumber daya yang tidak digunakan, Anda tidak akan melihat biaya yang tak terduga.

>**Catatan**: Jangan khawatir jika sumber daya lab tidak dapat segera dihapus. Terkadang sumber daya memiliki ketergantungan dan membutuhkan waktu lama untuk dihapus. Ini adalah tugas Administrator yang umum untuk memantau penggunaan sumber daya, jadi tinjau sumber daya Anda secara berkala di Portal untuk melihat bagaimana pembersihannya. 

1. Di portal Microsoft Azure, buka sesi shell **Bash** dalam panel **Cloud Shell**.

1. Buat daftar semua grup sumber daya yang dibuat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```sh
   az group list --query "[?starts_with(name,'az104-09c')].name" --output tsv
   ```

1. Hapus semua grup sumber daya yang Anda buat di seluruh lab modul ini dengan menjalankan perintah berikut:

   ```sh
   az group list --query "[?starts_with(name,'az104-09c')].[name]" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
   ```

    >**Catatan**: Perintah dijalankan secara asinkron (seperti yang ditentukan oleh parameter --nowait), jadi sementara Anda akan dapat menjalankan perintah Azure CLI lain segera setelah itu dalam sesi Bash yang sama, itu akan memakan waktu beberapa menit sebelum grup sumber daya benar-benar dihapus.

## Tinjau

Di lab ini, Anda telah:

+ Menyebarkan kluster Azure Kubernetes Service
+ Menyebarkan pod ke dalam kluster Azure Kubernetes Service
+ Beban kerja dalam kontainer yang diskalakan di kluster layanan Azure Kubernetes
