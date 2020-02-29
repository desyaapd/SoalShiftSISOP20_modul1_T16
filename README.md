# SoalShiftSISOP20_modul1_T16

Kelompok T16
   - I Gede Arimbawa Teja Putra Wardana || 05311840000045
   - Desya Ananda Puspita Dewi || 05311840000046
   
Soal Shift Modul 1

### Soal 1

Whits adalah seorang mahasiswa teknik informatika. Dia mendapatkan tugas praktikum untuk membuat laporan berdasarkan data yang ada pada file __“Sample-Superstore.tsv”__. Namun dia tidak dapat menyelesaikan tugas tersebut. Laporan yang diminta berupa : 

a. Tentukan wilayah bagian (region) mana yang memiliki keuntungan (profit) paling sedikit 

b. Tampilkan 2 negara bagian (state) yang memiliki keuntungan (profit) paling sedikit berdasarkan hasil poin a 

c. Tampilkan 10 produk (product name) yang memiliki keuntungan (profit) paling sedikit berdasarkan 2 negara bagian (state) hasil poin b Whits memohon kepada kalian yang sudah jago mengolah data untuk mengerjakan laporan tersebut. 

*Gunakan Awk dan Command pendukung*

**Pembahasan:**

untuk mendapatkan file data yang dibutuhkan, maka step pertama yang dilakukan adalah mengunduh link data dengan menggunakan `wget Sample-Superstore.tsv` pada program.

```bash
#!/bin/bash

echo "Region profit terkecil: "
awk -F'\t' 'FNR > 1{SUM[$13] +=$21} END{for (j in SUM) print j, SUM[j]}' Sample-Superstore.tsv | sort -gk2 | awk 'FNR < 2{print$1}'
```

- `echo "Region profit terkecil: "` akan menampilkan hasil output dari command awk

- `-F'\t'` merupakan _field seperator_ pada file yang akan ditambilkan, pemisah `\t` merupakan tab antar file yang ditampilkan

- `FNR > 1` untuk menyimpan jumlah record dalam file yang dituju dan dibaca mulai dari kolom kedua karena pada kolom pertama berisi header

- `{SUM[$13] +=$21}` merupakan kolom yang akan diberi perintah, dimana`$13` merupakan kolom region dan `$21` merupakan kolom profit

- `END{for (j in SUM) print j, SUM[j]` akan menampilkan seluruh profit dari masing masing region yang ada, kemudian akan dilakukan proses penyaringan dengan `sort -gk2`

- gunanya `k2` atau merupakan kolom 2 yaitu jumlah profit yang sesuai dengan `-g` atau generic number, kemudian awk akan menampilkan hasil akhir atau urutan profit terkecil paling sedikit berdasarkan region pada awal pemanggilan dan menampilkan `{print$1}`

```bash
#!bin/bash

awk -F "\t" 'NR>1 {if( $13=="Central" ) { a[$11]+=$21 }} END { for(i in a) print i","a[i] | "sort -t ',' -g -k2"}' Sample-Superstore.tsv | head -n 2
```

- penggunaan `-F "\t"` sama seperti pada jawaban soal sebelumnya dimana ia merupakan _field seperator_ menggunakan pemisah tab.

- `NR > 1` untuk menyimpan jumlah record dan dibaca mulai dari kolom kedua karena pada kolom pertama berisi header

- `( $13=="Central" )` merupakan hasil dari soal sebelumnya dimana profit terkecil terdapat pada region tersebut, sehingga untuk pengeksekusian akan berdasarkan region tersebut.

- kolom state dan profit `{ a[$11]+=$21 }` akan disimpan dalam suatu variabel `$11` dan `$21`

- kemudian hasil dari perintah akan dilakukan penyaringan dengan `"sort -t ',' -g -k2"` yang akan dilihat berdasarkan kolom jumlah profit serta akan ditampilkan menggunakan pembatas atau pemisah `-t ','`

- dimana hasil outputnya akan menampilkan `head -n 2` yang merupakan 2 baris pertama dari hasil eksekusi

```bash
#!bin/bash

awk -F "\t" 'NR>1 { if( ( $11=="Illinois" || $11=="Texas" ) && $13=="Central" ) { a[$17]+=$21; }} END { for(i in a) print i"="a[i] | "sort -t '=' -g -k2" }' Sample-Superstore.tsv | head -n 10
```
- penggunaan `-F "\t"` sama seperti pada jawaban soal sebelumnya dimana ia merupakan _field seperator_ menggunakan pemisah tab.

- `NR > 1` untuk menyimpan jumlah record dan dibaca mulai dari kolom kedua karena pada kolom pertama berisi header

- `( $11=="Illinois" || $11=="Texas" ) && $13=="Central" )` merupakan hasil dari soal sebelumnya dimana profit terkecil terdapat pada region tersebut,  serta 2 state sehingga untuk pengeksekusian akan berdasarkan region dan state tersebut.

- kolom product dan profit `{ a[$17]+=$21 }` akan disimpan dalam suatu variabel `$17` dan `$21`

- kemudian hasil dari perintah akan dilakukan penyaringan dengan `"sort -t '=' -g -k2"` yang akan dilihat berdasarkan kolom jumlah profit serta akan ditampilkan menggunakan pembatas atau pemisah `-t '='`

- dimana hasil outputnya akan menampilkan `head -n 10` yang merupakan 10 baris pertama dari hasil eksekusi





### Soal 2

Pada suatu siang, laptop Randolf dan Afairuzr dibajak oleh seseorang dan kehilangan data-data penting. Untuk mencegah kejadian yang sama terulang kembali mereka meminta bantuan kepada Whits karena dia adalah seorang yang punya banyak ide. Whits memikirkan sebuah ide namun dia meminta bantuan kalian kembali agar ide tersebut cepat diselesaikan. Idenya adalah kalian 

(a) membuat sebuah __script bash yang dapat menghasilkan password secara acak sebanyak 28 karakter yang terdapat huruf besar, huruf kecil, dan angka.__ 

(b) Password acak tersebut disimpan pada file berekstensi `.txt` dengan nama berdasarkan argumen yang diinputkan dan __HANYA berupa alphabet.__ 

(c) Kemudian supaya file `.txt` tersebut tidak mudah diketahui maka nama filenya akan di enkripsi dengan menggunakan konversi huruf (string manipulation) yang disesuaikan dengan jam(0-23) dibuatnya file tersebut dengan program terpisah dengan (misal: password.txt dibuat pada jam 01.28 maka namanya berubah menjadi qbttxpse.txt dengan perintah ‘bash soal2_enkripsi.sh password.txt’. Karena p adalah huruf ke 16 dan file dibuat pada jam 1 maka 16+1=17 dan huruf ke 17 adalah q dan begitu pula seterusnya. Apabila melebihi z, akan kembali ke a, contoh: huruf w dengan jam 5.28, maka akan menjadi huruf b.) dan 

(d) jangan lupa untuk membuat dekripsinya supaya nama file bisa kembali. 

_HINT: enkripsi yang digunakan adalah caesar cipher._

*Gunakan Bash Script*

**Pembahasan:**

```bash
#!/bin/bash

password=$(cat /dev/urandom | tr -dc 'a-zA-z0-9' | fold -w 28 | head -n 1)
namefile=$(echo $1 | tr -d 'a-zA-z')

echo $password > $namefile.txt
```

__Penjelasan:__

Pada soal 2, dijelaskan bahwa Whits membutuhkan ide, dan kami membantunya dengan cara membuat script bash untuk menyelesaikannya. Untuk membuat pasword random, dibutuhkan script seperti

```bash
password=$(cat /dev/urandom | tr -dc 'a-zA-Z0-9' | fold -w 28 | head -n 1)
```

- `cat /dev/urandom` digunakan untuk melakukan atau membuat sebuah file dengan input standar yang berisi output acak atau _random_

- ` | ` pipe merupakan pemisah atau penghubung setelah suatu command telah selesai dikerjakan dan akn dilanjutkan ke command berikutnya

- `tr -dc 'a-zA-Z0-9'` merupakan penerjemahan dari command `cat` sebelumnya, dimana `-dc 'a-zA-Z0-9'` akan melakukan output yang menerjemahkan dan menghapus semua karakter kecuali `a-zA-Z0-9`

- `fold -w 28` disini digunakan untuk membuat __newline__ setelah sudah terdapat 28 karakter yang telah diinput, sehingga menghasilkan output `head -n 1` yang merupakan _line_ pertama dari pengacakan karakter.

- setelah itu, hasil dari output akan disimpan dalam variabel `$password`

```bash
namefile=$(echo $1 | tr -d 'a-zA-z')
```

- `echo $1` akan mengambil atau menginputkan hasil dari argumen yang pertama, argumen sebelumnya

- hasil dari `$1` akan difilter kembali dengan `tr -d 'a-zA-Z'` dimana hasil argumen sebeumnya akan menerjemahkan hasil output dan menyimpan semua karakter yang berupa huruf

- hasil output akhir kemuadian akan disimpan dalam variable `$namefile`

```bash
echo $password > $namefile.txt
```
hasil akhirnya, dari varibale `$password` akan dipindahkan ke dalam variable `$namefile.txt` yang berekstensi `.txt` yang apabila kita memanggil atau membuka isi bash dari `$namefile.txt.txt` akan menampilkan password yang dibuat.

### Soal 3

1 tahun telah berlalu sejak pencampakan hati Kusuma. Akankah sang pujaan hati kembali ke naungan Kusuma? Memang tiada maaf bagi Elen. Tapi apa daya hati yang sudah hancur, Kusuma masih terguncang akan sikap Elen. Melihat kesedihan Kusuma, kalian mencoba menghibur Kusuma dengan mengirimkan gambar kucing. 

[a]  Maka dari itu, kalian mencoba membuat ***script untuk mendownload 28 gambar dari*** ` https://loremflickr.com/320/240/cat ` ***menggunakan command  wget*** dan menyimpan file dengan nama "pdkt_kusuma_NO" (contoh: pdkt_kusuma_1, pdkt_kusuma_2, pdkt_kusuma_3) serta jangan lupa untuk menyimpan log messages wget kedalam sebuah file `wget.log` . Karena kalian gak suka ribet, kalian membuat penjadwalan untuk menjalankan script download gambar tersebut. Namun, script download tersebut hanya berjalan

[b]  ***setiap 8 jam dimulai dari jam 6.05 setiap hari kecuali hari Sabtu***. Karena gambar yang didownload dari link tersebut bersifat random, maka ada kemungkinan gambar yang terdownload itu identik. Supaya gambar yang identik tidak dikira Kusuma sebagai spam, maka diperlukan sebuah ***script untuk memindahkan salah satu gambar identik***. Setelah memilah gambar yang identik, maka dihasilkan gambar yang berbeda antara satu dengan yang lain. Gambar yang berbeda tersebut, akan kalian kirim ke Kusuma supaya hatinya kembali ceria. Setelah semua gambar telah dikirim, kalian akan selalu menghibur Kusuma, jadi gambar yang telah terkirim tadi akan kalian simpan kedalam folder `./kenangan` dan kalian bisa mendownload gambar baru lagi. 

[c]  Maka dari itu buatlah sebuah **script untuk mengidentifikasi gambar yang identik dari keseluruhan gambar yang terdownload tadi**. Bila terindikasi sebagai gambar yang identik, maka sisakan 1 gambar dan pindahkan sisa file identik tersebut ke dalam folder `./duplicate` dengan format filename `duplicate_nomor` (contoh : `duplicate_200, duplicate_201`). Setelah itu lakukan pemindahan semua gambar yang tersisa kedalam folder `./kenangan` dengan format filename `kenangan_nomor` (contoh: `kenangan_252, kenangan_253`). Setelah tidak ada gambar di  current directory  , maka lakukan backup seluruh log menjadi ekstensi `.log.bak` . 

_Hint : Gunakan `wget.log` untuk membuat `location.log` yang isinya merupakan hasil dari grep "Location"._

**Pembahasan:**

```bash
#!/bin/bash

for((i=1;i<29;i++))
do
wget -O pdkt_kusuma_$i https://loremflickr.com/320/240/cat --append-output wget.log >> wget.log
done
```

__Penjelasan:__

`for((i=1;i<29;i++))` kondisi ini digunakan untuk menamai gambar atau melakukan proses _looping_ angka pada setiap file yang terunduh dari link yang diberikan Kusuma.

```bash
wget -O pdkt_kusuma_$i https://loremflickr.com/320/240/cat --append-output wget.log >> wget.log
```
- gambar akan terdownload dari link `https://loremflickr.com/320/240/cat` dengan menggunakan perintah `wget`

- `-O pdkt_kusuma_$i` akan menampilan output dari file yang telah terunduh dengan menamainya pdkt_kusuma dan `$i` merupakan angka yang akan mengurutkan filenya berdasarkan urutan unduhan.

- kemudian perintah pengunduhan atau _log messages_ akan dipindahkan atau disimpan kedalam `wget.log`

__KENDALA YANG DIALAMI__

   - link data pada soal pertama hanya dapat diunduh di salah satu laptop praktikan, sehingga tidak keduanya dapat berlatih atau mengerti isi soal sepenuhnya

   - karena kurangnya pemahaman dalam soal karena soal yang diberikan jauh lebih sulit dibandingkan soal pada saat asistensi

   - dalam pengisian readme terdapat kekacauan serta kurangnya koordinasi antar pengerjaan, sehingga file edit milik praktikan tidak dapat terbaca dan hanya dapat diedit oleh 1 orang.
