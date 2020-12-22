# Membangun-Infrastruktur-Apache-Spark
Pada kesempatan ini saya akan memberikan tutorial bagaimana cara membangun infrastruktur Apache Spark pada Virtual Machine. Disini saya sudah mempunyai Virtual Machine yang berada di Alibaba Cloud. Didalam Virtual Machine saya sudah terinstall Linux Ubuntu versi 18. Berikut adalah langkah-langkah dalam membangun Infrastruktur Apache Spark:
1.	login ke Virtual Machine

Untuk login ke vitual machine saya menggunakan windows powershell. Lalu ketikan perintah ssh username@alamat_ip. Kemudian kita memasukan password virtual machinenya. Jika username, alamat ip dan passwordnya benar maka kita akan berhasil masuk.  

ssh username@alamat_ip
 
2.	cek update paket

selanjutnya kita ketikan perintah sudo apt update. Perintah ini berfungsi untuk memastikan daftar paket kita dari semua repositori dan PPA terbaru. 

sudo apt update

3.	Install default-jdk

Kemudian kita install jdk dengan perintah sudo apt install default-jdk. JDK atau lengkapnya Java Development Kit adalah sebuah paket aplikasi yang berisi JVM (Java Virtual Machine) + JRE (Java Runtime Environment) + berbagai aplikasi untuk proses pembuatan kode program Java. Salah satu tambahan perintah yang ada di JDK adalah perintah javac yang dipakai untuk memproses kode program Java menjadi byte code. 

sudo apt install default-jdk
 
4.	Melihat versi java

Langkah selanjutnya adalah kita cek jdk kita tadi sudah berhasil kita instal atau belum. Caranya dengan mengecek versi java dengan mengetikan perintah java --version. Jika muncul versinya, maka kita telah berhasil menginstall jdk.

java --version
 
5.	Download paket Apache Spark

Langkah selanjutnya kita download paket file apache spark dari web apache dengan perintah  curl -O https://downloads.apache.org/spark/spark-2.4.7/spark-2.4.7-bin-hadoop2.7.tgz.

curl -O https://downloads.apache.org/spark/spark-2.4.7/spark-2.4.7-bin-hadoop2.7.tgz
 
6.	Ekstrak file apache spark

Setelah kita download filenya lalu kita ekstrak paket file tersebut dengan perintah tar xvzf spark-2.4.7-bin-hadoop2.7.tgz dan kita tunggu prosesnya sampai selesai.

tar xvzf spark-2.4.7-bin-hadoop2.7.tgz
 
7.	Pindahkan file apache spark

Kemudian kita pindahkan file hasil ekstrakan tadi ke dalam folder /opt/spark/ dengan perintah sudo mv spark-2.4.7-bin-hadoop2.7 /opt/spark.

sudo mv spark-2.4.7-bin-hadoop2.7 /opt/spark
 

8.	Menjalankan spark

Setelah itu kita masuk ke folder /opt/spark/ dan kita jalankan spark dengan perintah sudo sbin/start-master.sh.

sudo sbin/start-master.sh
 
 
9.	Mengidentifikasi spark berjalan dimana

Kemudian kita identifikasi spark berjalan dimana dengan menggunakan perintah ss -tunelp |grep 8080.

ss -tunelp |grep 8080
 
10.	Melihat proses dan lokasi spark

Selanjutnya kita coba melihat proses apa saja yang berjalan di sprak dan melihat lokasi spark yang dapat di akses melalui web. Selain itu juga untuk mengetahui url spark untuk digunakan menjalankan perintah slave nantinya. Dengan mengetikan perintah cat /opt/spark/logs/spark-root-org.apache.spark.deploy.master.Master-1-aziz.out.

cat /opt/spark/logs/spark-root-org.apache.spark.deploy.master.Master-1-aziz.out
 
11.	Menjalankan spark 

Kemudian menjalankan spark berdasarkan dari hasil log diatas tadi dengan perintah sudo sbin/start-slave.sh spark://sparktesting.dmxonqygcr0ehciiwimzfeacqd.bx.internal.cloudapp.net:7077. 

 sudo sbin/start-slave.sh spark://sparktesting.dmxonqygcr0ehciiwimzfeacqd.bx.internal.cloudapp.net:7077
 
12.	Membuat file untuk percobaan wordcount

Langkah selanjutnya kita buat file yang nantinya akan dihitung oleh wordcount dengan perintah pico input.txt. kemudian kita isi file tersebut dengan kata-kata bebas.

pico input.txt
 
13.	Masuk ke console scala

Untuk menggunakan wordcount kita masuk terlebih dahulu dengan perintah bin/spark-shell.

bin/spark-shell
 
14.	Memasukan file input.txt

Berikan perintah untuk memasukan file input.txt yang nantinya akan diproses oleh spark dengan perintah  val inputfile = sc.textFile(“input.txt”)

val inputfile = sc.textFile(“input.txt”)
 
15.	Memberikan perintah menghitung kata

ketikan perintah counts untuk membuat logika perhitungan setiap kata yang ada didalam file input.txt tadi dengan kode val counts = inputfile.flatMap(line => line.split(" ")).map(word => (word, 1)).reduceByKey(_+_);

val counts = inputfile.flatMap(line => line.split(" ")).map(word => (word, 1)).reduceByKey(_+_);
 
16.	Counts.cache()

Jalankan perintah counts.cache() seperti pada gambar dibawah ini.

counts.cache()
 
17.	Menyimpan hasil perhitungan

Kemudian kita jalankan perintah counts.saveAsTextFile("output") untuk menyimpan hasil peritungan tadi.

counts.saveAsTextFile("output")
 
18.	Melihat hasil wordcount

Untuk melihat hasil dari wordcount tadi kita keluar terlebih dahulu dari console scala dengan perintah Ctrl+C. Kemudain kita masuk ke direktori output. Terakhir kita jalan perintah cat part-00000 untuk melihat hasil wordcount.
 
 cat part-00000
