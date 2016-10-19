#ALGORITMA DES

A. Dokumentasi

Implementasi algoritma DES ini dengan menggunakan java. 
Line 9 merupakan pendeklarasian key yang akan digunakan dengan tipe data string
Line 10 untuk mendeklarasikan plaintext  yang nantinya juga akan diinputkan yang tipe datanya string
Line 11 mendeklarasikan leftshift yang dibuat dalam array sesuai dengan ketentuan pergeseran dalam algoritma des
Line 12-14 mendeklarasikan matriks intial permutation matrixIP 
Line 15-24 mendeklarasikan matrixIvIp
Line 25-27 mendeklarasikan matriks PcSatu untuk permutasi yang pertama key
Line 28-37 mendeklarasikan matriks PcDua untuk permutasi yang kedua key
Line 38-39 deklarasi matriks untuk melakukan expand
Line 40 mendeklarasikan array baru untuk sboxes
Line 42 mendeklarasikan matriks untuk pBox
Line 50-103 mendeklarasikan array sbox1 hingga sbox8

Line 107-115 merupakan bagian utnuk mengkonversi dari teks menjadi biner. untuk setiap byte, plainbiner akan di kelompokkan menjadi 8bit per bagian.  Plainbiner yang berupa spasi akan di gantikan menjadi 0

Line 118-137 merupakan bagian untuk membuat block biner 64 bit. Pada bagian ini di deklarasikan index=0. PlainBinnerBlocks akan dibentuk/dideklarasikan kedalam array. Ketika plainBinerBlock panjangnya sama dengan 64 bit maka akan dimasukkan kedalam block. Ketika plainbinerblock lebih kecil maka ditambahkan angka default. yang mana pada kodingan akan nilai defaultnya adalah 11111111 
 
Line 140-148 merupakan bagian untuk melakukan permutasi des. Dideklarasikan index merupakan sebuah int.  plainbinerblock yang berupa matriks. tiap elemen dalam matriks di masukkan kedalam variabel index yang terlihat pada line 143-144. Lalu dilakukan proses permutasi sesuai dengan array permutasi des yang telah di deklarasikan sebelumnya

Line 150-152 Untuk merubah key yang diinputkan kedalam binner

Plaintext yang telah dalam bentuk biner yang telah dilakukan intial permutation sebelumnya akan dibagi menjadi 2 bagian yaitu left dan right yang dibagi masing masingnya 32 bit pada line 154-160.

Key yang dalam bentuk biner dan telah dilakukan permutasi hingga dari 64 bit menjadi 56 bit dibagi menjadi 2 bagian getleftkeyBiner dan getRightKeyBiner yang dibagi 28 bit per masing-masing. 

line 170-174 public string leftshift untuk melakukan proses pergeseran, sesuai dengan matriks pergeseran yang telah di deklarasikan sebelumnya.

line 176-177 melakukan merge, menggabungkan leftkeybiner dengan rightkeybiner yang telah dilakukan leftshift .
line 180-181 proses mengexpand key sesuai matriks 
line 184-192 proses xor
line 194-212 melaukan proses sBox yang terdiri dari 8 sbox, yang setiap keyBiner dibagi menjadi 6bit permasing masing block

line 214 method akan melakukan proses encrypt
line215 plaintext diubah menjadi biner
line 216 List<String> plaintextBlocks = makeBlocks(plaintext,64); untuk mengecek plaintext berjumlah 64 bit tidak bisa lebih, jika kurang maka digunakan nilai default sesuai line 118-137. 
        List<String> K = new ArrayList<>(); --> Dibuat array baru yang di beri nama K, nantinya untuk menyimpan nilai nkey.
        List<String> C = new ArrayList<>(); --> Dibuat array baru dengan nama C, nantinya untuk leftkeybiner
        List<String> D = new ArrayList<>(); --> Dibuat array baru dengan nama D, untuk rightkeybiner
        List<String> L = new ArrayList<>(); --> Dibuat array baru dengan nama L untuk leftplainbiner
        List<String> R = new ArrayList<>(); --> Dibuat array baru dengan nama R nantinya untuk rightplainbiner
        List<String> A = new ArrayList<>(); --> Dibuat array baru dengan nama A nanti digunakan untuk xor
        List<String> B = new ArrayList<>(); --> Dibuat array baru dengan nama B untuk proses setelah sbox
        List<String> PB = new ArrayList<>(); --> dibuat array  baru untuk pBox
        List<String> chiper = new ArrayList<>(); --> array untuk chiper

Untuk setiap plaintextblocks 64 bit, maka
dilakukan permutasi terhadap plaintextblock dengan matrixIP/matriks intial maka ditulis perintah : plaintextBlock = permutation(plaintextBlock,matrixIp);

leftPlaintext = getLeftPlainBiner(plaintextBlock); --> dilakukan pembagian plainbiner yang telah di permutasi sebelumnya bagian left (32bit)
rightPlaintext = getRighPlainBiner(plaintextBlock);--> bagian plain biner yang kanan (32bit)
L.add(leftPlaintext); //L0 --> leftplaintext yang di dapatkan di masukkan dalam array L
R.add(rightPlaintext); //R0 -->rightplaintext dimasukkan dalam array R

System.out.println("Key : " + keyToBiner(key));
key = keyToBiner(key); --> dilakukan proses key yang dimasukkan menjadi biner
K.add(key); //K0 --> key dimasukkan kedalam array K
key = permutation(key, matrixPcSatu); --> selanjutnya dilakukan permutasi key yang telah jadi biner sesuai dengan matrixPcSatu

System.out.println("Key -> PC-1 : " + key);
System.out.println("-------------------");
          
C.add(getLeftKeyBiner(key));//C0 --> key yang telah di permutasi menjadi 56 bit dibagi menjadi 2 bagian. Bagian yang kiri akan dimasukkan dalam array C (0-28bit)
D.add(getRightKeyBiner(key));//D0 --> key bagian kanan dimasukkan dalam array D (28-56 bit)

Selanjutnya akan dilakukan proses pergeseran key ke kiri

for (int n=0; n<16; n++){ -->pergeseran key dilakukan sebanyak 16 kali sehingga dibuat perulangan dari n=0 hingga n<16 
C.add(leftShift(C.get(n), listShift[n+1])); --> setiap pergeseran bagian kiri key disimpan kembali kedalam array C
D.add(leftShift(D.get(n), listShift[n+1])); --> setiap pergeseran bagian kanan key kembali disimpan di aray D

key = C.get(n+1)+D.get(n+1); -->selanjutnya array c dan array d digabung menjadi key

k = permutation(key, matrixPcDua); --> key yang telah digabung dari array c dan D selanjutnya dilakukan permutasi yang kedua dengan menggunakan matrixPcDua.

K.add(k); --> Selanjutnya key yang telah dilakukan permutasi PC2 akan dimasukkan kedalam array K

a = xor(K.get(n+1),expand(R.get(n),matrixExpand));
A.add(a);
Selantjutnya dilakukan array R akan dilakukan expand dengan matriks expand yang selanjutnya akan di lakukan proses xor antara key n+1 dengan R yang telah di expand. Selanjutnya akan dimasukkan kedalam array A

b = sBox(A.get(n+1),this.sBoxes);
B.add(b); //B1
Dilakukan proses sBox yaitu array A dengan sBoxes yang telah di deklarasikan sebelumnya. Lalu dilakukan penambahan kedalam array B

b = permutation(b,pBox);
PB.add(b); //PB1
Selanjutnya dilakukan permutasi dari hasil sbox sebelumnya dengan matriks pbox lalu ditambahkan kedalm array PB

leftPlaintext = R.get(n);
L.add(leftPlaintext);
array R yang berisi leftPlaintext dimasukkan kedalam array L

rightPlaintext = xor(L.get(n),PB.get(n+1));
R.add(rightPlaintext);
plaintext yang bagian kanan akan berupa hasil xor antara array L(n) dengan array PB(n+1). yang selanjutnya akan dimasukkan kedalam array R

chiper.add(permutation(L.get(16)+R.get(16),matrixIvIp));
Selanjutnya dihasilkan chiper/ dimasukkan kedalam array chipper yang berupa permutasi dari gabungan array L dan R dengan matriks matrixIvIp

---------------------------------------------------------------------------------------------------------------------------------------

B. Konstribusi
Kontribusi dalam Kelompok :
Membantu dalam bagian pengkodingan permutasi dalam algoritma des
