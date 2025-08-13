## Transformer-Time-Series-Encoder
Pada repositori ini saya menggunakan metode **Transformer** yang dikhususkan untuk ***Time Series* menggunakan *Encoder Layers*** pada arsitektur transformer. Data yang saya gunakan ialah data **Indeks Harga Saham Gabungan (IHSG)** dengan satu variabel saja yaitu harga **penutupan atau *close***.

Dalam proses pemodelan, saya membuat alur sebagai berikut:
1.  **Pra-pemrosesan Data**: Membersihkan data, menangani nilai yang hilang dengan interpolasi spline, dan melakukan transformasi *log return* untuk menstabilkan varians.
2.  **Arsitektur Model**: Membangun model Transformer dari awal menggunakan PyTorch, yang terdiri dari *Positional Encoding* dan *Transformer Encoder Layers*.
3.  **Pelatihan & Evaluasi**: Melatih model pada data historis IHSG dan mengevaluasinya menggunakan metrik seperti MAPE, MAE, dan RMSE.
4.  **Prediksi**: Menggunakan model yang telah dilatih untuk memprediksi pergerakan IHSG untuk 30 hari kerja ke depan.

Berikut merupakan grafik untuk IHSG serta grafik transformasi data IHSG menggunakan Logreturn 
<img width="1589" height="990" alt="image" src="https://github.com/user-attachments/assets/7ce8843c-53e1-4b21-bdf0-92b89eccccb4" />

Pada bagian ***set-hyperparameter***, saya memisahkannya agar dapat lebih mudah untuk pemodelan dengan penjelasan sebagai berikut:
**Pengaturan Proses Pelatihan**
patience & min_delta: Parameter ini berfungsi sebagai "rem darurat". Jika model berhenti menunjukkan kemajuan yang berarti pada data baru setelah beberapa siklus, pelatihan akan dihentikan secara otomatis untuk mencegah overfitting (terlalu menghafal data latihan).
splitting: Ini adalah rasio untuk membagi data menjadi dua set: satu untuk melatih model dan satu lagi untuk menguji seberapa baik performanya pada data yang belum pernah dilihat.
epochs: Menentukan batas maksimal berapa kali model akan "belajar" dari keseluruhan data latihan.
learning_rate: Mengatur seberapa cepat model mengoreksi kesalahannya selama proses belajar.
batch_size: Jumlah sampel data yang diproses oleh model dalam satu langkah sebelum memperbarui pengetahuannya.
**Pengaturan Arsitektur Model Transformer**
input_window: Menentukan panjang "memori" model, yaitu berapa banyak data historis (hari) yang dilihat untuk membuat satu prediksi.
output_window: Menentukan seberapa jauh ke depan model akan mencoba untuk meramal.
fs (feature_size): Mengatur "kedalaman" atau kompleksitas informasi yang direpresentasikan oleh setiap titik data di dalam model.
nl (num_layers): Jumlah lapisan pemrosesan yang ditumpuk di dalam inti Transformer. Semakin banyak lapisan, semakin kompleks pola yang bisa dipelajari.
do (dropout): Sebuah teknik untuk membuat model lebih tangguh dengan cara "melupakan" sebagian kecil informasi secara acak selama pelatihan, sehingga tidak terlalu bergantung pada pola tertentu.
nh (num_heads): Memungkinkan model untuk memperhatikan beberapa bagian berbeda dari data historis secara bersamaan, seperti seorang analis yang melihat beberapa indikator sekaligus.
