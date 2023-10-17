# Jarkom-Modul-2-A19-2023

# Kelompok A19:
| Nama | NRP |
| ---------------------- | ---------- |
| Nayya Kamila Putri Y | 5025211183 |
| Javier Nararya Aqsa Setiyono | 5025211245 |

## No 1
Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi dengan pembagian sebagai berikut. Folder topologi dapat diakses pada drive berikut

GNS Dibuat sebagai berikut

//MASUKIN FOTO GNSNYA JAV//

Pembuatan konfigurasi jaringan

- Pandudewanata

```
auto eth0
iface eth0 inet dhcp
auto eth1 iface eth1 inet static address 192.178.1.1 netmask 255.255.255.0
auto eth2 iface eth2 inet static address 192.178.2.1 netmask 255.255.255.0
auto eth3 iface eth3 inet static address 192.178.3.1 netmask 255.255.255.0
```

- DNS Master Yudhistira

```sh
auto eth0
iface eth0 inet static
   address 192.178.1.3
   netmask 255.255.255.0
   gateway 192.178.1.1
```

- DNS Slave Werkudara

```
auto eth0
iface eth0 inet static
	address 192.178.1.2
	netmask 255.255.255.0
	gateway 192.178.1.1
```

- Nakula

```
auto eth0
iface eth0 inet static
	address 192.178.1.2
	netmask 255.255.255.0
	gateway 192.178.1.1
```

- Sadewa

```
auto eth0
iface eth0 inet static
	address 192.178.1.2
	netmask 255.255.255.0
	gateway 192.178.1.1
```

- Abimanyu

```
auto eth0
iface eth0 inet static
	address 192.178.1.2
	netmask 255.255.255.0
	gateway 192.178.1.1
```

- Prabukusuma

```
auto eth0
iface eth0 inet static
	address 192.178.1.2
	netmask 255.255.255.0
	gateway 192.178.1.1
```

- Wisanggeni

```
auto eth0
iface eth0 inet static
	address 192.178.1.2
	netmask 255.255.255.0
	gateway 192.178.1.1
```

- Arjuna

```
auto eth0
iface eth0 inet static
	address 192.178.3.5
	netmask 255.255.255.0
	gateway 192.178.3.1
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

Untuk cek apakah nakula client terhubung ke server arjuna, gunakan `echo nameserver 192.178.3.5 > /etc/resolv.conf` lalu ping

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

Untuk cek apakah Nakula client terhubung ke server Yudhistira, gunakan `echo nameserver 192.178.1.3 > /etc/resolv.conf` lalu ping
