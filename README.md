# Jarkom-Modul-2-A19-2023

# Kelompok A19:
| Nama | NRP |
| ---------------------- | ---------- |
| Nayya Kamila Putri Y | 5025211183 |
| Javier Nararya Aqsa Setiyono | 5025211245 |

## No 1
Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi dengan pembagian sebagai berikut. Folder topologi dapat diakses pada drive berikut

GNS Dibuat sebagai berikut

//FOTO GNS//

Pembuatan konfigurasi jaringan

- Pandudewanata

```
auto eth0
iface eth0 inet dhcp
auto eth1
iface eth1 inet static
	address 192.178.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.178.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.178.3.1
	netmask 255.255.255.0
```

- DNS Master Yudhistira

```sh
auto eth0
iface eth0 inet static
	address 192.178.2.2
	netmask 255.255.255.0
	gateway 10.56.2.1
```

- DNS Slave Werkudara

```
auto eth0
iface eth0 inet static
	address 192.178.2.3
	netmask 255.255.255.0
	gateway 10.56.2.1
```

- Nakula

```
auto eth0
iface eth0 inet static
	address 192.178.1.2
	netmask 255.255.255.0
	gateway 10.56.1.1
```

- Sadewa

```
auto eth0
iface eth0 inet static
	address 192.178.1.3
	netmask 255.255.255.0
	gateway 10.56.1.1
```

- Abimanyu

```
auto eth0
iface eth0 inet static
	address 192.178.3.3
	netmask 255.255.255.0
	gateway 10.56.3.1
```

- Prabukusuma

```
auto eth0
iface eth0 inet static
	address 192.178.3.2
	netmask 255.255.255.0
	gateway 10.56.3.1
```

- Wisanggeni

```
auto eth0
iface eth0 inet static
	address 192.178.3.4
	netmask 255.255.255.0
	gateway 10.56.3.1
```

## No 2
Buatlah website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok.

Maka kami akan membuat website www.arjuna.a19.com

Masuk ke node arjuna, lalu buka `nano /etc/bind/named.conf.local`

Masukkan
```
zone "arjuna.a19.com" {
	type master;
	file "/etc/bind/arjuna/arjuna.a19.com";
};
```

Buat direktori bernama `arjuna`, lakukan cp db.lokal, lalu setting. Ketik `service bind9 restart`

Lakukan ping

## No 3
Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com

Buka node Yudhistira, lalu buka `nano /etc/bind/named.conf.local`

Masukkan
```
zone "abimanyu.a19.com" {
	type master;
	file "/etc/bind/abimanyu/abimanyu.a19.com";
};
```

Buat direktori bernama `abimanyu`, lakukan cp db.lokal. Masuk kedalam file abimanyu.a19.com dan setting

Lakukan ping

## No 4
Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

Pada Yudhistira, edit file `/etc/bind/jarkom/abimanyu.a19.com` lalu tambahkan subdomain parikesit.abimanyu.a19.com yang mengarah ke IP abimanyu
```
nano /etc/bind/jarkom/abimanyu.a19.com
```

Setelah ditambahkan, lalu ketik `service bind9 restart`

Lakukan ping

## No 5
Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)

Edit `/etc/bind/named.conf.local` pada Yudhistira
```
nano /etc/bind/named.conf.local
```

Tambahkan konfigurasi dalam file tersebut dengan reverse 3 byte awal dari IP Abimanyu, lalu copykan file ptr ke dalam folder jarkom dan ubah namanya menjadi `3.178.192.in-addr.arpa`
```
cp /root/abimanyu.a19.ptr /etc/bind/jarkom/3.178.192.in-addr.arpa
```

Untuk mencek masuk ke nakula client, hubungkan ke server pusat

## No 6
Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

Lakukan konfigurasi pada server Yudhistira dalam file `/etc/bind/named.conf.local` hingga menjadi seperti di bawah ini:
```
zone "arjuna.a19.com" {
      type master;
      file "/etc/bind/jarkom/arjuna.a19.com";
};

zone "abimanyu.a19.com" {
      type master;
      notify yes;
      also-notify { 192.178.3.3; };
      allow-transfer { 192.178.3.3; };
      file "/etc/bind/jarkom/abimanyu.a19.com";
};

zone "1.178.192.in-addr.arpa" {
	type master;
  	file "/etc/bind/jarkom/1.178.192.in-addr.arpa";
};
```

Restart service bind
```
service bind9 restart
```

Lakukan konfigurasi pada Werkudara dengan update package list dan install aplikasi bind9 dengan command berikut:
```
 apt-get update
 apt-get install bind9 -y
```

Kemudian, buka file `/etc/bind/named.conf.local` pada werkudara dan tambahkan perintah seperti di bawah ini:
```
zone "abimanyu.a19.com" {
  type slave;
  masters {192.178.2.2; }; 
  file "/var/lib/bind/abimanyu.a19.com";
};
```

Restart service bind
```
service bind9 restart
```

## No 7
Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.

Masuk node yudhistira
```
/etc/bind/abimanyu/abimanyu.a19.com
```

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.a19.com. root.abimanyu.a19.com. (
                   2022101001         ; Serial
                       604800         ; Refresh
                        86400         ; Retry
                      2419200         ; Expire
                       604800 )       ; Negative Cache TTL
;
@               IN      NS      abimanyu.a19.com.
@               IN      A       192.178.3.3   
www             IN      CNAME   abimanyu.a19.com.
parikesit       IN      A       192.178.3.3
www.parikesit   IN      CNAME   parikesit.abimanyu.a19.com.
nsl             IN      A       192.178.3.3
baratayuda      IN      NS      nsl
@               IN      AAAA    ::1
```

Pada Werkudara, lakukan konfigurasi dengan menambahkan perintah berikut pada `named.conf.local`
```
zone "baratayuda.abimanyu.a19.com" {
  type master;
  file "/etc/bind/baratayuda/baratayuda.abimanyu.a19.com";
};
```

Buat folder `baratayuda` pada Werkudara, lalu edit file `/etc/bind/baratayuda/baratayuda.abimanyu.a19.com` seperti di bawah ini
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     baratayuda.abimanyu.a19.com. root.baratayuda.abimanyu.a19.com. (
                   2022101001         ; Serial
                       604800         ; Refresh
                        86400         ; Retry
                      2419200         ; Expire
                       604800 )       ; Negative Cache TTL
;
@       IN      NS      baratayuda.abimanyu.a19.com.
@       IN      A       192.178.3.3
www     IN      CNAME   baratayuda.abimanyu.a19.com.
@       IN      AAAA    ::1
```

## No 8
Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.

Buka node werkudara, lalu setting seperti ini
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     baratayuda.abimanyu.a19.com. root.baratayuda.abimanyu.a19.com. (
                   2022101001         ; Serial
                       604800         ; Refresh
                        86400         ; Retry
                      2419200         ; Expire
                       604800 )       ; Negative Cache TTL
;
@       IN      NS      baratayuda.abimanyu.a19.com.
@       IN      A       192.178.3.3 
www     IN      CNAME   baratayuda.abimanyu.a19.com.
rjp     IN      A       192.178.3.3
www.rjp IN      CNAME   rjp.baratayuda.abimanyu.a19.com.
@       IN      AAAA    ::1
```

## No 9
Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.

Install Nginx pada Arjuna dengan command berikut:
```
apt-get update
apt-get install nginx -y
```

Start Nginx
```
service nginx start
```

Buat file `nginx-lb-block` yang berisi
```
upstream myweb  {
      server 192.178.1.4; 
      server 192.178.1.5; 
      server 192.178.1.6;
}

server {
      listen 80;
      server_name arjuna.a19.com;

      location / {
      proxy_pass http://myweb;
      }
}
```

Lakukan konfigurasi pada nginx dengan meng-copy file `/root/nginx-lb-bloc`k ke file default dalam sites-available dengan perintah
```
cp /root/nginx-lb-block /etc/nginx/sites-available/default
```

Lalu buat symlink (link simbolik dari file konfigurasi Nginx yang berada di direktori `/etc/nginx/sites-available` ke direktori `/etc/nginx/sites-enabled`
```
ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled
```

Restart Nginx
```
service nginx restart
```

## No 10
Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh: (Prabakusuma:8001, Abimanyu:8002, Wisanggeni:8003)

Lakukan konfigurasi pada setiap worker Prabukusuma. Buat file ```nginx-block``` pada Prabukusuma dengan nano nginx-block dan edit file seperti di bawah ini:
```
server {

      listen 8001;

      root /var/www/arjuna.a19;

      index index.php index.html index.htm;
      server_name _;

      location / {
                      try_files $uri $uri/ /index.php?$query_string;
      }

      # pass PHP scripts to FastCGI server
      location ~ \.php$ {
      include snippets/fastcgi-php.conf;
      fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
      }

location ~ /\.ht {
                      deny all;
      }

      error_log /var/log/nginx/default_error.log;
      access_log /var/log/nginx/default_access.log;
}
```

Copy ke file default Nginx
```
cp /root/nginx-block /etc/nginx/sites-available/default
```

Lakukan symlink
```
ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled
```

Restart Nginx
```
service nginx restart
```

Periksa apakah bekerja
```
nginx -t
```

Lakukan hal yang sama untuk worker Wisanggeni dan Abimanyu Abimanyu File `arjuna-nginx-block`
```
server {

      listen 8002;

      root /var/www/arjuna.a19;

      index index.php index.html index.htm;
      server_name _;

      location / {
                      try_files $uri $uri/ /index.php?$query_string;
      }

      location ~ \.php$ {
      include snippets/fastcgi-php.conf;
      fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
      }

location ~ /\.ht {
                      deny all;
      }

      error_log /var/log/nginx/default_error.log;
      access_log /var/log/nginx/default_access.log;
}
```

Wisanggeni File `nginx-block`
```
server {

      listen 8003;

      root /var/www/arjuna.a19;

      index index.php index.html index.htm;
      server_name _;

      location / {
                      try_files $uri $uri/ /index.php?$query_string;
      }

      # pass PHP scripts to FastCGI server
      location ~ \.php$ {
      include snippets/fastcgi-php.conf;
      fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
      }

location ~ /\.ht {
                      deny all;
      }

      error_log /var/log/nginx/default_error.log;
      access_log /var/log/nginx/default_access.log;
}
```

Konfigurasi file `nginx-lb-block` pada Arjuna
```
upstream myweb  {
      server 192.178.1.4:8002; 
      server 192.178.1.5:8001; 
      server 192.178.1.6:8003; 
}

server {
      listen 80;
      server_name arjuna.a19.com;

      location / {
      proxy_pass http://myweb;
      }
}
```
