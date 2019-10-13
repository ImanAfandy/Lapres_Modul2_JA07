# Lapres_Modul2_JA07

sebelum mengerjakan soal yang diberikan kita harus terlebih dahulu membuat topologi uml-nya.
### langkah-langkah :
a. membuat file topologi seperti gambar. 
![1](https://user-images.githubusercontent.com/45744801/66715095-50300d00-ede9-11e9-9b8c-0fb5139f9178.PNG)

b. bash file topologi yang telah dibuat.

c. login ke semua uml.

d. Hilangkan tanda pagar (#) bagian net.ipv4.ip_forward=1 dengan perintah nano /etc/sysctl.conf pada router pikachu.
![2](https://user-images.githubusercontent.com/45744801/66715122-b4eb6780-ede9-11e9-82d9-28ffc1d3149b.PNG)

e. jalankan perintah sysctl -p.

f. setting IP dari setiap UML dengan dengan perintah nano /etc/network/interfaces.
![3](https://user-images.githubusercontent.com/45744801/66715181-583c7c80-edea-11e9-8cfe-ed71809194be.PNG)
![4](https://user-images.githubusercontent.com/45744801/66715183-58d51300-edea-11e9-8ac3-1e18abcabf4d.PNG)
![5](https://user-images.githubusercontent.com/45744801/66715184-596da980-edea-11e9-9f6e-4c74a9692435.PNG)
![6](https://user-images.githubusercontent.com/45744801/66715185-5a064000-edea-11e9-8673-ff104f40329b.PNG)
![7](https://user-images.githubusercontent.com/45744801/66715186-5a9ed680-edea-11e9-9a28-b5fe8e7053a4.PNG)
![8](https://user-images.githubusercontent.com/45744801/66715187-5b376d00-edea-11e9-9d6a-fc47b30e4566.PNG)

g. ketikkan perintah service networking restart di setiap uml setelah setting ip.

h. ketikkan perintah atau membuat file script yang berisi iptables –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 192.168.0.0/16.

i. Export proxy pada setiap UML dengan sintaks seperti di bawah ini:

#### export http_proxy=”http://ITS-564415-ab673:798d2@proxy.its.ac.id:8080”
#### export https_proxy=”http://ITS-564415-ab673:798d2@proxy.its.ac.id:8080”
#### export ftp_proxy=”http://ITS-564415-ab673:798d2@proxy.its.ac.id:8080”

j. setelah itu lakukan perintah apt-get update.

#### 1. membuat sebuah website utama dengan alamat http://kanto.yy.com yang diatur DNS-nya pada ARTICUNO dan mengarah ke IP Server MEWTWO
langkah-langkah :

a) Buka ARTICUNO dan update package lists dengan menjalankan command: apt-get update

b) Setelah melakukan update silahkan install aplikasi bind9 pada ARTICUNO dengan perintah: apt-get install bind9 -y

c) Lakukan perintah pada ARTICUNO. Isikan seperti berikut: nano /etc/bind/named.conf.local

d) Isikan konfigurasi domain kanto.a7.com sesuai gambar berikut :
```
zone "kanto.a7.com"{
	type master;
	file "/etc/bind/jarkom/jarkomtc.com";
	};
 ```
e) Buat folder dengan perintah mkdir /etc/bind/jarkom

f) copykan file db.local pada path /etc/bind ke dalam folder kanto.a7.com yang baru saja dibuat dan diubah namanya menjadi jarkom

g) lakukan nano /etc/bind/jarkom/kanto.a7.com dan edit seperti di gambar.
![10](https://user-images.githubusercontent.com/45744801/66715382-e31e7680-edec-11e9-8445-c2961e72e313.PNG)

h) service bind9 restart

#### 2. memiliki alias http://www.kanto.yy.com, yang diatur DNS-nya pada ARTICUNO dan mengarah ke IP Server MEWTWO
langkah-langkah :

a. tingal menambahkan 
```www	IN CNAME kanto.a7.com
```
![11](https://user-images.githubusercontent.com/45744801/66715479-00a01000-edee-11e9-9a88-35fd1f3f2f0d.PNG)

b) lalu service bind9 restart
#### 3. dan subdomain http://www.pallet.kanto.yy.com yang diatur DNS-nya pada ARTICUNO dan mengarah ke IP Server MEWTWO
langkah-langkah :

a). tinggal menambahkan
```www.pallet IN A 10.151.73.67
```
![12](https://user-images.githubusercontent.com/45744801/66715480-00a01000-edee-11e9-8e00-c38692669c75.PNG)

b) lalu service bind9 restart

#### 4. membuat reverse domain.
langkah-langkah :

a) buka nano /etc/bind/named.conf.local

b) Lalu tambahkan konfigurasi berikut ke dalam file named.conf.local
```zone "73.151.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/73.151.10.in-addr.arpa";
};
```
c) lakukan cp /etc/bind/db.local /etc/bind/jarkom/73.151.10.in-addr.arpa

d) Edit file 73.151.10.in-addr.arpa menjadi gambar di bawah ini
![13](https://user-images.githubusercontent.com/45744801/66715595-5e812780-edef-11e9-830a-6b219e7aa5e4.PNG)

e) service bind9 restart

#### 5. Untuk mengantisipasi server rusak, mereka meminta dibuatkan DNS Server Slave pada MOLTRES agar layanan tidak terganggu.
langkah-langkah :

##### I. Konfigurasi Pada Server ARTICUNO
a) Edit file /etc/bind/named.conf.local dan sesuaikan dengan syntax berikut
```zone "kanto.a7.com" {
    type master;
    notify yes;
    also-notify { 10.151.73.68; }; IP MOLTRES
    allow-transfer { 10.151.73.68; }; IP MOLTRES
    file "/etc/bind/jarkom/kanto.a7.com";
};
```
![14](https://user-images.githubusercontent.com/45744801/66715674-39d97f80-edf0-11e9-9de4-61c7b1962523.PNG)

b) lalu service bind9 restart

##### II. Konfigurasi Pada Server MOLTRES
langkah-langkah :

a) Buka MOLTRES dan update package lists dengan menjalankan command: apt-get update

b) Setelah melakukan update silahkan install aplikasi bind9 pada MOLTRES dengan perintah: apt-get install bind9 -y

c) Kemudian buka file /etc/bind/named.conf.local pada MEWTWO dan tambahkan syntax berikut:
```
zone "kanto.a7.com" {
    type slave;
    masters { 10.151.73.66; }; // Masukan IP ARTICUNO tanpa tanda petik
    file "/var/lib/bind/kanto.a7.com";
};
```
d) lalu service bind9 restart

##### III. Testing
langkah-langkah :

a) Pada server ARTICUNO silahkan matikan bind9 dengan cara service bind9 stop

b) Pada client PSYDUCK dan snorlax pastikan pengaturan nameserver mengarah ke IP ARTICUNO dan IP MOLTRES
![15](https://user-images.githubusercontent.com/45744801/66715812-f97b0100-edf1-11e9-9fe0-d2a3d9252a18.PNG)
![16](https://user-images.githubusercontent.com/45744801/66715813-fa139780-edf1-11e9-839a-20dfcdfa2299.PNG)

#### 6. selain website utama mereka juga meminta dibuatkan subdomain dengan alamat http://pewter.kanto.yy.com yang didelegasikan pada server MOLTRES dan mengarah ke IP Server MEWTWO.
langkah-langkah :
### I. Konfigurasi Pada Server ARTICUNO

a) lakukan nano /etc/bind/jarkom/kanto.a7.com

b) Tambahkan konfigurasi seperti pada gambar ke dalam file kanto.a7.com
![17](https://user-images.githubusercontent.com/45744801/66715902-e6b4fc00-edf2-11e9-8678-cfa8070f2f67.PNG)

c) Kemudian edit file /etc/bind/named.conf.options pada ARTICUNO dengan nano /etc/bind/named.conf.options

d) Kemudian comment dnssec-validation auto; dan tambahkan baris berikut pada /etc/bind/named.conf.options
```
allow-query{any;};
```
![18](https://user-images.githubusercontent.com/45744801/66715947-5fb45380-edf3-11e9-8ebf-dc45c61061b5.PNG)

e) service bind9 restart

### II. Konfigurasi Pada Server MOLTRES
langkah-langkah :

a) Pada MOLTRES edit file /etc/bind/named.conf.options dengan nano /etc/bind/named.conf.options

b) Kemudian comment dnssec-validation auto; dan tambahkan baris berikut pada /etc/bind/named.conf.options
allow-query{any;};
![19](https://user-images.githubusercontent.com/45744801/66716016-f719a680-edf3-11e9-8956-643ca12f919c.PNG)

c) Lalu edit file /etc/bind/named.conf.local menjadi seperti gambar di bawah:
![20](https://user-images.githubusercontent.com/45744801/66716033-17496580-edf4-11e9-8b9e-dadd79aa8172.PNG)

d) Kemudian buat direktori dengan delegasi dengan mkdir /etc/bind/delegasi

e) Copy db.local ke direktori pucang dan edit namanya menjadi pewter.kanto.a7.com

f) Kemudian edit file pewter.kanto.a7.com menjadi seperti dibawah ini :
![21](https://user-images.githubusercontent.com/45744801/66716099-760edf00-edf4-11e9-8afd-57af0e1fed91.PNG)

g) service bind9 restart
