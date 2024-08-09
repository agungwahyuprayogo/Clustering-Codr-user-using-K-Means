## Identifikasi Masalah

1.	Belum adanya clustering pengguna yang menggunakan aplikasi Codr.
2.	Tidak semua pengguna aplikasi Codr menggunakan aplikasi tersebut dari awal (membuka aplikasi dan Sign Up) sampai tuntas atau menyelesaikan tugas setelah materi yang diberikan di aplikasi (Finish Horray.1).

## Rumusan Masalah

Bagaimana implementasi dari metode K-Means dalam clustering pengguna untuk rekomendasi pengembangan fitur pada aplikasi Codr?

## Tujuan Penelitian

1.	Clustering pengguna berdasarkan pola prilaku pengguna dalam menggunakan aplikasi Codr.
2.	Memberikan rekomendasi kepada pengembangan fitur aplikasi Codr berdasarkan cluster pengguna yang ditemukan.

## Manfaat Penelitian

1.	Pengembang aplikasi bisa mengembangkan fitur – fitur berdasarkan clustering pengguna yang ditemukan
2.	Meningkatkan tingkat kepuasan dan keterlibatan pengguna dengan menyediakan fitur-fitur yang lebih relevan dan menarik.

## Batasan Masalah

1.	Penelitian hanya akan fokus terhadap data yang diperoleh dari aplikasi Codr pada platform iOS.
2.	Data yang digunakan dalam penelitian ini hanya terbatas pada data First Time Download, Installations, dan User Flow.
3.	Penelitian hanya akan menggunakan metode K-Means untuk clustering data pengguna yang diperoleh.

# Business Understanding

memahami pola perilaku pengguna dalam menggunakan aplikasi dan Mengidentifikasi bagian mana saja dari aplikasi yang dapat ditingkatkan berdasarkan hasil dari clustering.

# Data Understanding

Data user flow mencakup ID pengguna dan kolom First App Open sampai Finish Horray1. Setiap kolom memiliki nilai 1 dan 0, yang mewakili penggunaan fitur pada aplikasi Codr. Jika nilai pada kolom adalah 1, itu menunjukkan bahwa pengguna telah menggunakan fitur tersebut dan melanjutkan ke fitur berikutnya. Sebaliknya, jika nilai pada kolom adalah 0, pengguna tidak menggunakan fitur tersebut dan tidak melanjutkan ke fitur berikutnya.

## Source Data

- Data didapat dari pengembang aplikasi Codr yang menggunakan tools Mixpanel.
- Tanggal pada data UserFlow.csv dimulai dari 26 April hingga 27 November
- Data berisi 74 baris dan 15 kolom berukuran 8.8 kb (kilo byte)

## Data Dictionary

- UserId : ID pengguna
- First App Open : Apakah pengguna membuka aplikasi? (1:Ya, 0:Tidak)
- Read Onboarding : Apakah pengguna membaca orientasi setelah membuka Aplikasi? (1:Ya, 0:Tidak)
- Sign Up : Apakah pengguna melakukan Sign Up? (1:Ya, 0:Tidak)
- Load Topic : Apakah pengguna memuat topik? (1:Ya, 0:Tidak)
- Topic Choosed : Apakah pengguna memilih topik? (1:Ya, 0:Tidak)
- Load Learning Page : Apakah pengguna Memuat Halaman Pembelajaran? (1:Ya, 0:Tidak)
- Sub Topic Choosed : Apakah pengguna memilih sub topik? (1:Ya, 0:Tidak)
- Read Material : Apakah pengguna membaca materi? (1:Ya, 0:Tidak)
- Correct Answer Quiz : Apakah pengguna menjawab kuis dengan benar? (1:Ya, 0:Tidak)
- Finish Hooray: Dipengaruhi kolom Kuis Jawaban Benar, jika pengguna menjawab benar maka 1, jika salah maka 0
- Exercise Choosed : Apakah pengguna melakukan latihan? (1:Ya, 0:Tidak)
- Exercise Choosed.1 : Apakah pengguna memilih latihan lain? (1:Ya, 0:Tidak)
- Correct Answer Exercise : Apakah pengguna benar dalam mengerjakan soal? (1:Ya, 0:Tidak)
- Finish Hooray.1 : Dipengaruhi oleh kolom Latihan Jawaban Benar, jika pengguna benar dalam menjawab maka 1, jika salah maka 0

# **Data Preparation**

* **Dataset**
    * UserFlow.csv
* **Code use** :
    * Python 3.9.13
* **Package** :
    * Pandas, Numpy, Matplotlib, KModes and Warning
 
# Data Profiling

![18 data profiling](https://github.com/user-attachments/assets/36da5564-6c46-49cf-9b7a-8c0fdc817552)

menampilkan info dataset

![19 info data](https://github.com/user-attachments/assets/8a3fb8f2-e330-48f2-806e-cf958c5e7a3f)

Dari sini terlihat, ukuran dari dataset adalah sekitar 8.8KB. memiliki 74 baris dan 15 kolom dan tidak ada missing value

menampilkan jumlah baris dan kolom

![20 data shape](https://github.com/user-attachments/assets/7bbc8323-6e2d-4913-b25d-3489ce39b7a7)

# EDA

Exploratory Data Analysis (EDA) adalah proses kritis dalam melakukan penyelidikan awal terhadap data dengan tujuan menemukan pola, anomali, pengujian hipotesis dan mampu memeriksa asumsi dengan bantuan ringkasan statistik kemudian representasi grafis (visualisasi)

Kita cek berapa min, max, rata - rata, dll yang bisa didapat dari dataset

![describe](https://github.com/user-attachments/assets/6d6ec395-8332-4289-87e0-3272d03813d6)

Dari sini kita bisa melihat, semua data dalam bentuk kategori (1 untuk melanjutkan dan 0 untuk berhenti). **Sekitar 25% pengguna berhenti saat mencapai Daftar** dan tidak melanjutkan ke alur aplikasi alur berikutnya. Lalu ada **beberapa pengguna yang berhenti di bagian Baca Materi** dan tidak melanjutkan menjawab kuis. Ada **kemungkinan Pengguna tidak mengerti bahasa Inggris** dan tidak dapat menjawab pertanyaan kuis.

Mari kita lihat berapa banyak pengguna aplikasi yang membuka aplikasi dari awal (First App Open) hingga Selesai (Finish Horray.1)

![2 berapa pengguna](https://github.com/user-attachments/assets/4212d8a3-1ecd-482b-89b9-0ce014182215)

Dari sini kita dapat melihat, bahwa semakin maju aliran aliran Aplikasi, semakin rendah pelanggan untuk melanjutkan. Dari Read Onboarding ke Sign up dan dari Sign Up ke Load Topic mengalami penurunan yang signifikan

menampilkan penurunan aktivitas pengguna dalam menggunakan aplikasi dengan pie chart

![3 Pie chart First App Open](https://github.com/user-attachments/assets/1879d5a9-e008-453c-892e-eb1db2e312ad)

![4 Pie chart Read Onboarding](https://github.com/user-attachments/assets/f2a8dbb2-8025-471e-9ed7-c630a91a10ab)

![5 Pie chart sign up](https://github.com/user-attachments/assets/a47662fd-5da6-4dbe-8825-5d4a22921b9c)

![6 looad topic](https://github.com/user-attachments/assets/b32a38b1-7b7b-41f4-8fba-f21b97468083)

![7 topic choosed](https://github.com/user-attachments/assets/3d93b644-ee21-4ddb-9d3b-bccee4026f2d)

![8 load learning page](https://github.com/user-attachments/assets/39414f96-aa98-414d-ac30-1d6a15539fd6)

![9 sub topic choosed](https://github.com/user-attachments/assets/61a7697c-0371-49c5-badf-db058a39370b)

![10 read material](https://github.com/user-attachments/assets/c41ba3dd-7cd6-4f4f-b811-5fe079fb7146)

![11 correct answer quiz](https://github.com/user-attachments/assets/00a9719b-24cb-4d97-8e4e-050ad41e0a3b)

![12 finish horray](https://github.com/user-attachments/assets/3bbed1b8-aa2c-41b2-89d4-0e7aa61d6711)

![13 exercise choosed](https://github.com/user-attachments/assets/a1f49df7-69f9-48fa-99dc-4465069bd886)

![14 exercise choosed 1](https://github.com/user-attachments/assets/1364a6a4-e625-4f5e-a096-275eacf20ca0)

![15 correct answer exercise](https://github.com/user-attachments/assets/9fca9d3f-1f88-4e39-9b5e-da0fa764cc72)

![16 finish horray 1](https://github.com/user-attachments/assets/9abe9489-4be9-4713-82d4-142f3dcc5fc5)

## Elbow Method

![21 elbow method](https://github.com/user-attachments/assets/cbbbeccf-75ed-4fb0-9355-c79eb9591fe0)

berdasarkan visualiasi diatas, k terbaik antara 2 dan 3, namun mengingat kolom dataset berjumlah 14. saya memilih 5 karena k = 5 terdapat siku sekian derajat.

## Clustering

![22 hasil cluster](https://github.com/user-attachments/assets/5ff9f3c8-3c72-4c25-912d-ff84f4b52df2)

Dari grafik diketahui bahwa **jumlah user terbanyak masuk cluster 2** yaitu **sebanyak 31 user**, kemudian **cluster ke-3 dengan jumlah 23**, **cluster 0 sebanyak sebanyak 13 pengguna** dan **cluster 1 sebanyak 7 pengguna**

Menunjukan cluster 0 - cluster 4, apa perbedaan antara cluster 1 dan yang lainnya**

## Cluster 0

![23 cluster 0](https://github.com/user-attachments/assets/7339405b-786c-4f06-8b46-fe422b7707a3)

pada cluster ini terlihat jika pengguna membuka aplikasi dari awal (First App Open) hingga Load Topic

disarankan untuk memberi achievment kepada pengguna dan memberikan poin dalam aplikasi untuk membuat pengguna lebih semangat dalam menggunakan aplikasi dan mau untuk melanjutkan materi dalam aplikasi tersebut

## Cluster 1

![24 cluster 1](https://github.com/user-attachments/assets/40892714-5461-4ab3-bd7f-e45344bca8b2)

pada cluster ini pengguna hanya membuka aplikasi dari First App Open hanya sampai Read Onboarding, disarankan pembuat aplikasi untuk membuat fitur bahasa Indonesia. memungkinkan untuk memberi materi pada orang yang tidak sepenuhnya paham bahasa inggris

## Cluster 2

![25 cluster 2](https://github.com/user-attachments/assets/4b69ec85-3cb4-44d1-a47a-60a4d1559132)

Dari semua cluster, cluster 2 adalah cluster yang dimana penggunanya membuka aplikasi hingga Finish. Disarankan agar menambahkan fitur material riview untuk ringkasan materi atau jika ada yang salah dalam pengerjaan soal, lalu pengguna disarankan agar dibuatkan opsi untuk membuat playgroundnya sendiri dan bisa explore kemampuan dia, dan yang terakhir ini opsional, pengguna yang sudah sampai tahap ini diharapkan untuk diberi certificate atau title untuk menunjukan kalau pengguna tersebut sudah menyelesaikan materi hingga kelar

## Cluster 3

![26 cluster 3](https://github.com/user-attachments/assets/f7a113f1-595a-41c2-b2d4-aea104079e91)

Pada cluster ini banyak pengguna yang berhenti di exercise, disarankan untuk menambahkan fitur 'hint' agar membantu pengguna yang kesulitan dalam menjawab soal

## Cluster 4

![27 cluster 4](https://github.com/user-attachments/assets/3b720a1a-b115-4c92-8061-9a2a46d7b15f)

Pada Cluster ini, pengguna berhenti di fitur Load Topic, saran pertama untuk menambahkan bahasa Indonesia dan Geust Mode, agar pengguna bisa melihat terlebih dahulu materi didalam aplikasi tersebut bagaimana, dan jika cocok baru pengguna bisa login.
