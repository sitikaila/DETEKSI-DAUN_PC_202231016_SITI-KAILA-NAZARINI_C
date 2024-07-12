
# Penjelasan Mengenai Project UAS PCD

## 1. Teori yang mendukung
Teori Dasar

a. Pemrosesan Citra Digital

Pemrosesan citra digital melibatkan manipulasi dan analisis gambar dengan menggunakan komputer. Teknik ini digunakan dalam berbagai aplikasi seperti pengenalan wajah, pengawasan video, dan analisis medis. Pemrosesan citra mencakup berbagai operasi seperti peningkatan gambar, segmentasi, ekstraksi fitur, dan analisis pola.

b. Konversi Ruang Warna

Proyek ini dimulai dengan mengkonversi citra input dari ruang warna BGR (Biru, Hijau, Merah) ke ruang warna HSV (Hue, Satuasi, Nilai) menggunakan cv2.cvtColor(img, cv2.COLOR_BGR2HSV). Hal ini karena HSV adalah ruang warna yang lebih sesuai untuk segmentasi berbasis warna. Konversi dari RGB ke HSV berguna karena dalam ruang warna HSV, informasi warna (Hue) dipisahkan dari informasi intensitas (Saturation dan Value), sehingga lebih mudah untuk segmentasi berdasarkan warna.

c. Ruang Warna (Color Space)

Ruang warna adalah model matematis yang menggambarkan warna sebagai angka atau vektor. Dua ruang warna yang paling umum digunakan dalam pemrosesan citra adalah RGB dan HSV.

- RGB (Red, Green, Blue): Ini adalah ruang warna yang paling umum digunakan dalam perangkat tampilan. Warna dihasilkan dari kombinasi intensitas tiga warna dasar: merah, hijau, dan biru.
- HSV (Hue, Saturation, Value): Ruang warna ini lebih dekat dengan cara manusia melihat warna.
    - Hue (Corak): Menggambarkan jenis warna.
    - Saturation (Kejenuhan): Menggambarkan kecerahan atau kekuatan warna.
    - Value (Nilai): Menggambarkan kecerahan warna.

Dalam proyek ini, ruang warna RGB digunakan sebagai input citra. Namun, ruang warna RGB tidak sesuai untuk segmentasi berbasis warna karena tidak dapat membedakan antara warna yang sama tapi memiliki tingkat kecerahan yang berbeda.

d. Thresholding dan Masking

Thresholding adalah metode untuk mengonversi gambar abu-abu menjadi gambar biner. Masking adalah proses menerapkan mask pada gambar untuk mengekstrak bagian tertentu dari gambar. Proyek ini menggunakan thresholding untuk memisahkan buah dan daun dari sisanya citra. Thresholding adalah teknik yang mengkonversi citra menjadi citra biner (yaitu citra dengan hanya dua warna: hitam dan putih) berdasarkan nilai ambang tertentu.
Dalam proyek ini, dua nilai ambang ditentukan untuk buah dan daun:
- lower_frt dan upper_frt untuk buah
- lower_leaf dan upper_leaf untuk daun
Nilai ambang ini digunakan untuk membuat masker untuk buah dan daun menggunakan cv2.inRange(hsv, lower_frt, upper_frt) dan cv2.inRange(hsv, lower_leaf, upper_leaf), masing-masing. Fungsi inRange mengembalikan citra biner di mana piksel dengan nilai dalam rentang yang ditentukan diatur ke 255 (putih) dan piksel di luar rentang diatur ke 0 (hitam).

e. Operasi Bitwise

Proyek ini menggunakan operasi AND bitwise untuk menerapkan masker ke citra asli. Fungsi cv2.bitwise_and melakukan operasi AND piksel demi piksel antara citra asli dan masker. Hasilnya adalah citra di mana hanya piksel yang sesuai dengan buah atau daun yang dipertahankan, sementara sisanya citra diatur ke hitam. Operasi bitwise memungkinkan manipulasi langsung pada bit individual dalam gambar. Dalam konteks pemrosesan citra, operasi ini digunakan untuk menggabungkan gambar atau bagian-bagian gambar dengan cara tertentu. cv2.bitwise_and, operasi ini digunakan untuk menerapkan mask pada gambar sehingga hanya bagian gambar yang sesuai dengan mask yang ditampilkan.

### Teori Segmentasi Berbasis Warna
Segmentasi berbasis warna adalah teknik yang digunakan untuk memisahkan objek dari citra berdasarkan sifat warna mereka. Dalam proyek ini, ruang warna HSV digunakan karena lebih sesuai untuk segmentasi berbasis warna daripada ruang warna BGR.

Ruang warna HSV adalah sistem koordinat silinder di mana:

- Hue (H) mewakili nada warna (0-360 derajat)
- Satuasi (S) mewakili kemurnian warna (0-255)
- Nilai (V) mewakili kecerahan (0-255)

Dengan mengkonversi citra ke HSV, kita dapat memisahkan buah dan daun berdasarkan nilai hue mereka. Misalnya, buah cenderung memiliki nilai hue antara 0-30 derajat (rentang warna merah-jingga), sementara daun cenderung memiliki nilai hue antara 30-90 derajat (rentang warna hijau).

Dengan menerapkan thresholding ke citra HSV, kita dapat membuat masker yang memisahkan buah dan daun dari sisanya citra. Masker yang dihasilkan kemudian dapat digunakan untuk memisahkan buah dan daun dari citra asli.

## 2. Tahapan Menyelesaikan Project
Langkah 1 : Mengimpor Library

Proyek ini dimulai dengan mengimpor library yang diperlukan:
- cv2 (OpenCV) untuk pengolahan citra
- numpy (np) untuk komputasi numerik
- matplotlib.pyplot (plt) untuk plotting dan visualisasi

Langkah 2 : Membaca Citra

Pada langkah ini, membaca citra dari file menggunakan fungsi cv2.imread(). Citra disimpan dalam variabel img. Fungsi cv2.imread() mengembalikan array 3-dimensi, di mana setiap piksel diwakili oleh tiga nilai: biru, hijau, dan merah (BGR).

Langkah 3 : Mengkonversi Citra ke RGB

Pada langkah ini,  mengkonversi citra dari ruang warna BGR ke ruang warna RGB menggunakan fungsi cv2.cvtColor(). Hal ini karena sebagian besar algoritma pengolahan citra mengharapkan ruang warna RGB, dan OpenCV menggunakan ruang warna BGR secara default. Citra yang dikonversi disimpan dalam variabel image_rgb. Mengonversi gambar dari ruang warna BGR (standar di OpenCV) ke RGB untuk visualisasi dengan Matplotlib, dan dari BGR ke HSV untuk pemrosesan lebih lanjut. 

Langkah 4 : Mengkonversi Citra ke HSV

Pada langkah ini, mengkonversi citra dari ruang warna BGR ke ruang warna HSV (Hue, Saturation, Value) menggunakan fungsi cv2.cvtColor(). Ruang warna HSV lebih sesuai untuk segmentasi berbasis warna, yang akan di lakukan pada proyek ini. Citra yang dikonversi disimpan dalam variabel hsv. Ruang warna HSV memisahkan informasi warna dari intensitas, sehingga memudahkan segmentasi berdasarkan warna. 

Langkah 5 : Mendefinisikan Ambang untuk Buah dan Daun

Pada langkah ini, mendefinisikan dua set ambang untuk segmentasi buah dan daun:

- lower_frt dan upper_frt untuk segmentasi buah
- lower_leaf dan upper_leaf untuk segmentasi daun
Ambang-ambang ini didefinisikan dalam ruang warna HSV dan digunakan untuk memisahkan buah dan daun dari sisanya citra.

Langkah 6 : Membuat Mask untuk Buah dan Daun

Membuat dua mask menggunakan fungsi cv2.inRange(), mask ini akan memiliki nilai putih (255) untuk piksel yang berada dalam rentang warna yang ditentukan dan hitam (0) untuk piksel lainnya.
- Untuk buah, rentang warna yang digunakan adalah lower_frt = np.array([0, 50, 50]) dan upper_frt = np.array([30, 255, 255]).
- Untuk daun, rentang warna yang digunakan adalah lower_leaf = np.array([30, 40, 40]) dan upper_leaf = np.array([90, 255, 255]).

Langkah 7 : Segmentasi Daun

melakukan segmentasi buah dan daun dari citra asli menggunakan fungsi cv2.bitwise_and():

- frt_just untuk segmentasi buah
- leaf_just untuk segmentasi daun

Citra-citra yang dihasilkan diperoleh dengan melakukan operasi AND bitwise antara citra asli dan masker yang sesuai.

Langkah 8 : Visualisasi Hasilnya

memvisualisasikan hasil menggunakan library matplotlib.pyplot:

- Citra asli ditampilkan di pojok kiri atas
- Masker buah ditampilkan di pojok kanan atas
- Hasil segmentasi buah ditampilkan di pojok kiri bawah
- Hasil segmentasi daun ditampilkan di pojok kanan bawah

Fungsi plt.tight_layout() digunakan untuk memastikan bahwa subplot-subplot tersebut teralign dengan baik, dan fungsi plt.show() digunakan untuk menampilkan plot.
