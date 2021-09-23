# Jarkom-Modul-1-B06-2021

## Anggota Kelompok : 
- 05111940000048 | Bagaskoro Kuncoro Ardi 
- 05111940000096 | Stefanus Albert Kosim 

# Display Filter

### 1. Sebutkan webserver yang digunakan pada "ichimarumaru.tech"! 

Jawab :
``` 
nginx/1.18.0 (Ubuntu)
Expression : " http.request and http.host ichimarumaru.tech "
```

Screenshot :

Untuk menemukan web server yang digunakan, maka paket perlu di *stream* terlebih dahulu. Hasil dari *stream* akan menampilkan server yang digunakan.

![Stream](https://github.com/yanzkosim/Jarkom-Modul-1-B06-2021/blob/main/Screenshots/1.1.png)

![Resulit](https://github.com/yanzkosim/Jarkom-Modul-1-B06-2021/blob/main/Screenshots/1.2.png)

### 2. Temukan paket dari web-web yang menggunakan basic authentication method!

Jawab : 
```
Expression : "http.authbasic"
```

Screenshot : 

![No2](https://github.com/yanzkosim/Jarkom-Modul-1-B06-2021/blob/main/Screenshots/2.1.png)

### 3. Ikuti perintah di basic.ichimarumaru.tech! Username dan password bisa didapatkan dari file .pcapng!

Jawab : 
```
Username dan Password didapatkan dengan menggunakan :
Expression : “http.authbasic”
dimana pada Credentials berisi username:password

Urutan konfigurasi pengkabelan T568A adalah :
putih hijau, hijau, putih orange, biru, putih biru, orange, putih cokelat, cokelat
```

Screenshot :

![UnamePass](https://github.com/yanzkosim/Jarkom-Modul-1-B06-2021/blob/main/Screenshots/3.1.png)

![T568A](https://github.com/yanzkosim/Jarkom-Modul-1-B06-2021/blob/main/Screenshots/3.2.png)

### 4. Temukan paket mysql yang mengandung perintah query select!

Jawab : 
```
Expression : “ mysql.query contains "select" "
```

Screenshot : 

![select](https://github.com/yanzkosim/Jarkom-Modul-1-B06-2021/blob/main/Screenshots/4.png)

### 5. Login ke portal.ichimarumaru.tech kemudian ikuti perintahnya! Username dan password bisa didapat dari query insert pada table users dari file .pcap!

Jawab : 
```
Untuk mendapatkan username dan password, digunakan ekspresi:
Expression : “ mysql matches "insert" "

Urutan konfigurasi pengkabelan T568B adalah :
putih orange, orange, putih hijau, biru, putih biru, hijau, putih cokelat, cokelat
```

Screenshot :

![Filter](https://github.com/yanzkosim/Jarkom-Modul-1-B06-2021/blob/main/Screenshots/5.1.png)

![Webpage](https://github.com/yanzkosim/Jarkom-Modul-1-B06-2021/blob/main/Screenshots/5.2.png)

### 6. Cari username dan password ketika melakukan login ke FTP Server!

Jawab : 
```
Expression : "ftp.request.command == USER || ftp.request.command == PASS"
```

Screenshot :

![Result](https://github.com/yanzkosim/Jarkom-Modul-1-B06-2021/blob/main/Screenshots/6.1.png)

### 7. Ada 500 file zip yang disimpan ke FTP Server dengan nama 0.zip, 1.zip, 2.zip, ..., 499.zip. Simpan dan Buka file pdf tersebut. (Hint = nama pdf-nya "Real.pdf")

Jawab : 
```
Karena diberikan hint bahwa nama pdf adalah Real.pdf, maka dapat digunakan frame karena menunjuk pada metadata
Expression : " frame contains "Real.pdf" "

Untuk mengunduh filenya, paket terlebih dahulu di-stream, lalu sourcenya diganti menjadi RAW dan kemudian disimpan sebagai nama file yang asli
```

Screenshot :

![Result](https://github.com/yanzkosim/Jarkom-Modul-1-B06-2021/blob/main/Screenshots/7.1.png)

### 8. Cari paket yang menunjukan pengambilan file dari FTP tersebut!

Jawab : 
```
Karena pengambilan file sama dengan mengunduh, maka operasi yang akan muncul pada Info di Wireshark adalah RETR
Expression : "ftp-data.command contains "RETR" "
Hasilnya adalah tidak terdapat paket yang menunjukkan pengambilan file dari ftp
```

Screenshot :

![Result](https://github.com/yanzkosim/Jarkom-Modul-1-B06-2021/blob/main/Screenshots/8.1.png)

### 9.Dari paket-paket yang menuju FTP terdapat indikasi penyimpanan beberapa file. Salah satunya adalah sebuah file berisi data rahasia dengan nama "secret.zip". Simpan dan buka file tersebut!

Jawab : 
```
Karena dari nomor 8 tidak ditemukan data yang diambil, maka dapat langsung dicari berdasarkan string "secret.zip"
Expression : " ftp-data.command contains "secret.zip" "

Untuk mengunduh filenya, paket terlebih dahulu di-stream, lalu sourcenya diganti menjadi RAW dan kemudian disimpan sebagai nama file yang asli yaitu secret.zip
```

Screenshot :

![Result](https://github.com/yanzkosim/Jarkom-Modul-1-B06-2021/blob/main/Screenshots/9.1.png)

### 10. Selain itu terdapat "history.txt" yang kemungkinan berisi history bash server tersebut! Gunakan isi dari "history.txt" untuk menemukan password untuk membuka file rahasia yang ada di "secret.zip"!

Jawab : 
```
Dilakukan filter untuk mencari paket dengan info yang mengandung string "history.txt"
Expression : " ftp-data.command contains “history.txt” "

Selanjutnya dilihat detailnya dan ditemukan history berupa command untuk melakukan zip dengan password $key, dimana $key berada pada file "bukanapaapa.txt"

Selanjutnya dilakukan filter lagi untuk mencari paket dengan info yang mengandung string "bukanapaapa.txt"
Expression : " ftp-data.command contains "bukanapaapa.txt" " 

Pada detail bukanapaapa.txt, hanya terdapat 1 line string yaitu password yang akan digunakan untuk membuka Wanted.pdf
```

Screenshot :

![history.txt](https://github.com/yanzkosim/Jarkom-Modul-1-B06-2021/blob/main/Screenshots/10.1.png)

![bukanapaapa.txt](https://github.com/yanzkosim/Jarkom-Modul-1-B06-2021/blob/main/Screenshots/10.2.png)

![Wanted.pdf](https://github.com/yanzkosim/Jarkom-Modul-1-B06-2021/blob/main/Screenshots/10.3.png)

# Capture Filter

### 11. Filter sehingga wireshark hanya mengambil paket yang berasal dari port 80! 
```
Digunakan expression capture filter
" src port 80 "
```

Screenshot :

![Result](https://github.com/yanzkosim/Jarkom-Modul-1-B06-2021/blob/main/Screenshots/11.png)

### 12. Filter sehingga wireshark hanya mengambil paket yang mengandung port 21!
```
Digunakan expression capture filter
"  port 21 "
```

Screenshot :

![Result](https://github.com/yanzkosim/Jarkom-Modul-1-B06-2021/blob/main/Screenshots/12.png)

### 13. Filter sehingga wireshark hanya menampilkan paket yang menuju port 443!
```
Digunakan expression capture filter
"  dst port 443 "
```

Screenshot :

![Result](https://github.com/yanzkosim/Jarkom-Modul-1-B06-2021/blob/main/Screenshots/13.png)

### 14. Filter sehingga wireshark hanya mengambil paket yang tujuannya ke kemenag.go.id!
```
Digunakan expression capture filter
" dst host kemenag.go.id "
```

Screenshot :

![Result](https://github.com/yanzkosim/Jarkom-Modul-1-B06-2021/blob/main/Screenshots/14.png)

### 15. Filter sehingga wireshark hanya mengambil paket yang berasal dari ip kalian!
```
Digunakan expression capture filter
"  src host 192.168.100.8 "
```

Screenshot :

![Result](https://github.com/yanzkosim/Jarkom-Modul-1-B06-2021/blob/main/Screenshots/15.png)
