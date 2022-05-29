# Rakamin

Nama Kelompok :
Team Five

Anggota kelompok :
Trie Sony K.,
Amru,
M. Reza Syahziar,
Aulia Pradana R.,
M. Shohih Alwi,
Gilang Fitra R.

Dataset : Used Car Auction Prices (https://www.kaggle.com/datasets/tunguz/used-car-auction-prices)

## EDA

Descriptive Analysis (Summary)
- Semua kolom tipenya sudah sesuai
- Kolom dengan missing value : `make`, `model`, `trim`, `body`, `transmission`, `condition`, `odometer`, `color`, dan `interior`
- Nilai min pada selling price dan odometer menunjukkan angka yang tidak wajar dan perlu dicek kembali, sementara itu freq pada transmission mengalami data imbalance dimana sebanyak 475914 adalah automatic dari 493458 jumlah data. Unique pada model, trim dan body sangat banyak sehingga tidak direkomendasikan untuk menjadi feature atau target!


Univariate Analysis (Summary)
- Feature ‘condition’ memiliki rata - rata bernilai 3.39, median bernilai 3.5, dan nilai modus bernilai 1.9 (median > mean > modus), maka feature tersebut memiliki tipe distribusi Negatively Skewed maka diperlukan proses transformasi data agar dapat lebih mudah untuk dianalisis. dan dapat dilihat pada boxplot data sample tidak terdapat Outlier
- `odometer`, `mmr` memiliki distribusi yang positively skewed, rekomendasi untuk dilakukan log transformation
- `year` memiliki distribusi yang negatively skewed, rekomendasi anti-log transformation atau dibiarkan


Multivariate Analysis (Summary)
- Dari table korelasi dapat dilihat bahwa mmr mempunyai korelasi positif yang sangat kuat dengan sellingprice, feature ‘condition’ memiliki nilai korelasi paling kecil dengan target. Feature yang perlu dipertahankan adalah ‘year’, ‘condition’ dan ‘odometer’
- ‘odometer’ dan ‘year’ cenderung redundan dapat menyebabkan multicollinearity, perlu dipertimbangkan drop salah satu yang memiliki korelasi kecil terhadap target

Business Insight 
(1) 
Pengamatan awal: Beberapa warna mendominasi jumlah sales
Pertanyaan bisnis: Apakah warna punya pengaruh terhadap sales? Adakah insight yang bisa digali untuk mendapat rekomendasi bisnis?

(2)
Pengamatan awal : Merk, Jenis, dan Tahun mobil tertentu mendominasi top 10 penjualan mobil bekas
Pertanyaan Bisnis :  Apakah Merk, Bentuk, dan Tahun mobil tersebut berpengaruh terhadap penjualan?
 
(3)
Pertanyaan Bisnis : Seller mana yang paling banyak memberi keuntungan terhadap perusahaan kita?

## Data Pre-Processing 

> Data Cleansing

Missing Values (Summary)
- Kolom yang didrop dikarenakan kardinalistas yang tinggi yaitu : ['model','trim','vin','mmr','seller']
- Kolom dengan missing value : `make`, `body`, `transmission`, `condition`, `odometer`, `color`, dan `interior`
- Nilai missing value di jadikan persen dan nilai yang datanya memiliki missing value lebih dari 10% akan didrop, seluruh data memiliki missing value dibawah 10% kecuali ‘transmission’ yang memiliki nilai missing value 11.7%, 
- Transmission diisi menggunakan filna yang nilainya disi dengan modus

Duplicated Data (Summary)
- Untuk duplicated bernilai 0 yang berarti tidak ada data yang terduplikasi

Outliers (Summary)
- Handling outlier menggunakan metode menggunakan Z-Score berdasarkan nilai  z-score kurang dari 3, tetapi terdapat juga saran untuk tetap menggunankan nilai outier ini dikarenakan nilai tersebut memang nilai yang wajar dalam auction mobil meskipun nilainya extream,  jika menggunakan metode z-score data  berkurang sebanyak 25058 baris

> Feature Engineering

Transformation (Summary)
- Log Transformasi pada data ini hanya ingin menunjukkan bagaimana distribusinya, tetapi karena hasil perubanhan log transformasi hasil yang didapatkan tidak memuaskan jadi tidak diaplikasikan dalam data yang dimiliki, melakukan pengamatan dengan kolom odometer dan sellingprice

Encoding (Summary)
- Untuk kolom dengan nilai unik tinggi disederhanakan nilainya agar lebih mudah untuk proses yang akan dijalankan pada machine learning,  kolom tersebut yaitu `make`, `body`,dan `state`.
- Body yang awalnya memiliki nilai unique 43 menjadi 10, dengan encoding huruf besar kecil dan pembenaran kata-kata yang sebenarnya sama 
- Untuk kolom state dikelompokkan menjadi beberapa nilai unik, pada kolom ini berisi kode negara Unite State dan beberapa negara lain yang pengelompokkannya menjadi ['west','south','northeast','midwest’, 'outside_us’] 
- Untuk kolom 'saledate’ diproses menjadi day, month dan year agar dapat mendukung proses yang berikutnya  

Feature Encoding (Summary)
- Feature encode menggunakan Onehot Encoding dengan hasil kolom sebanyak 122, yang hasil dari dataPre-processing ini kan dilanjutkan untuk pembutan model machine learning


## Modeling

Split Data Train & Test(Summary)
- Data yang sudah di Preprocessing disimpan/ di write pada file bernama ‘clean_dataset.csv’, data ini yang akan di bagi menjadi data Training dan data Testing dengan persentase Training set : Testing set =  80:20
- Dari proses preprocessing feature yang didapatkan sebanyak 121, yang fiture ini akan digunakan untuk mencari harga mobil
- Random set yang digunakan yaitu 69

Modeling (Algoritma yang diimplementasikan) (Summary)
- Algoritma yang dicoba pada data set ini yaitu Linear dan Non Linear, untuk Linear yaitu : Linear Regression, Ridge, Lasso, ElasticNet, dan XGB Regressor . Untuk Non Linear yaitu : Decision Tree dan Random Forest

Model Evaluation: Pemilihan dan perhitungan metrics (Summary)
- Metrics Evaluation yang digunakan untuk menentukan model yang akan diaplikasikan pada regresi prediksi  harga mobil yaitu : 𝑹^𝟐,  𝑹𝑴𝑺𝑬( 𝑹𝒐𝒐𝒕 𝑴𝒆𝒂𝒏 𝒔𝒒𝒖𝒂𝒓𝒆 𝑬𝒓𝒓𝒐𝒓), dan 𝑴𝑨𝑬(𝑴𝒆𝒂𝒏 𝑨𝒃𝒔𝒐𝒍𝒖𝒕𝒆 𝑬𝒓𝒓𝒐𝒓)  
- Dari hasil yang didapatkan untuk nilai 𝑹^𝟐 , yang paling bagus dan yang akan diaplikasikan yaitu untuk Algoritma Linear metode XGB Regressor 

Model Evaluation: Apakah Model sudah best-fit (Summary)
Model sudah di anggap Best-fit dikarenakan model yang digunakan memiliki nilai Model Evaluation yang terbaik  dengan nilai:
      𝑅^2 = 0.836
      𝑅^2 = 0.844
      RMSE = 3329.1142
      MAE = 2294.3902

Hyperparameter Tuning (Summary)
Hyperparameter Tuning yang digunakan pada pemodelan ini menggnakan parameter: 
    learning_rate=[0.05, 0.1, 0.2], 
    n_estimators=[100, 400, 800],
    max_depth=[3, 6, 9],
    min_child_weight=[1, 10, 100]


> Feature Importance

Feature Importance (Summary)
Dari Feature Importance didapatkan feature-feature dengan skor yang tinggi, dari hasil tersebut feature dengan nilai skor yang tinggi akan diteruskan pada proses Feature Selection

Feature Selection (Summary)





