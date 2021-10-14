
## Table of Content
1. Intro
1. Core
    - Intro to Web Scraping
    - Web Scraping Technique
    - Scraping on Qwiklabs Public Profile
1. Closing
1. References

## Intro
Beberapa waktu lalu pada event JuaraGCP Season 5, Komunitas mengalami masalah untuk melakukan validasi: "Quest apa sajakah yang sudah saya selesaikan dan valid untuk season ini?"

Sehingga terbersitlah untuk membuat aplikasi validator yang ada pada tautan [ini](https://juaragcps5-qwik-validator.herokuapp.com/)

Bagaimanakah cara kerjanya situs ini?

Yuk kita cari tau lebih lanjut dengan membaca tulisan ini sejenak.

## Core

### Intro to Web Scraping
Inti dari aplikasi ini sebenenarnya adalah pada suatu teknik yang dinamakan dengan `Web Scraping`.

Apakah `Web Scraping` ini?

Mengutip dari niagahoster, pengertian dari *web scraping* adalah suatu `proses pengambilan data dari sebuah website`.

Proses ini sebenarnya bisa saja dilakukan secara manual dan secara otomatis.

Bayangkan bila kita membuat suatu artikel situs koran digital, kemudian kita menyalin seluruh isi dari suatu artikel tersebut, dari situs koran digital, ke dalam aplikasi catatan kita (e.g. notepad / texteditor), maka kita secara tidak langsung sudah melakukan *web scraping* ini *loh* !

Namun dalam proses yang ada tentunya kita tidak ingin melakukan selamanya manual seperti itu terus bukan?

Tentunya kita pada akhirnya ingin melakukan proses otomasi untuk melakukan *web scraping* ini, nah pada titik inilah kita harus melakukan proses untuk memikirkan dengan melakukan koding, membuat aplikasi, ataupun menulis ekstensi pada browser kesayangan kita untuk bisa melakukan *web scraping* ini.

Dan perlu diingat:  
Mengambil data dari suatu web, merupakan ranah abu abu (*gray zone*) yang masih diperdebatkan legalitasnya, ada beberapa yang bilang boleh boleh saja, ada beberapa yang bilang sebenarnya tidak boleh.

Pada tulisan ini, *web scraping* digunakan untuk pembelajaran semata yang bertujuan membantu komunitas yah 😉

### Web Scraping Technique
Sebenarnya dalam Web Scraping ini ada berbagai macam teknik:
- Menyalin data manual
- Menggunakan *regular expressions* (RegEx)
- Parsing HTML
- Menganalisis DOM
- Menggunakan XPath
- sampai dengan Menggunakan google sheet

namun, pada tulisan ini, yang difokuskan adalah pada *Parsing HTML* dan *Analisis DOM* saja yah.

Secara dasarnya yang akan digunakan adalah, kita akan meminta aplikasi yang kita untuk melakukan request HTTP ke server yang menyimpan datanya untuk kita ekstrak, kemudian kita akan menganalisis isi dari HTML tersebut (berupa DOM), untuk mendapatkan data data yang kita akan gunakan.

Analogi sederhananya adalah sebagai berikut:  
Dengan menggunakan browser, apabila kita membuka situs apapun, maka sebenarnya, kita akan mengakses sebuah **penyedia** situs, kemudian **penyedia** situs akan memberikan kita suatu halaman berupa HTML yang akan di-*render* oleh browser untuk menampilkannya lebih indah.

Nah dengan melakukan *web scraping* ini, kita akan mengambil halaman HTML yang ada, kemudian menganalisis isi dari HTML tersebut untuk mendapatkan data yang kita inginkan, dan memprosesnya lebih lanjut !

### Scraping on Qwiklabs Public Profile
Nah sebagai contoh pembelajaran dari *web scraping* ini, kita akan menggunakan public profile qwiklabs yang ada pada tautan [ini](https://www.qwiklabs.com/public_profiles/bfc56f2a-5078-4b06-8fd8-7defddf3db6e).

**Disclaimer:**  
Perlu diingat bahwa teknik untuk *web scraping* bisa jadi berbeda untuk situs yang berbeda, jadi cara yang ada pada pembelajaran ini, bisa dijadikan dasar pembelajaran, namun belum tentu bisa digunakan **sama persis** pada situs lainnnya

Bukalah link ini dengan browser kesayangan masing masing (pada contoh ini penulis menggunakan Firefox sebagai browsernya).

Pada tautan di atas, berisi konten "badge" dari Quest pada qwiklabs apa sajakah yang sudah pernah diselesaikan oleh seseorang.

Dari sini sebenarnya konten apa sajakah yang bisa kita dapatkan?

Mari kita coba untuk meng-*inspect element* dari tautan ini.

Klik kanan pada browser di halaman kosong, kemudian pilih inspect:

![assets01](/assets/image01.png)

Kemudian browser akan menampilkan kode HTML yang merupakan struktur dari tampilan yang ada pada halaman *public profile qwiklabs* itu sendiri.

Selanjutnya kita akan mengarahkan pada salah satu badge yang ada pada halaman tersebut.

Tekan tombol `Pick Element` Kemudian arahkan ke salah satu badge yang ada pada halaman tersebut, kemudian click element tersebut sehingga pada nanti pada tab `inspector` yang ada di browser akan mengarah pada element yang diklik tadi

![assets02](/assets/image02.png)

![assets03](/assets/image03.png)

![assets04](/assets/image04.png)

Dan dari sini dapat diketahui beberapa hal:
- Seluruh badge yang ada pada *public qwiklabs profile*, ada di dalam sebuah `div` yang memiliki `class` bernama `profile-badges`
- Untuk setiap badge yang ada, dibungkus dalam sebuah `div` yang memiliki `class` bernama `profile-badge`
- dalam `div profile-badge` tersebut, memiliki dua buah `span`, dimana `span` pertama adalah nama dari quest, dan `span` kedua adalah tanggal menyelesaikan quest tersebut

dengan kata lain, struktur yang kita inginkan sebenarnya adalah sebagai berikut:

```
div profile-badges
  div profile-badge
    span (Nama Quest)
    span (Tanggal Quest Selesai)
  div profile-badge
    span (Nama Quest)
    span (Tanggal Quest Selesai)
  div profile-badge
    span (Nama Quest)
    span (Tanggal Quest Selesai)
  dst ...
```

Nah dari sinilah baru kita akan mulai membuat aplikasi web scrapingnya

Secara sederhananya alur dari aplikasi web scrapingnya adalah sebagai berikut:
1. Mengambil HTTP Request berdasarkan link public profile qwiklabs yang ada
2. Mengambil `div` dengan class `profile-badges`
3. Mengambil `div` dengan class `profile-badge` dan mengambil `span` yang ada.
4. Menyusunnya menjadi data yang dibutuhkan
5. Membandingkan data yang dibutuhkan dengan data kevalidan quest yang ada serta tanggal penyelesaiannya
6. Menampilkan hasilnya dalam bentuk visual yang lebih indah agar enak dipandang.

Cara untuk membuat ini bisa menggunakan bahasa pemrograman apapun.

Namun penulis menggunakan nodejs dengan stack sebagai berikut:
- Express, Pug, dan Tailwind

Untuk pembahasan *ngoding*-nya mungkin akan dilanjutkan di lain hari, namun untuk aplikasi jadinya bisa dibuka pada tautan di bawah ini yah.

https://github.com/withered-flowers/apps-validate-qwiklabs-juaragcp

## Closing
Nah setelah mempelajari *Web Scraping* ini jadi tertarik untuk mengambil data dari situs mana nih?

Diingatkan sekali lagi yah, bahwa *Web Scraping* ini adalah ranah abu abu, jadi, gunakan sebijaknya yah !

## References
- https://www.niagahoster.co.id/blog/web-scraping/