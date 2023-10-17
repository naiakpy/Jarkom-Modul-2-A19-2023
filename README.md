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
