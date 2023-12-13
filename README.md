# RSA_DES

## rsa 
<hr>

RSA merupakan algoritma kriptografi kunci publik, yang berarti bahwa terdapat sepasang kunci yang berbeda untuk enkripsi dan dekripsi. Pasangan kunci ini terdiri dari kunci publik dan kunci privat.

Untuk mendapatkan kunci dilakukan dengan pemilihan dua bilangan prima kemudian mengalikan antar keduanya untuk mendapatkan ```n````.

setelah itu akan dikalingan  masing-masing bilangan prima tersebut yang sudah dikurangi satu terlebih dahulu untuk mendapatkan fungsi *phi euler*.

- *Public key* didapatkan dengan cara memilih acak bilangan antar 2 dan *phi euler* dimana bilangan tersebut harus relatif prima terhadap *phi euler* (memiliki nilai FPB 1).
- *Private key* didapatkan dengan cara mencari invers modulo dari *public key* terhadap *phi euler*. Yang bisa dengan menggunakan cara mencari suatu bilangan yang memenuhi ```(d * e) % phi_n == 1```.

<br>

Untuk proses **enkripsi** dilakukan dengan cara setiap karakter dalam suatu pesan akan dikonversi ke nilai ASCII-nya kemudian akan dilakukan persamaan ```pesan ^ public_key % modulus``` atau ```pesan ^ e % n```.

Sedangkan, untuk proses **dekripsi** dilakukan dengan cara setiap angka yang merepresentasikan tiap karakter dalam suatu pesan akan dipangkatkan dengan *private key*-nya dan dimodulus dengan nilai *modulus* yang sudah ditetapkan sebelumnya. ```pesan ^ d % n````.

<br>

## des
<hr>

Penjelasan tentang *Data-Encrypted-System* ini bisa dilihat lebih lanjut pada: [Data-Encrypted-System](https://github.com/azharanas17/Data-Encryption-Standard/tree/master)


<br>

## rsa dan des
<hr>
Server akan membuka socket dan cliet akan menghubungkannya pada socket tersebut dengan alamat dan port yang sama.

Kemudian pada client akan membangkitkan kunci yang akan digunakan pada rsa. dan mengirimkan public_keynya dengan dilakukan encode terlebih dahulu

Di bagian server akan menghasilkan client_id untuk masing-masing client, lalu mengirimkannya ke client dengan dilakukan encode terlebih dahulu.

Jika server mengirimkan dictionary *public keys* maka Client akan menerimanya dan memperbarui *public keys* yang ada di Client.
Jika server mengirimkan *public key* dengan *client_id* maka Client akan menerima *public_key* yang dimiliki *client_id* tertentu.

Dan jika server mengirimkan data dengan nilai *step* maka Client akan menerimanya dan mengecek pada *step* berapa saat ini. Tapi data di sini merupakan data yang dikirim oleh Client lain, sehingga server hanya meneruskan ke Client yang dituju saja.

*=//=//=//=//=//=//=//=//=//=//=* <br>
Nilai *step*: 1 dihasilkan saat client dalam *state*: listen. Dan pada saat itu client akan menghasilkan suatu nilai acak ```n_1``` kemudian dienkripsi dengan menggunakan *public_key* dari targetnya dan mengirimkan hasil enkripsi tersebut kepada targetnya.

Saat nilai *step*: 1 diterima pada suatu Client, maka Client tersebut akan melakukan dekripsi terhadap data yang dikirim dengan menggunakan *private_key*nya dan menyimpan bilangan acak ```n_1``` yang dikirim dari Client pengirim.
Selanjutnya Client yang menerima akan mengembalikan nilai ```n_1``` yang diterima dan ditambahkan dengan ```n_2``` yang ia hasilkan sendiri kepada Client pengirim. Tentunya setelah dienkripsi. Dan nilai *step* menjadi 2.

Saat nilai *step*: 2 diterima pada suatu Client, maka Client tersebut akan melakukan dekripsi juga dengan menggunakan *private_key*nya sendiri dan menyimpan ```n_1``` dan ```n_2``` yang telah dikirim. Jika ```n_1``` yang dikirim pada awal (saat nilai *step* belum menjadi 1) dan yang diterima saat ini tidak sama, maka akan terjadi error. Namun jika sudah sama maka dilanjutkan dengan mengirimkan kembali ```n_2``` pada pengirim. 

Saat nilai *step*: 3 diterima pada suatu Client, maka Client tersebut kembali melakukan dekripsi dan menyimpan ```n_2``` yang dikirim. ```n_2``` saat ini lagi-lagi harus sama dengan saat dirinya mengirim pada *step* sebelumnya. Jika sudah sama, *state* selanjutnya menjadi "chat" dan yang dikirim saat ini adalah ```n_1``` dan ```key``` untuk des. Sehingga untuk selanjutnya, selain memproses di fungsi ```receive_messages()``` untuk nilai *step*: 4 juga memproses state *chat* pada ```main function```.