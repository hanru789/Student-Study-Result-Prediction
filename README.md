# Laporan Proyek Machine Learning- Ruhan Masykuri
## Domain Proyek
Proyek ini dilakukan untuk membuat model yang dapat memperkirakan kemungkinan hasil belajar siswa di sekolah. Sekolah adalah tempat siswa melakukan belajar secara intensif sehingga keberhasilan sekolah tidak hanya bergantung pada kegiatan siswa selama jam pelajaran. Berdasarkan penelitian diketahui bahwa sarapan pagi memiliki pengaruh signifikan terhadap konsentrasi siswa pada jam pelajaran berlangsung [^1]. Selain itu latar belakang siswa, kegiatan siswa diluar jadwal sekolah, dan berbagai hal lainnya tentu memiliki korelasi tersendiri terhadap capaian  sekolah para siswa.

Projek ini perlu dilakukan karena model yang dihasilkan dapat memperkirakan bagaimana prestasi yang dapat dicapai siswa di sekolah. Dengan mengetahui hal tersebut diharapkan siswa maupun keluarga siswa dapat mengevaluasi dan meningkatkan hal yang perlu ditingkatkan agar capaian  pembelajaran siswa dapat meningkat.

## Busines Understanding

Siswa dapat dikatakan siswa ketika dia melakukan kegiatan pembelajaran di sekolah. Selain itu siswa dapat berperan sebagai anak bagi orang tuanya di rumah, sebagai kakak bagi saudaranya, sebagai mitra mungkin bagi yang berjiwa entrepreneur. Tentunya hal ini mengakibatkan kegiatan siswa tidak sepenuhnya bisa dialokasikan untuk keberhasilan pembelajarannya di sekolah. Setiap kegiatan dari masing-masing peran diluar jam pelajaran tentu akan berpengaruh terhadap kegiatan siswa pada jam belajarnya. Sehingga dengan melakukan projek ini kita dapat mengusahakan peningkatan pada faktor-faktor yang dapat menunjang keberhasilan prestasi belajar siswa
- Problem statements
    Bagaimana pengaruh latar belakang dan keseharian siswa terhadap capaian pembelajarannya di sekolah?
- Goals
    Membuat model machine learning yang dapat memprediksi kinerja belajar siswa di sekolah berdasarkan latar belakang siswa dan kesehariannya.

## Data Understanding

Projek ini dilakukan dengan menggunakan dataset [Students Performance in Exams](https://www.kaggle.com/spscientist/students-performance-in-exams) yang dapat didownload di kaggle [^2]. Data ini berisi variabel berupa gender, etnis, pendidikan orang tua, makan siang, persiapan ujian, nilai matematika, nilai membaca, dan nilai menulis. Setiap siswa memiliki latar belakang yang bervariasi dan nilai ujian yang diperoleh pun juga berbeda-beda.

Berdasarkan variable inilah nantinya dibuat model prediksi yang dapat memperkirakan capaian prestasi siswa. Data ini terdiri dari seribu baris dan delapan kolom. Delapan kolom adalah sebagai berikut:
- Gender
    Data non numerik berupa kategori male dan female. Pada variabel ini akan dilakukan one hot encoding.

![This is an image](https://github.com/hanru789/submision-image/blob/main/pie-gender.png)
- Etnis
    Data non numerik berupa kategori group A, group B, group C, group D, dan group E. Pada kolom ini juga akan dilakukan one hot encoding

![](https://github.com/hanru789/submision-image/blob/main/pie-race.png)
- Parental level of education
    Data non numerik dengan kategori associate's degree, bachelor's degree, high school, master's degree, some college, some high school. Pada kolom ini juga akan dilakukan one hot encoding

![](https://github.com/hanru789/submision-image/blob/main/pie-parent_education.png)
- Lunch
    Data non numerik berupa kategori free/reduced dan standard. Kolom ini juga akan dilakukan one hot encoding

![](https://github.com/hanru789/submision-image/blob/main/pie-linch.png)
- Test preparation course
    Data non numerik berupa kategori completed dan none. Akan dilakukan one hot encoding pada kolom ini.

![](https://github.com/hanru789/submision-image/blob/main/pie-test_prep.png)
- Math score
    Data numerik. Pada kolom ini terdapat nilai siswa dengan nilai nol, hal ini terdeteksi ketika dilakukan pengecekan data yang hilang. Setelah ditelusuri itu bukan merupakan missing value karena memang suatu hal yang mungkin seorang siswa mendapatkan nilai nol pada ujian matematika. Pada data numerik ini terdapat outliers sehingga perlu ditangani.
    
    ![](https://github.com/hanru789/submision-image/blob/main/outlier-math.png)
- Reading score
    Data numerik. Pada data numerik reading score ini terdapat outliers sehingga perlu ditangani.
    
    ![](https://github.com/hanru789/submision-image/blob/main/outlier-read.png)
- Writing score
    Data numerik. Pada data numerik writing score ini terdapat outliers sehingga perlu ditangani.
    
    ![](https://github.com/hanru789/submision-image/blob/main/outlier-write.png)

Yang menjadi atribut pada dataset ini adalah kolom gender, etnis, parental level of education, lunch, dan test preparation course. Targetnya adalah nilai rata-rata dari kolom math score, reading score, dan writing score. Rata-rata dari ketiga nilai ujian tersebut dapat mewakili penilaian keberhasilan belajar siswa. Nilai rata-rata berupa numerik diubah menjadi predikat sehingga nilai akhir yang menjadi target adalah predikat A sampai E.

## Data Preparation
- Data Cleaning
    Ketika dilakukan pengecekan missing value terdapat sebuah nilai nol pada kolom math test. Setelah diselidiki ternyata itu tidak dapat dikategorikan sebagai missing value karena tidak mustahil siswa memperoleh nilai nol dalam pelajaran matematika. Pada data numerik terdapat outliers sehingga dilakukan penanganan menggunakan IQR Methode. Metode ini dilakukan dengan membuang data yang memiliki nilai lebih kecil dari quartil bawah dikuran 1,5 IQR dan quartil atas ditambah 1,5 IQR.
- Feature Selection
    Pada proyek ini tidak dilakukan feature selection karena seluruh fitur digunakan baik sebagai atribut maupun target. Hal ini dikarenakan proyek ini bukan berorientasi sepenuhnya untuk mendapatkan akurasi prediksi maksimal, tetapi untuk mengkategorikan keberhasilan siswa di sekolah berdasarkan faktor-faktor yang ada.
- Data Transform
    Data non numerik terdapat pada kolom gender, etnis, parental level of education, lunch, dan test preparation  course. Data non numerik ini perlu diubah menjadi data numerik agar dapat dimengerti oleh komputer. Dilakukan teknik one hot encoder sehingga kolom non numerik berubah menjadi beberapa kolom sejumlah variasi nilainya dengan nilai berupa biner. Lima kolom kemudian berubah menjadi 17 kolom yang kemudian menjadi atribut dalam model machine learning.
- Feature Engineering
    Data numerik pada dataset ini adalah math score, reading score, dan writing score. Nilai ketiga kolom ini dirata-ratakan  dan menjadi satu kolom rata-rata nilai. Nilai pada kolom rata-rata memiliki range dari 1 – 100 sehingga perlu diubah menjadi lima predikat.
    | Predikat  | Nilai |
    | -------- | ----- |
    | A | 90 - 100 |
    | B | 80 - 89 |
    | C | 70 - 79 |
    | D | 60 - 69 |
    | E | < 60 |
   
   Nilai berupa predikat ini diolah kembali menggunakan one hot encoder sehingga menjadi lima kolom. Lima kolom ini yang menjadi label pada model machine learning.

- Data splitting dilakukan untuk melihat kinerja model machine learning perlu dilakukan pengujian terhadap model tersebut. Untuk itu perlu dilakukan pembagian dari seluruh dataset menjadi 80 persen data training dan 20 persen data yang digunakan untuk pengujian.

Data yang terdapat pada dataset tidak serta merta bisa digunakan langsung menjadi input model machine learning. Kadang dataset itu bisa berupa gambar, suara, dan teks yang tidak bisa dimengerti oleh komputer. Jika kita memaksakan  data mentah langsung menjadi input training model machine learning, model yang dihasilkan tidak akan bekerja optimal. Berbagai teknik perlu dilakukan kepada data agar model yang dihasilkan dapat bekerja dengan optimal.

 


## Modeling
Modeling adalah tahap pemilihan model yang dirasa paling powerful sesuai kebutuhan. Terdapat banyak model yang bisa digunakan untuk suatu dataset. Pada projek ini digunakan neural network. Neural network adalah suatu teknik machine learning yang meniru cara kerja sistem saraf manusia. Setiap perceptron mewakili sebuah sel saraf pada otak manusia. Cara kerja neural network dapat dapat dibagi menjadi empat tahap berikut:
- Layer pertama berfungsi sebagai input yang menerima masukan berupa numerik dari atribut dataset. Jumlah perceptron pada layer pertama ini disesuaikan dengan berapa input yang dibutuhkan. Model berupa neural network empat layer dengan 17 input

```sh
Dense(17, activation='relu', input_shape=(17,)),   
```
- Bobot dari input lah yang akan melatih perceptron menentukan berapa parameternya.

- Selanjutnya setiap input akan dikalikan dengan bobotnya masing-masing.lalu hasilnya akan ditambahkan dengan bias
- Layer ke-2 dan ke-3 merupakan hidden layyer. Layer ke-2 memiliki input 17 dari layer sebelumnya dan menghasilkan output 13 untuk layer ke-3.
- Layer ke-3 memiliki input 13 dan menghasilkan output 9 untuk output layer.

```sh
Dense(13, activation='relu'),
Dense(9, activation='relu'), 
```
- Output layer mendapat input 9 dari layer sebelumnya. Selanjutnya adalah mengaplikasikan fungsi aktivasi yang akan menyesuaikan kebutuhan output dari neural network. Output yang dihasilkan adalah 5 output.

```sh
Dense(5, activation='softmax')
```
- Optimizer yang digunakan sgd dan metrik accuracy

```sh
model.compile(optimizer='sgd', loss='mean_squared_error', metrics=['accuracy'])
```
Neural network ini menggunakan 4 layer yang mana layer pertama menjadi tempat masukan input dan layer keempat akan menghasilkan output berupa prediksi. Jumlah perceptron pada input layer dan output layer haruslah sama dengan jumlah input dan output dari model yang dibuat. Sedangkan pada hidden layer tidak ada ketentuan pasti berapa perceptron yang dibutuhkan, kita bisa melakukan beberapa percobaan untuk mendapatkan berapa perceptron yang optimal agar akurasi yang diperoleh mumpuni dan proses komputasi tidak memakan banyak waktu.

## Evaluation
Untuk menilai kinerja model digunakan metric accuracy. Metric accuracy menilai akurasi model machine learning. Artinya seberapa baikkah machine learning dapat melakukan tugas prediksi berdasarkan atribut yang diberikan. Untuk memahami bagaimana akurasi dapat diperoleh, dapat dilihat pada formula berikut.

![](https://github.com/hanru789/submision-image/blob/main/accuracy-formula.PNG)

- TN = True Negative
- TP = True Positive
- FN = False Negative
- FP = False Positive

Untuk melihat berapa nilai loss dari model digunakan Mean Square Eror. MSE adalah nilai rata-rata dari kuadrat perbedaan data atau nilai error. Untuk lebih jelas dapat dilihat pada formula berikut.

![](https://github.com/hanru789/submision-image/blob/main/loss-mse.PNG)




Pada projek ini model machine learning dapat melakukan prediksi dengan akurasi 0,45. Sedangkan akurasi dengan data validation diperoleh val_accuracu  sebesar 0,48


![](https://github.com/hanru789/submision-image/blob/main/plot-try.png)


[^1]: L. A. Arifin, "HUBUNGAN SARAPAN PAGI DENGAN KONSENTRASI SISWA DI SEKOLAH," Jurnal Pendidikan Olahraga dan Kesehatan, pp. 203 - 207, 2015. 
[^2]: J. Seshapanpu, "kaggle," 2019. [Online]. Available: https://www.kaggle.com/spscientist/students-performance-in-exams. [Accessed 04 02 2022].
