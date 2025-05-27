# Laporan Proyek Machine Learning - Nama Anda

## Project Overview

**Latar Belakang** 

Sistem rekomendasi konten telah menjadi bagian terpenting dari platform streaming modern seperti Netflix, Disney+, dan Amazon Prime. Sistem ini bertujuan membantu pengguna menemukan film atau acara TV yang sesuai dengan preferensi mereka di tengah jumlah konten yang sangat besar. Dari sekian banyaknya film yang diproduksi membuat calon penonton kesulitan dalam menentukan film yang akan ditontonnya. Untuk mencari film tentunya akan memakan waktu, selain itu film yang sudah  ditentukan untuk ditonton belum tentu sesuai dengan keinginan calon penonton setelah menontonnya, sehingga akan  menghabiskan waktu lebih banyak lagi[1]. Hal ini yang menjadi alasan mengapa sulit bagi orang untuk mencari suatu judul film.Salah satu pendekatan dalam membangun sistem tersebut adalah content-based filtering, yang merekomendasikan item berdasarkan kemiripan konten dengan item yang dicari pengguna.

Metode Content-Based Filtering adalah salah satu  teknik  sistem  rekomendasi  yang  menggunakan fitur,  atribut,  atau  karakteristik  sebuah item untuk memberikan   rekomendasi   kepada   pengguna   dan berprinsip   memberikan   rekomendasi   berdasarkan kemiripan item[2]. Content-based filtering sendiri, bekerja dengan menganalisis fitur-fitur konten seperti genre, deskripsi, tipe konten untuk menentukan kemiripan antar film. Dalam proyek ini, digunakan dataset yang memuat judul film Netflix beserta atribut kontennya untuk membangun sistem rekomendasi yang memberikan saran tontonan tanpa mengandalkan interaksi pengguna seperti rating atau riwayat penonton.

**Kenapa Masalah Ini Penting**

Di era digital dengan lautan pilihan konten hiburan, pengguna seringkali mengalami kesulitan dalam memilih film yang sesuai dengan preferensi mereka. Tanpa sistem rekomendasi yang efektif, banyak konten berkualitas yang tidak pernah dijangkau oleh pengguna dan juga banyaknya konten membuat pengguna malas untuk mencari satu persatu. Oleh karena itu, pengembangan sistem rekomendasi berbasis konten menjadi sangat penting untuk Meningkatkan pengalaman pengguna dan mengurangi waktu pencarian konten yang sesuai.


**Referensi**
- Fajriansyah, Muhammad , et al. “Sistem Rekomendasi Film Menggunakan Content Based Filtering.” Jurnal Pengembangan Teknologi Informasi Dan Ilmu Komputer, vol. 5, no. 6, 6 May 2021, pp. 2188–2199, j-ptiik.ub.ac.id. Accessed 23 May 2025. 
- None Sadesty Rahmadhani, et al. "Sistem Rekomendasi Penelusuran Buku Berbasis Content-Based Filtering Dengan Pembobotan TF-RF." Jurnal Informatika Polinema, vol. 10, no. 4, 30 Aug. 2024, pp. 491–500, https://doi.org/10.33795/jip.v10i4.5565. Accessed 19 May 2025.

## Business Understanding

### Problem Statements

Dalam layanan streaming seperti Netflix, banyak pengguna kesulitan menemukan film atau acara TV yang sesuai dengan preferensi mereka tanpa harus menelusuri ratusan judul. Tanpa adanya riwayat tontonan atau rating dari pengguna, bagaimana sistem dapat memberikan rekomendasi konten yang relevan hanya berdasarkan informasi konten itu sendiri seperti genre, tipe, dan deskripsi dari film?

### Goals

Tujuan dari proyek ini adalah membangun sistem rekomendasi film dan acara TV berbasis content-based filtering. Sistem ini diharapkan mampu memberikan 10 rekomendasi konten yang relevan berdasarkan input berupa judul, dengan menggunakan fitur-fitur seperti genre, tipe konten, dan deskripsi. Sehingga, pengguna dapat menemukan konten yang sesuai dengan preferensi mereka secara lebih cepat dan efisien.


### Solution statements
Untuk mencapai tujuan tersebut, dilakukan pendekatan berbasis pemrosesan teks dan penghitungan kemiripan konten. Langkah-langkahnya antara lain:
- Menggabungkan fitur teks dari kolom type, listed_in, dan description menjadi satu representasi konten.
- Melakukan vektorisasi teks menggunakan TF-IDF Vectorizer untuk mengubah teks menjadi representasi numerik.
- Mengukur kemiripan antar konten menggunakan cosine similarity.
- Mengembangkan fungsi rekomendasi untuk mengembalikan 10 konten paling mirip berdasarkan judul yang diberikan pengguna.

## Data Understanding
Dataset Netflix Movies and TV Shows merupakan dataset yang berasal dari kaggle, dengan link berikut : https://www.kaggle.com/datasets/shivamb/netflix-shows. Dataset ini memiliki data sebanyak 8.807 baris data serta 12 kolom dan terdapat kolom yang memiliki missing value di bagian director, cast, dan country. 

Berikut penjelasan setiap fiturnya antara lain :  
1. release_year
    - Rentang penayangan/release film ini tidak menunjukkan adanya outlier, karena rentang masih wajar, dan ketika dilihat untuk baris datanya memang benar untuk release ya tahun 1925.

2. show_id
    - ID unik untuk setiap judul yang tercantum di Netflix. Digunakan sebagai identifikasi unik.

3. type
    - Menunjukkan tipe konten untuk menandakan apakah konten berupa film atau serial TV. Tipenya antara lain : Movie dan TV Show.

4. title
    - Judul tayangan yang ada di netflix. Setiap judul biasanya unik dan menjadi identitas utama sebuah tayangan. 

5. director
    - Nama sutradara dari setiap film, banyak sutradara yang menjembatani film yang sama, dan banyak pula yang kosong(missing_value). 

6. cast
    - Daftar aktor atau aktris yang terlibat dalam tayangan. Namun beberapa terdapat data yang kosong.

7. country
    - Negara tempat produksi konten dilakukan. Isinya bisa berisi lebih dari satu negara atau kosong.

8. date_added
    - Tanggal film ditambahkan ke platform Netflix.

9. rating
    - Kategori usia yang diperbolehkan menonton tayangan. Kategori usia (TV-MA, PG, R, dll) : 
        - TV-MA	atau Mature Audience –> khusus dewasa (usia 17+), berisi kekerasan, seks, dll.
        - PG atau Parental Guidance –> butuh bimbingan orang tua untuk anak-anak.
        - R	atau Restricted –> terbatas untuk usia 17+ kecuali didampingi orang tua.
        - TV-G	atau General Audience –> aman untuk semua usia.
        - NC-17	-> Tidak cocok untuk anak-anak di bawah usia 17, meski dengan pengawasan.

10. duration
    -  Lama tayangan film atau jumlah season. untuk movie dalam satuan menit (misal: "90 min") dan untuk TV Show dalam jumlah musim (misal: "1 Season")

11. listed_in
    - Genre atau kategori tayangan. berisi multi genre yang dipisahkan oleh koma, berisi Dramas, Comedies, Action & Adventure

12. description
    -  Ringkasan atau sinopsis singkat dari konten. 


### Eksplorasi Data (EDA) & Visualisasi
- Tipe data sebagian besar bertipe object/string.
- film yang ditambahkan kebanyakan adalah 2000 keatas.
- Type distribusinya lumayan merata dibanding dengan fitur lainnya dan didominasi oleh Movie dibandingkan TV Show.
- Terdapat missing value terutama di kolom seperti director, cast, country, date_added, rating, dan duration. Namun missing value ini tidak akan diproses lebih lanjut dikarenakan kolom yang digunakan hanya kolom ype, description, dan listed_in saja.
- Tidak terdapat data duplikat pada semua kolom.
- Terdapat outlier dikolom release_year sebanyak 719 outliers. Namun outlier ini tidak akan diproses lebih lanjut dikarenakan outlier didalam film menunjukkan bahwa nilai-nilai tersebut valid, karena film lama dari tahun 1970-an atau 1990-an memang ada dan bukan merupakan kesalahan data.

## Data Preparation
Dalam tahap ini, dilakukan beberapa proses untuk memastikan data siap digunakan dalam pembangunan sistem rekomendasi berbasis konten:
- Handling Missing Value: Dilakukan penghapusan data yang memiliki missing value pada kolom type, description, dan listed_in. Ketiga kolom ini merupakan fitur utama yang digunakan untuk membentuk sistem rekomendasi konten, sehingga keberadaan nilai kosong dapat mengganggu hasil rekomendasi. Hasil penghapusan pada kolom type, description, dan listed in adalah 0 baris data dikarenakan kolom-kolom tersebut tidak memiliki missing value. Namun terdapat missing value di kolom lain seperti kolom director, cast, country, date_added, rating, dan duration. Dikarenakan yang digunakan hanya kolom  type, description, dan listed_in maka dari itu saya tidak melakukan handling missing value pada kolom director, cast, country, date_added, rating, dan duration.
- Feature Engineering (Penggabungan Fitur): Kolom type, listed_in, dan description digabung menjadi satu kolom teks panjang menggunakan tanda pemisah spasi. Penggabungan ini bertujuan untuk menjadikan satu konten secara menyeluruh dalam satu vektor teks, yang kemudian akan digunakan sebagai input untuk teknik vektorisasi.
- Text Vectorization (TF-IDF Vectorizer): Hasil penggabungan fitur teks kemudian diproses menggunakan TF-IDF Vectorizer untuk mengubah teks menjadi representasi numerik berbasis frekuensi dan pentingnya istilah dalam dokumen. Hasil vektorisasi ini digunakan untuk mengukur kemiripan antar konten. 

## Modeling
Dalam tahap pemodelan ini, digunakan pendekatan content-based filtering yang fokus pada kemiripan antar konten berdasarkan informasi deskriptif. Tahapan modeling dilakukan sebagai berikut:
- Similarity Computation (Cosine Similarity): Setelah teks diubah menjadi vektor, dilakukan perhitungan kemiripan antar konten menggunakan metode cosine similarity. Kemiripan ini digunakan untuk mengidentifikasi dan mengurutkan rekomendasi konten yang paling relevan terhadap input judul yang diberikan oleh pengguna. hasil mendekati 1 atau 1 merupakan hasil yang sangat mirip dengan inputannya, sedangkan mendekati 0 atau 0 merupakan hasil yang berbeda dengan inputannya
- Rekomendasi Konten : Ketika pengguna memberikan input berupa judul film atau acara TV, sistem akan mencocokkan judul tersebut dengan data yang ada, kemudian menghitung cosine similarity antara konten input dan seluruh konten lainnya. Selanjutnya, sistem akan mengembalikan 10 rekomendasi konten teratas dengan nilai kemiripan tertinggi, yang dianggap paling relevan dengan konten input.

## Evaluation
### Metrik Evaluasi
Karena sistem ini tidak menggunakan data interaksi pengguna (seperti klik atau rating), maka dilakukan evaluasi manual berdasarkan konten yang relevan. Pendekatan ini dilakukan dengan menilai apakah hasil rekomendasi relevan dengan input berdasarkan genre(listed_in), tipe(type), dan deskripsinya(description).

### Evaluasi Manual
Evaluasi manual pada sistem rekomendasi dilakukan dengan menguji fungsi get_recommendations() menggunakan input judul "Narcos". Fungsi ini bekerja dengan mengecek keberadaan judul pada dataset, kemudian menghitung cosine similarity antara konten "Narcos" dengan seluruh konten lain. Setelah itu, sistem mengurutkan konten berdasarkan skor kemiripan tertinggi dan mengembalikan 10 konten teratas yang dianggap paling mirip. Pada sistem ini menampilkan data dengan kolom title, listed_in, type, description, dan similiarity dari konten narcos tersebut.
- pengujian pada film narcos menghasilkan : 
    - Genre dan tema dari rekomendasi ("El Chapo", "Narcos: Mexico", "Bad Blood", "Cocaine Cowboys: The Kings of Miami", "Ganglands", dll)  memiliki kemiripan yang kuat dengan "Narcos", baik dari segi genre (Crime TV Shows, TV Action & Adventure), type (TV Show), maupun dari deskripsi.
    ![Image](https://github.com/user-attachments/assets/2e92043c-ae12-4a1e-af8f-b900a365b371)
    - Hal ini menunjukkan bahwa sistem rekomendasi dapat menangkap konteks konten dengan baik.

### Evaluasi Kuantitatif: Precision@10 dan Recall@10
Untuk memberikan penilaian yang lebih kuat terhadap performa sistem rekomendasi, dilakukan evaluasi menggunakan metrik kuantitatif seperti Precision@10 dan Recall@10. Penilaian dilakukan dengan memilih judul film dilanjutkan dengan mengambil 10 rekomendasi film teratas untuk masing-masing judul menggunakan sistem get_recommendation, lalu dihitung berapa rekomendasi film yang sama menurut genre, deskripsi, dan tipe tayangan. 
![Image](https://github.com/user-attachments/assets/38163465-c7d1-4033-a49a-e5c61a31097a)
Gambar diatas merupakan hasil evaluasi menggunakan Precision@10 dan Recall@10. Hasil evaluasi ini menunjukkan performa sistem dalam menilai kecocokan antara judul dan genre yang dihasilkan, dengan menggunakan metrik Precision@10 dan Recall@10. 
- Precision@10 bernilai 1.0 untuk semua judul, artinya dari 10 kata kunci teratas yang diprediksi, semua benar relevan atau tepat sesuai judul tersebut. 
- Nilai Recall@10 berbeda-beda: 
    - untuk "Narcos" dan "The Crown" sebesar 0.67, yang berarti sistem menangkap sekitar 67% dari seluruh kata kunci relevan yang seharusnya ditemukan
    - untuk "Jaguar" hanya 0.33, menunjukkan bahwa hanya 33% dari kata kunci relevan yang terdeteksi. 
- Maka dari itu sistem ini tepat dalam prediksi film teratas, tapi masih ada beberapa yang belum tercover. 

### Hubungan dengan Business Understanding
Sistem yang dibangun ini dapat menjawab beberapa pertanyaan terkait dengan business understanding antara lain : 
- Apakah sudah menjawab setiap problem statment?
    - Ya, sistem rekomendasi yang dibangun telah menjawab masalah utama yang diangkat. Dengan menggunakan pendekatan content-based filtering, sistem mampu memberikan rekomendasi berdasarkan kesamaan deskripsi konten, genre, dan tipe tanpa memerlukan data historis pengguna seperti riwayat tontonan atau rating dari pengguna. Sistem mengambil dataset untuk list film di netflix dan menggunakan cosine similiarity untuk menghitung kedekatan konten satu dengan yang serupa tanpa perlu melihat riwayat tontonan. 
- Apakah berhasil mencapai setiap goals yang diharapkan?
    - Ya, tujuan dari proyek ini tercapai antara lain Sistem berhasil memberikan rekomendasi konten berbasis kemiripan konten berdasarkan genre, deskripsi, tipe. Selain itu, sistem juga dapat memberikan 10 rekomendasi film yang relevan dengan inputan film sebelumnya sehingga konten serupa secara cepat, efisien, dan tanpa data pengguna sebelumnya.
- Apakah setiap solution statement yang kamu rencanakan berdampak? Jelaskan!
    - Ya. Setiap solusi yang telah direncanakan memiliki dampak terhadap keberhasilan sistem. Berawal dari penggabungan fitur teks (type, listed_in, description) dilanjutkan dengan TF-IDF Vectorization lalu perhitungan Cosine Similarity dan yang terakhir membuat fungsi rekomendasi. Semua solusi tersebut berkontribusi dalam membangun sistem yang relevan dengan masalah dan tujuannya dan dengan sistem ini, pengguna lebih mudah menemukan konten serupa. 


## Kesimpulan
Sistem rekomendasi berbasis konten (content-based filtering) berhasil dibangun dengan memanfaatkan teknik TF-IDF Vectorizer untuk representasi fitur teks dan cosine similarity untuk menghitung kemiripan antar konten. Evaluasi dilakukan secara manual dan juga menggunakan metriks Precision@10 dan Recall@10. Pada evaluasi manual hasil rekomendasi menunjukkan relevansi yang bagus dengan konten input dan pada evaluasi menggunakan matriks menunjukkan bahwa sistem rekomendasi dapat memberikan hasil rekomendasi film secara tepat, meskipun beberapa belum tercover sepenuhnya. Sistem ini mampu memberikan rekomendasi film atau acara TV yang relevan berdasarkan genre, jenis konten, dan deskripsi, tanpa memerlukan data interaksi pengguna seperti rating atau riwayat tontonan. 

