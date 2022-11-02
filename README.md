# Network-Analysis
# **Laporan Proyek Machine Learning - Habib Nurkholis M004X0007**
___
## **Domain Proyek**
---
Pada 2021 terdapat setidaknya 3.75 juta penyandang tunanetra di Indonesia yang berarti lebih dari 1.5% populasi penduduk Indonesia. Jumlah tersebut terbilang sangat signifikan namun banyak penyandang tunanetra yang masih belum bisa menjadi bagian penuh dari masyarakat. Salah satu faktor yang menyebabkan kesenjangan tersebut adalah sulitnya komunikasi antara penyandang tunanetra dan non disabilitas. 

Huruf braille merupakan salah satu sarana menyampaikan pesan kepada penyandang tunanetra sehingga mereka dapat mendapatkan informasi dan pengetahuan sebagaimana non disabilitas. Meskipun pemahaman mengenai huruf braille ini cukup penting seperti halnya pemahaman akan bahasa isyarat untuk tunarungu, banyak orang yang masih belum menguasainya.
## **Business Understanding**
---
### **Problem Statements**
Bagaimana cara untuk membantu melancarkan komunikasi antara penyandang tunanetra dan non disabilitas?

### **Goals**
Tujuan dari proyek ini adalah memberikan platform bagi non disabilitas untuk mengerti huruf braille sehingga dapat membantu mereka berkomunikasi dengan penyandang disabilitas.
### **Solution Statements**
Solusi yang ditawarkan dalam proyek ini adalah membuat sebuah model Machine Learning menggunakan jaringan syaraf tiruan untuk mengidentifikasi gambar huruf braille dan mengubahnya ke dalam bentuk teks.

## **Data Understanding**
---
Data yang digunakan untuk proyek ini didapatkan dari kaggle yang dapat diakses melalui link berikut    
[Kaggle - Braille Character Dataset](https://www.kaggle.com/datasets/shanks0465/braille-character-dataset)
### **Exploratory Data Analysis** 
**Judul**     
Braille Character Dataset

**Deskripsi**    
Dataset ini dibuat untuk tujuan training Convolutional Neural Network pada Braille Character Recognition.

**Deskripsi gambar**   
Setiap gambar memiliki resolusi 28x28 pixel dalam Black White Scale.
Setiap gambar berisi karakter alfabet dan nomor setiap gambar, contoh f1.JPG6whs dan f2.JPG6whs.

**Augmentasi gambar**   
Setiap data gambar memiliki augmentasi khusus yang dikodekan di setiap penamaan data tersebut, yakni 
* whs - width height shift    
* rot - Rotation    
* dim - brightness

**Komposisi Dataset**   
_26 karakter * 3 augmentasi * 20 gambar_ yang berbeda yang di augmentasi seperti different shift, rotational dan brightness value, dengan total data **1560** gambar.

## **Data Preparation**
---
Data image yang diambil dari Kaggle API masih berada dalam satu direktori yang sama sehingga perlu dipisahkan sesuai label huruf di dalam folder yang berbeda. Pada tiap folder terdapat 60 data yang tersusun seperti berikut
> a    
  ---> a1whs.jpg    
  ---> a1rot.jpg    
  ---> a1dim.jpg    
  ---> ...

Data image diberikan augmentasi tambahan dengan melakukan 
* Rotasi dengan rentang -20 hingga +20 derajat    
* Shearing ke arah sumbu x dan y dengan rasio sebesar 0.2
    
Data kemudian dipisah menjadi data training dan data validation dengan rasio 8:2 sehingga didapatkan 
* Data training sejumlah 1248 dalam 26 kelas    
* Data validation sejumlah 312 dalam 26 kelas

## **Modeling**
---
**Input Layer**    
Pada proyek ini model ML yang digunakan adalah model Sequential dengan input berupa tensor array 28 x 28 x 3 yang merupakan input data image dengan ukuran 28 x 28 pixel dan data komposisi warna rgb. 

**Hidden Layer**    
Selain layer input model memiliki 12 hidden layer dimana data image selanjutnya masuk ke layer scaling dimana akan mengubah nilai dari data array dengan mengalikannya dengan 1/255 untuk selanjutnya data image memasuki layer convolution 2D dan maxpooling. Terdapat 2 layer Dropout dalam model yang berfungsi untuk mencegah overfitting.

**Output Layer**      
Output layer menghasilkan 26 nilai dari setiap kelas yang diberikan dimana nilai tertinggi merupakan kelas yang memiliki probabilitas terbesar.

**Compiling**    
Optimizer yang digunakan adalah adam, loss function yang digunakan adalah categorical crossentropy, dan metric yang digunakan untuk mengetahui performa model adalah akurasi.

**Training**    
Training model dilakukan selama 100 epoch dan dilakukan callback ketika akurasi training dan validation mencapai 92%. Didapatkan performansi model cukup tinggi dengan akurasi sebesar 92% dan loss yang rendah di kisaran 0.2.

**Testing**    
Selanjutnya untuk mengetahui kinerja model, diberikan data image di luar yang digunakan untuk training dan evaluasi untuk dilihat ketepatan prediksi model. Hasil dari testing ini ialah hanya 3 dari 7 data image saja yang diprediksi dengan benar. 

## **Evaluation**
---
**Evaluasi**    
Model yang telah dibuat memiliki performansi yang cukup baik ketika memprediksi gambar huruf braille yang memiliki bentuk geometris yang baik akan tetapi tidak bisa memprediksi gambar huruf braille asli. Oleh karena itu dapat dikatakan bahwa model ini masih belum bisa digunakan secara aplikatif di lapangan, akan tetapi dapat digunakan untuk keperluan pembelajaran. Untuk melanjutkan penerapan lebih lanjut model dapat dideploy ke dalam format TFLite.

**Saran**    
Selanjutnya untuk pengembangan lebih lanjut dapat dilakukan penambahan sebagai berikut :
* Penambahan data berupa data huruf braille asli 
* Penggunaan image preprocessing tambahan untuk meningkatkan kualitas data
* Prediksi terhadap beberapa huruf dalam satu frame image yang sama
* Pengembangan yang lebih jauh sehingga dapat mendeteksi kata dan kalimat
