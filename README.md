# Lapres_Modul2_JA07

sebelum mengerjakan soal yang diberikan kita harus terlebih dahulu membuat topologi uml-nya.
### langkah-langkah :
a. membuat file topologi seperti gambar. (gambar 1)

b. bash file topologi yang telah dibuat.
c. login ke semua uml.
d. Hilangkan tanda pagar (#) bagian net.ipv4.ip_forward=1 dengan perintah nano /etc/sysctl.conf pada router pikachu. (gambar 2)
e. jalankan perintah sysctl -p
f. setting IP dari setiap UML dengan dengan perintah nano /etc/network/interfaces.
gambar 3
gambar 4
gambar 5
gambar 6
gambar 7
gambar 8
g. ketikkan perintah service networking restart di setiap uml setelah setting ip.
h. ketikkan perintah atau membuat file script yang berisi iptables –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 192.168.0.0/16.
i. Export proxy pada setiap UML dengan sintaks seperti di bawah ini:
export http_proxy=”http://ITS-564415-ab673:798d2@proxy.its.ac.id:8080”
export https_proxy=”http://ITS-564415-ab673:798d2@proxy.its.ac.id:8080”
export ftp_proxy=”http://ITS-564415-ab673:798d2@proxy.its.ac.id:8080”
j. setelah itu lakukan perintah apt-get update.

## 1. membuat sebuah website utama dengan (1) alamat http://kanto.yy.com
langkah-langkah :
###
