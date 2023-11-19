# Jarkom Modul 3 B13 2023
## Topologi 

- [Jarkom Modul 3 B13 2023](#jarkom-modul-3-b13-2023)
  - [Topologi](#topologi)
  - [Soal No 1](#soal-no-1)
  - [Solusi](#solusi)
      - [Aura (DHCP Relay)](#aura-dhcp-relay)
    - [Switch 1](#switch-1)
      - [Himmel (DHCP Server)](#himmel-dhcp-server)
      - [Heiter (DNS Server)](#heiter-dns-server)
    - [Switch 3](#switch-3)
      - [Danken (Database Server)](#danken-database-server)
      - [Elsen (Load Balancer)](#elsen-load-balancer)
    - [Switch 3](#switch-3-1)
      - [Revolte](#revolte)
      - [Ritcher](#ritcher)
      - [Lawine](#lawine)
      - [Linie](#linie)
      - [Lugner](#lugner)
    - [Switch 4](#switch-4)
      - [Sein](#sein)
      - [Stark](#stark)
      - [Frieren](#frieren)
      - [Flamme](#flamme)
      - [Fern](#fern)
  - [Soal No 2,3,4, dan 5](#soal-no-234-dan-5)
  - [Solusi](#solusi-1)
  - [Soal 6,7,8,9, dan 10](#soal-6789-dan-10)
  - [Solusi](#solusi-2)
    - [Download dan Install PHP](#download-dan-install-php)
    - [Download Nginx](#download-nginx)
    - [Konfigurasi PHP](#konfigurasi-php)
    - [Konfigurasi Nginx](#konfigurasi-nginx)
    - [Setup Load Balancer](#setup-load-balancer)
    - [Testing untuk 1000 request dan 100 request/second](#testing-untuk-1000-request-dan-100-requestsecond)
      - [Testing 1000 request](#testing-1000-request)
  - [Laporan Grinmore 1 (8)](#laporan-grinmore-1-8)
    - [Round Robin](#round-robin)
      - [Testing 200 request dan 10 request/second](#testing-200-request-dan-10-requestsecond)
    - [Least Connection](#least-connection)
      - [Testing 200 request dan 10 request/second](#testing-200-request-dan-10-requestsecond-1)
    - [IP Hash](#ip-hash)
      - [Testing 200 request dan 10 request/second](#testing-200-request-dan-10-requestsecond-2)
    - [Generic Hash](#generic-hash)
      - [Testing 200 request dan 10 request/second](#testing-200-request-dan-10-requestsecond-3)
    - [Grafik](#grafik)
    - [Kesimpulan](#kesimpulan)
    - [Laporan Grinmore 2 (9)](#laporan-grinmore-2-9)
      - [Testing 100 request dan 10 request/second dengan 3 worker](#testing-100-request-dan-10-requestsecond-dengan-3-worker)
      - [Testing 100 request dan 10 request/second dengan 2 worker](#testing-100-request-dan-10-requestsecond-dengan-2-worker)
      - [Testing 100 request dan 10 request/second dengan 1 worker](#testing-100-request-dan-10-requestsecond-dengan-1-worker)
    - [Grafik](#grafik-1)
    - [Kesimpulan](#kesimpulan-1)
  - [Set Authentication pada Nginx (10)](#set-authentication-pada-nginx-10)
  - [Set Proxy Pass to ITS (11)](#set-proxy-pass-to-its-11)
  - [Set Access to Specific IP (12)](#set-access-to-specific-ip-12)
  - [Soal No 11, 12, dan 13](#soal-no-11-12-dan-13)
  - [Solusi](#solusi-3)
    - [Setup Database (13)](#setup-database-13)
    - [Setup Laravel (14)](#setup-laravel-14)
    - [Testing pada Laravel worker (15,16,17)](#testing-pada-laravel-worker-151617)
      - [Testing POST /auth/register (15)](#testing-post-authregister-15)
      - [Testing POST /auth/login (16)](#testing-post-authlogin-16)
      - [Testing GET /me (17)](#testing-get-me-17)
  - [Proxy Bind (18)](#proxy-bind-18)
  - [PHP FPM (19)](#php-fpm-19)
      - [Script FPM 1](#script-fpm-1)
      - [Hasil Testing FPM 1](#hasil-testing-fpm-1)
      - [Script FPM 2](#script-fpm-2)
      - [Hasil Testing FPM 2](#hasil-testing-fpm-2)
      - [Script FPM 3](#script-fpm-3)
      - [Hasil Testing FPM 3](#hasil-testing-fpm-3)
    - [Grafik](#grafik-2)
    - [Kesimpulan](#kesimpulan-2)
  - [Load Balancer Using Least Connection (20)](#load-balancer-using-least-connection-20)


![Topologi](/assets/Topologi.png)

| Node    | Kategori            | Image Docker                    | Konfigurasi IP |   |
|---------|---------------------|---------------------------------|----------------|---|
| Aura    | Router (DHCP Relay) | danielcristh0/debian-buster:1.1 | Dynamic        |   |
| Himmel  | DHCP Server         | danielcristh0/debian-buster:1.1 | Static         |   |
| Heiter  | DNS Server          | danielcristh0/debian-buster:1.1 | Static         |   |
| Denken  | Database Server     | danielcristh0/debian-buster:1.1 | Static         |   |
| Eisen   | Load Balancer       | danielcristh0/debian-buster:1.1 | Static         |   |
| Frieren | Laravel Worker      | danielcristh0/debian-buster:1.1 | Static         |   |
| Flamme  | Laravel Worker      | danielcristh0/debian-buster:1.1 | Static         |   |
| Fern    | Laravel Worker      | danielcristh0/debian-buster:1.1 | Static         |   |
| Lawine  | PHP Worker          | danielcristh0/debian-buster:1.1 | Static         |   |
| Linie   | PHP Worker          | danielcristh0/debian-buster:1.1 | Static         |   |
| Lugner  | PHP Worker          | danielcristh0/debian-buster:1.1 | Static         |   |
| Revolte | Client              | danielcristh0/debian-buster:1.1 | Dynamic        |   |
| Richter | Client              | danielcristh0/debian-buster:1.1 | Dynamic        |   |
| Sein    | Client              | danielcristh0/debian-buster:1.1 | Dynamic        |   |
| Stark   | Client              | danielcristh0/debian-buster:1.1 | Dynamic        |   |

Setelah mengalahkan Demon King, perjalanan berlanjut. Kali ini, kalian diminta untuk melakukan register domain berupa riegel.canyon.yyy.com untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP (0) mengarah pada worker yang memiliki IP 10.15.x.1.

## Soal No 1
Lakukan konfigurasi sesuai dengan peta yang sudah diberikan.

## Solusi
Solusi dari soal diatas adalah kita melakukan konfigurasi untuk ke semua node sesuai dengan table diatas

#### Aura (DHCP Relay)

```
auto eth0
iface eth0 inet dhcp
up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.15.0.0/16


auto eth1
iface eth1 inet static
	address 10.15.1.0
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.15.2.0
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.15.3.0
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 10.15.4.0
	netmask 255.255.255.0
```

### Switch 1

#### Himmel (DHCP Server)
```
auto eth0
iface eth0 inet static
	address 10.15.1.1
	netmask 255.255.255.0
	gateway 10.15.1.0
```

#### Heiter (DNS Server)
```
auto eth0
iface eth0 inet static
	address 10.15.1.2
	netmask 255.255.255.0
	gateway 10.15.1.0
```

### Switch 3

#### Danken (Database Server)
```
auto eth0
iface eth0 inet static
	address 10.15.2.1
	netmask 255.255.255.0
	gateway 10.15.2.0
```

#### Elsen (Load Balancer)
```
auto eth0
iface eth0 inet static
	address 10.15.2.2
	netmask 255.255.255.0
	gateway 10.15.2.0
```

### Switch 3

#### Revolte 
```
auto eth0
iface eth0 inet dhcp
```

#### Ritcher
```
auto eth0
iface eth0 inet dhcp
```

#### Lawine 
```
auto eth0
iface eth0 inet static
	address 10.15.3.1
	netmask 255.255.255.0
	gateway 10.15.3.0
```
#### Linie 
```
auto eth0
iface eth0 inet static
	address 10.15.3.2
	netmask 255.255.255.0
	gateway 10.15.3.0
```
#### Lugner 
```
auto eth0
iface eth0 inet static
	address 10.15.3.3
	netmask 255.255.255.0
	gateway 10.15.3.0
```

### Switch 4
#### Sein 
```
auto eth0
iface eth0 inet dhcp
```

####  Stark 
```
auto eth0
iface eth0 inet dhcp
```
#### Frieren 
```
auto eth0
iface eth0 inet static
	address 10.15.4.1
	netmask 255.255.255.0
	gateway 10.15.4.0
```
#### Flamme 
```
auto eth0
iface eth0 inet static
	address 10.15.4.1
	netmask 255.255.255.0
	gateway 10.15.4.0
```
#### Fern 
```
auto eth0
iface eth0 inet static
	address 10.15.4.1
	netmask 255.255.255.0
	gateway 10.15.4.0
```
Setelah itu kita melakukan registrasi domain yaitu `riegel.canyon.b13.com` dan `granz.channel.b13.com` degan cara melakukan setup pada server Heiter sebagai DNS server.

1. Pertama kita install `bind9` dengan cara menulis syntax berikut:
```
apt-get install bind9 -y
```
2. Kemudian kita tambahkan script berikut pada file `/etc/bind/named.conf.local`
```
zone "canyon.b13.com" {
        type master;
        file "/etc/bind/jarkom/canyon.b13.com";
};

zone "channel.b13.com" {
        type master;
        file "/etc/bind/jarkom/channel.b13.com";
};
```

3. Selanjutnya kita membuat folder `jarkom` pada `/etc/bind` dengan cara menulis syntax berikut:
```
mkdir /etc/bind/jarkom
```

4. Kemudian kita buat file `canyon.b13.com` pada folder `/etc/bind/jarkom` dengan cara menulis syntax berikut:

```
touch /etc/bind/jarkom/canyon.b13.com
```
Setelah itu kita tambahkan script berikut pada file `canyon.b13.com`
```
; ; BIND data file for local loopback interface ; $TTL 604800 

@ IN SOA canyon.b13.com. root.canyon.b13.com. (
                        2023141101      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      canyon.b13.com.
@       IN      A       10.15.1.2 ;ip dns
riegel  IN      A       10.15.2.2 ;ip lawine
www.riegel      IN      CNAME   riegel.canyon.b13.com.
@       IN      AAAA    ::1

```
5. Kemudian kita buat file `channel.b13.com` pada folder `/etc/bind/jarkom` dengan cara menulis syntax berikut:
```
touch /etc/bind/jarkom/channel.b13.com
```
Setelah itu kita tambahkan script berikut pada file `channel.b13.com`
```
; ; BIND data file for local loopback interface ; $TTL 604800 

@ IN SOA channel.b13.com. root.channel.b13.com. (
                        2023111402      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      channel.b13.com.
@       IN      A       10.15.1.2 ; ip dns server
granz   IN      A       10.15.2.2 ; ip frieren
www.granz       IN      CNAME   granz.channel.b13.com.
@       IN      AAAA    ::1
```
6. Kemudian kita restart service `bind9` dengan cara menulis syntax berikut:
```
service bind9 restart
```

## Soal No 2,3,4, dan 5
- Semua CLIENT harus menggunakan konfigurasi dari DHCP Server.
Client yang melalui Switch3 mendapatkan range IP dari
	- 10.15.3.16 - 10.15.3.32 dan 
	- 10.15.3.64 - 10.15.3.80 
- Client yang melalui Switch4 mendapatkan range IP dari 
	- 10.15.4.12 - 10.15.4.20 dan 
	- 10.15.4.160 - 10.15.4.168 
- Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut.
- Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit.



## Solusi
Untuk menyelesaikan solusi diatas kami melakukan setup server terlebih dahulu kedalam server Himmel sebagai DHCP server.

1. Pertama install `isc-dhcp-server` dengan cara menulis syntax berikut: 
```
apt-get install isc-dhcp-server -y
```
2. Setelah terinstall kita tambahkan script berikut pada file `isc-dhcp-server`
```
INTERFACESv4="eth0"
INTERFACESv6=""
```
- `eth0` adalah interface yang digunakan untuk DHCP server 

3. Kemudian kita tambahkan script berikut pada file `/etc/dhcp/dhcpd.conf`
```
option domain-name "example.org";
option domain-name-servers ns1.example.org, ns2.example.org;

default-lease-time 600;
max-lease-time 7200;

ddns-update-style none;

subnet 10.15.1.0 netmask 255.255.255.0 {
}

subnet 10.15.2.0 netmask 255.255.255.0 {
}

subnet 10.15.3.0 netmask 255.255.255.0 {
    range 10.15.3.16 10.15.3.32;
    range 10.15.3.64 10.15.3.80;
    option routers 10.15.3.0;
    option broadcast-address 10.15.3.255;
    option domain-name-servers 10.15.1.2;
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 10.15.4.0 netmask 255.255.255.0 {
    range 10.15.4.12 10.15.4.32;
    range 10.15.4.160 10.15.4.168;
    option routers 10.15.4.0;
    option broadcast-address 10.15.4.255;
    option domain-name-servers 10.15.1.2;
    default-lease-time 720;
    max-lease-time 5760;
}
``` 
- `subnet 10.15.3.0` adalah subnet yang digunakan untuk client yang melalui switch 3 
- `subnet 10.15.4.0` adalah subnet yang digunakan untuk client yang melalui switch 4
- `range` adalah range IP yang digunakan untuk client yang melalui switch 3 dan 4
- `option routers` adalah IP dari router yang digunakan untuk client yang melalui switch 3 dan 4 
- `option broadcast-address` adalah IP broadcast yang digunakan untuk client yang melalui switch 3 dan 4
- `option domain-name-servers` adalah IP dari DNS server yang digunakan untuk client yang melalui switch 3 dan 4
- `default-lease-time` adalah waktu lease default yang digunakan untuk client yang melalui switch 3 dan 4
- `max-lease-time` adalah waktu lease maksimal yang digunakan untuk client yang melalui switch 3 dan 4

4. Kemudian kita restart service `isc-dhcp-server` dengan cara menulis syntax berikut:
```
service isc-dhcp-server restart
```

5. Setelah itu kita setting client agar menggunakan DHCP server dengan cara menulis syntax berikut pada file `/etc/network/interfaces`
```
auto eth0
iface eth0 inet dhcp
```
> Sudah diatur pada soal no 1

6. Setelah itu kita testing dengan cara menuliskan syntax berikut pada client
```
ifconfig
```
![ifconfig](/assets/ifconfig.png)
// Gambar diatas adalah hasil dari node cein yang mendapatkan IP dari DHCP server

## Soal 6,7,8,9, dan 10
- Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3. (6)
- Kepala suku dari Bredt Region memberikan resource server sebagai berikut:
	- Lawine, 4GB, 2vCPU, dan 80 GB SSD.
	- Linie, 2GB, 2vCPU, dan 50 GB SSD.
	- Lugner 1GB, 1vCPU, dan 25 GB SSD.

Aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second. (7)

## Solusi

### Download dan Install PHP
Untuk menyelesaikan soal diatas pertama tama kita melakukan instalasi dulu terhdapap php worker yaitu `lawine`, `linie`, dan `lugner` dengan cara menulis syntax berikut:
```
apt-get update
apt-get install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2
curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg
sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
```

- `apt-get update` adalah syntax untuk melakukan update terhadap server
- `apt-get install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2` adalah syntax untuk menginstall beberapa package yang dibutuhkan untuk menginstall php
- `curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg` adalah syntax untuk mendownload keyring dari php
- `sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'` adalah syntax untuk menambahkan repository php kedalam server 

Setelah itu kita install php dengan cara menulis syntax berikut:
```
apt-get update
apt-get install php8.0-mbstring php8.0-xml php8.0-cli php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y
```

- `apt-get update` adalah syntax untuk melakukan update terhadap server
- `apt-get install php8.0-mbstring php8.0-xml php8.0-cli php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y` adalah syntax untuk menginstall php dan beberapa package yang dibutuhkan untuk php 
- `php8.0-mbstring` adalah package untuk menginstall mbstring pada php 
- `php8.0-xml` adalah package untuk menginstall xml pada php
- `php8.0-cli` adalah package untuk menginstall cli pada php 
- `php8.0-common` adalah package untuk menginstall common pada php
- `php8.0-intl` adalah package untuk menginstall intl pada php
- `php8.0-opcache` adalah package untuk menginstall opcache pada php
- `php8.0-readline` adalah package untuk menginstall readline pada php 
- `php8.0-mysql` adalah package untuk menginstall mysql pada php
- `php8.0-fpm` adalah package untuk menginstall fpm pada php
- `php8.0-curl` adalah package untuk menginstall curl pada php
- `unzip` adalah package untuk menginstall unzip pada server
- `wget` adalah package untuk menginstall wget pada server

### Download Nginx 
Setelah itu kita install nginx sebagai web server dengan cara menulis syntax berikut:
```
apt-get install nginx -y
```

- `apt-get install nginx -y` adalah syntax untuk menginstall nginx pada server 

Setelah itu kita bisa menulis syntax berikut untuk mengecek apakah nginx sudah terinstall atau belum
```
service nginx status
```
![nginx](/assets/nginx.png)

Stelah itu kita download keperluan untuk deploy di server dengan cara menulis syntax berikut:
```
wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1' -O modul-3
```

- `wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1' -O modul-3.zip` adalah syntax untuk mendownload file modul-3 yang dibutuhkan untuk deploy di server 

Setelah itu kita extract file yang sudah didownload dengan cara menulis syntax berikut:
```
unzip modul-3
```

- `unzip modul-3` adalah syntax untuk mengekstrak file modul-3 yang sudah didownload

Setelah itu kita pindahkan file diatas ke dalam `/var/www` dengan cara menulis syntax berikut:
```
mv modul-3 /var/www
```

- `mv modul-3 /var/www` adalah syntax untuk memindahkan file modul-3 ke dalam folder `/var/www`

### Konfigurasi PHP
Setalah kita melakukan instalasi php maka selanjutnya adalah menyalakan php-fpm dengan cara menulis syntax berikut:
```
service php8.0-fpm start
```
- PHP Fpm adalah FastCGI Process Manager yang merupakan implementasi dari FastCGI untuk PHP. FPM berfungsi untuk memproses PHP secara asinkron. FPM akan memproses PHP secara asinkron dengan menggunakan worker. Worker akan memproses PHP secara para atau berkelompok sehingga dapat meningkatkan performa dari PHP.

### Konfigurasi Nginx
Tahap berikutnya adalah melakukan konfigurasi nginx untuk deploy halaman web dengan cara menulis syntax berikut:
```
rm /etc/nginx/sites-available/default
```

kemudian kita ganti dengan file default dengan konfigurasi yang telah kita buat sebagai berikut : 

```
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        root /var/www/modul-3;

        index index.php;

        server_name _;

        location / {
                try_files $uri $uri/ =404;
        }

        # pass PHP scripts to FastCGI server
        #
        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/run/php/php8.0-fpm.sock;
        }


        location ~ /\.ht {
                deny all;
        }
}
```

Setelah itu kita restart nginx dengan cara menulis syntax berikut:
```
service nginx restart
```

- `service nginx restart` adalah syntax untuk merestart nginx

Setelah itu kita testing dengan cara menulis syntax berikut:
```
curl localhost
```

![curl](/assets/localhost.png)

Selesai selanjutnya ulangi langkah diatas untuk menginstall php pada node `linie` dan `lugner`

### Setup Load Balancer
Setelah kita melakukan setup php worker selanjutnya adalah melakukan setup load balancer dengan cara menulis syntax berikut pada node `eisen`:
```
apt-get update && apt-get install nginx -y
```

- `apt-get update && apt-get install nginx -y` adalah syntax untuk menginstall nginx pada server

Setelah itu kita buat file `granz.channel.b13.com` pada folder `/etc/nginx/sites-available` dengan cara menulis syntax berikut:
```
upstream web {
   # no load balancing method is specified for Round Robin
   server 10.15.3.1;
   server 10.15.3.2;
   server 10.15.3.3;
}


server {
        listen 80;
        server_name grenz.channel.b13.com;

        location ^~ /its {
                proxy_pass https://www.its.ac.id;
        }

        location / {

                proxy_pass http://web;
        }
        # Batasan akses berdasarkan alamat IP
        allow 10.15.3.69;
        allow 10.15.3.70;
        allow 10.15.4.167;
        allow 10.15.4.168;
        deny all;
}
```

- `upstream web` adalah script untuk menentukan server yang akan di load balance

Setalah itu kita test dengan mengirimkan request ke `granz.channel.b13.com` dengan cara menulis syntax berikut:

### Testing untuk 1000 request dan 100 request/second
#### Testing 1000 request

Untuk melakukan testing tersebut kita membutuhkan tools yaitu `apache2-utils`, untuk menginstallnya kita bisa menuliskan syntax berikut:
```
apt-get install apache2-utils -y
```

- `apt-get install apache2-utils -y` adalah syntax untuk menginstall apache2-utils pada server

Selannjutnya kita bisa melakukan testing dengan cara menuliskan syntax berikut:
```
ab -n 1000 -c 100 -r http://granz.channel.b13.com/
```
- n adalah jumlah request yang dikirimkan
- c adalah jumlah request yang dikirimkan secara bersamaan
- r adalah request yang dikirimkan secara random

Kemudian hasilnya adalah sebagai berikut:

![1000 request](/assets/1000.png)

Tampilan pada masing masing node adalah sebagai berikut:

![1000 request](/assets/1000request.gif)

## Laporan Grinmore 1 (8)
> buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:

### Round Robin 
Untuk melakukan testing ini kita tidak perlu melakukan konfigurasi apapun karena konfigurasi round robin sudah dilakukan pada soal no 6 dan 7. 

#### Testing 200 request dan 10 request/second
Untuk melakukan testing tersebut kita bisa langsung menuliskan syntax berikut:
```
ab -n 200 -c 10 -r http://granz.channel.b13.com/
```

Hasilnya adalah sebagai berikut:

![Round Robin](/assets/Round%20Robin.png)

Dari gambar diatas bisa kita lihat bahwa hasil dari testing round robin adalah sebagai berikut:
- Time taken for tests : 0.333 seconds
- Complete requests : 200
- Failed requests : 67
- Time per request : 16,33 [ms] (mean)

### Least Connection
Untuk melakukan testing ini kita perlu melakukan konfigurasi pada file `granz.channel.b13.com` dengan cara menulis syntax berikut:
```
upstream web {
   # no load balancing method is specified for Round Robin
   least_conn;
   server
   ...
}
```

#### Testing 200 request dan 10 request/second
Untuk melakukan testing tersebut kita bisa langsung menuliskan syntax berikut:
```
ab -n 200 -c 10 -r http://granz.channel.b13.com/
```

Hasilnya adalah sebagai berikut:

![Least Connection](/assets/least_conn.png)


Dari gambar diatas bisa kita lihat bahwa hasil dari testing least connection adalah sebagai berikut:

- Time taken for tests : 0.288 seconds
- Complete requests : 200
- Failed requests : 60
- Time per request : 14,39 [ms] (mean)

### IP Hash
Untuk melakukan testing ini kita perlu melakukan konfigurasi pada file `granz.channel.b13.com` dengan cara menulis syntax berikut:
```
upstream web {
   # no load balancing method is specified for Round Robin
   ip_hash;
   server
   ...
}
```

#### Testing 200 request dan 10 request/second
Untuk melakukan testing tersebut kita bisa langsung menuliskan syntax berikut:
```
ab -n 200 -c 10 -r http://granz.channel.b13.com/
```

Hasilnya adalah sebagai berikut:

![IP Hash](/assets/ip_hash.png)

Dari gambar diatas bisa kita lihat bahwa hasil dari testing IP Hash adalah sebagai berikut:

- Time taken for tests : 0.257 seconds
- Complete requests : 200
- Failed requests : 0
- Time per request : 12,835 [ms] (mean)

### Generic Hash

Untuk melakukan testing ini kita perlu melakukan konfigurasi pada file `granz.channel.b13.com` dengan cara menulis syntax berikut:
```
upstream web {
   # no load balancing method is specified for Round Robin
   hash $remote_addr consistent;
   server
   ...
}
```

#### Testing 200 request dan 10 request/second
Untuk melakukan testing tersebut kita bisa langsung menuliskan syntax berikut:
```
ab -n 200 -c 10 -r http://granz.channel.b13.com/
```

Hasilnya adalah sebagai berikut:

![Generic Hash](/assets/generic_hash.png)

Dari gambar diatas bisa kita lihat bahwa hasil dari testing Generic Hash adalah sebagai berikut:

- Time taken for tests : 0.257 seconds
- Complete requests : 200
- Failed requests : 0
- Time per request : 12,835 [ms] (mean)

### Grafik
![Grafik](/assets/hasil_testing.png)


### Kesimpulan 
Dari hasil testing diatas bisa kita bisa simpulkan bahwa untuk load balancer yang paling baik adalah IP Hash dan Generic Hash karena memiliki nilai time per request yang paling kecil. 

Hal tersebut dikarenakan IP Hash dan Generic Hash menggunakan algoritma hash untuk melakukan load balancing. Algoritma hash akan melakukan hashing terhadap IP address dari client yang melakukan request. Sehingga setiap client akan mendapatkan server yang sama selama server tersebut masih aktif. 

Sedangkan untuk algoritma round robin dan least connection tidak menggunakan algoritma hash sehingga tidak bisa menjamin bahwa setiap client akan mendapatkan server yang sama selama server tersebut masih aktif.



### Laporan Grinmore 2 (9)
> Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire. (9)

Untuk melakukan testing ini kita perlu mengembalikan konfigurasi pada file `granz.channel.b13.com` seperti semula yaitu dengan menggunakan algoritma round robin.

Setelah itu kita bisa melakukan testing.

#### Testing 100 request dan 10 request/second dengan 3 worker
Untuk melakukan testing tersebut kita bisa langsung menuliskan syntax berikut:
```
ab -n 100 -c 10 -r http://granz.channel.b13.com/
```

Hasilnya adalah sebagai berikut:

![3 worker](/assets/3Worker.png)

Dari gambar diatas bisa kita lihat bahwa hasil dari testing 3 worker adalah sebagai berikut:

- Time taken for tests : 0.137 seconds
- Complete requests : 100
- Failed requests : 33
- Time per request : 15,2 [ms] (mean)

#### Testing 100 request dan 10 request/second dengan 2 worker

Untuk melakukan testing tersebut kita bisa langsung mematikan salah satu worker, dalam case ini kita mematikan worker dengan IP `10.15.3.1`.

Setelah itu kita bisa langsung menuliskan syntax berikut:
```
ab -n 100 -c 10 -r http://granz.channel.b13.com/
```

Hasilnya adalah sebagai berikut:

![2 worker](/assets/2Worker.png)

Dari gambar diatas bisa kita lihat bahwa hasil dari testing 2 worker adalah sebagai berikut:

- Time taken for tests : 0.125 seconds
- Complete requests : 100
- Failed requests : 50
- Time per request : 12,5 [ms] (mean)

#### Testing 100 request dan 10 request/second dengan 1 worker

Untuk melakukan testing tersebut kita bisa langsung mematikan salah satu worker, dalam case ini kita mematikan worker dengan IP `10.15.3.2`.

Setelah itu kita bisa langsung menuliskan syntax berikut:
```
ab -n 100 -c 10 -r http://granz.channel.b13.com/
```

Hasilnya adalah sebagai berikut:

![1 worker](/assets/1Worker.png)

Dari gambar diatas bisa kita lihat bahwa hasil dari testing 1 worker adalah sebagai berikut:

- Time taken for tests : 0.115 seconds
- Complete requests : 100
- Failed requests : 0
- Time per request : 11,5 [ms] (mean)

### Grafik

![Grafik](/assets/hasil_testing_worker.png)

### Kesimpulan

Berdasarkan data load balancer round robin yang Anda berikan, berikut adalah beberapa kesimpulan yang dapat ditarik:

- Secara keseluruhan, load balancer berkinerja baik. Ketiga konfigurasi pekerja dapat menyelesaikan 100 permintaan, dan waktu rata-rata per permintaan relatif rendah.
- Menggunakan satu pekerja adalah konfigurasi yang paling efisien. Waktu rata-rata per permintaan adalah 11,5 ms, dan tidak ada permintaan yang gagal. Namun, konfigurasi ini juga yang paling tidak dapat diskalakan, karena tidak akan dapat menangani peningkatan lalu lintas yang besar.
- Menggunakan dua pekerja adalah kompromi yang baik antara efisiensi dan skalabilitas. Waktu rata-rata per permintaan adalah 12,5 ms, dan hanya ada 50 permintaan yang gagal. Konfigurasi ini akan dapat menangani peningkatan lalu lintas moderat.
- Menggunakan tiga pekerja adalah konfigurasi yang paling dapat diskalakan. Waktu rata-rata per permintaan adalah 15,2 ms, dan ada 33 permintaan yang gagal. Konfigurasi ini akan dapat menangani peningkatan lalu lintas yang besar, tetapi juga yang paling tidak efisien.

Berdasarkan kesimpulan tersebut, kita merekomendasikan penggunaan dua pekerja untuk sebagian besar lingkungan produksi. Konfigurasi ini memberikan keseimbangan yang baik antara efisiensi dan skalabilitas. Jika kita mengharapkan peningkatan lalu lintas yang besar, kita mungkin perlu mempertimbangkan untuk menggunakan tiga pekerja. Namun, ini akan mengurangi efisiensi.

Penjelasan Lebih Detail

- Efisiensi mengacu pada jumlah pekerjaan yang dapat diselesaikan oleh load balancer dalam satuan waktu tertentu. Dalam hal ini, efisiensi diukur dengan waktu rata-rata per permintaan.
- Skalabilitas mengacu pada kemampuan load balancer untuk menangani peningkatan lalu lintas. Dalam hal ini, skalabilitas diukur dengan jumlah permintaan yang dapat diselesaikan load balancer sebelum terjadi kegagalan.

Berdasarkan penjelasan di atas, konfigurasi dengan satu pekerja adalah yang paling efisien. Namun, konfigurasi ini tidak dapat menangani peningkatan lalu lintas yang besar. Konfigurasi dengan dua pekerja adalah kompromi yang baik antara efisiensi dan skalabilitas. Konfigurasi ini dapat menangani peningkatan lalu lintas moderat. Konfigurasi dengan tiga pekerja adalah yang paling dapat diskalakan, tetapi juga yang paling tidak efisien.

## Set Authentication pada Nginx (10)
> Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/ (10)

Untuk melakukan konfigurasi autentikasi pada nginx kita perlu menginstall package `apache2-utils` dengan cara menulis syntax berikut:
```
apt-get install apache2-utils -y
```

Selanjutnya kita bisa membuat file `htpasswd` dengan cara menulis syntax berikut:
```
htpasswd -b -c /etc/nginx/rahasisakita/htpasswd netics ajkb13
```

- `htpasswd -b -c /etc/nginx/rahasisakita/htpasswd netics ajkb13` adalah syntax untuk membuat file htpasswd dengan username `netics` dan password `ajkb13`

Setelah itu kita bisa melakukan konfigurasi pada file `granz.channel.b13.com` dengan cara menulis syntax berikut:

```
...
location / {
        auth_basic "Restricted Access";
        auth_basic_user_file /etc/nginx/rahasisakita/htpasswd;

        proxy_pass http://web;
}
...
```

Setelah itu kita bisa melakukan testing dengan cara menulis syntax berikut:
```
curl -u netics:ajkb13 http://granz.channel.b13.com/
```

- `curl -u netics:ajkb13 http://granz.channel.b13.com/` adalah syntax untuk melakukan testing dengan menggunakan username `netics` dan password `ajkb13`

Hasilnya adalah sebagai berikut:

![auth](/assets/test_password.png)
// Gambar diatas adalah hasil dari testing dengan menggunakan username `netics` dan password `ajkb13` dan yang tidak menggunakan username dan password

## Set Proxy Pass to ITS (11)
> Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id. (11) hint: (proxy_pass)

Untuk melakukan konfigurasi proxy pass kita bisa menulis syntax berikut pada file `granz.channel.b13.com`:
```
...
location ~* /its{
        proxy_pass https://www.its.ac.id;
}
...
```

Setelah itu kita bisa melakukan testing dengan cara menulis syntax berikut:
```
curl http://granz.channel.b13.com/its
```
atau 
```
curl http://granz.channel.b13.com/halo/its
```

Hasilnya adalah sebagai berikut:
![its](/assets/its1.png)


## Set Access to Specific IP (12)
> Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168. (12) hint: (fixed in dulu clinetnya)

Untuk melakukan konfigurasi tersebut kita bisa menulis syntax berikut pada file `granz.channel.b13.com`:
```
# Kode sebelumnya
...
# Batasan akses berdasarkan alamat IP
allow 10.15.3.69;
allow 10.15.3.70;
allow 10.15.4.167;
allow 10.15.4.168;
deny all;
```

Setelah itu kita bisa melakukan testing dengan cara menulis syntax berikut:
```
curl http://granz.channel.b13.com/
```

Hasilnya adalah sebagai berikut:
![ip](/assets/testing_ip.png)

Apabila kita melakukan testing dengan IP yang sesuai dengan yang diizinkan akan mendapatkan hasil seperti berikut : 

![ip](/assets/ip_testing2.png)

Jika kita perhatikan pada gambar diatas, kita bisa melihat bahwa IP yang digunakan oleh Richter mempunyai pesan error 401. Hal ini dikarenakan IP yang digunakan oleh Richter belum melakukan authentikasi sehingga tidak bisa mengakses halaman web.

Sedangkan untuk IP yang digunakan oleh Revolte mendapatkan kode 403 yang artinya IP tersebut tidak diizinkan untuk mengakses halaman web. 


## Soal No 11, 12, dan 13
Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern. (13)

Frieren, Flamme, dan Fern memiliki Riegel Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer (14)

Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.
POST /auth/register (15)
POST /auth/login (16)
GET /me (17)

## Solusi
### Setup Database (13)
Untuk menyelesaikan soal diatas kita perlu melakukan setup database terlebih dahulu.

1. Pertama kita masuk kedalam server `denken` dan selanjutnya kita install mysql dengan cara menulis syntax berikut:
```
apt-get update
apt-get install mariadb-server -y
service mysql start
```
- `apt-get update` adalah syntax untuk melakukan update terhadap server
- `apt-get install mariadb-server -y` adalah syntax untuk menginstall mysql pada server
- `service mysql start` adalah syntax untuk menyalakan mysql

2. Setelah itu kita bisa melakukan konfigurasi mysql dengan cara menulis syntax berikut:
```
mysql << EOF

CREATE USER 'kelompokb13'@'10.15.4.%' IDENTIFIED BY 'passwordb13';
CREATE DATABASE db_kelompokb13;
GRANT ALL PRIVILEGES ON db_kelompokb13.* TO 'kelompokb13'@'10.15.4.%';
FLUSH PRIVILEGES;

EOF
```

- `mysql << EOF` adalah syntax untuk melakukan konfigurasi mysql
- `CREATE USER 'kelompokb13'@'10.15.4.%' IDENTIFIED BY 'passwordb13';` adalah syntax untuk membuat user `kelompokb13` dengan password `passwordb13` yang bisa diakses dari IP `10.15.4.%`
- `CREATE DATABASE db_kelompokb13;` adalah syntax untuk membuat database `db_kelompokb13`

3. Setelah itu kita bisa melakukan restart mysql dengan cara menulis syntax berikut:
```
service mysql restart
```

### Setup Laravel (14)

Instalasi database sudah selesai selanjutnya kita bisa melakukan setup laravel pada 3 worker laravel tersebut dengan cara menulis syntax berikut:
```bash
apt-get update && apt-get install git -y
apt-get install lynx -y 
apt-get install nginx  -y
```

- `apt-get update && apt-get install git -y` adalah syntax untuk melakukan update dan menginstall git pada server
- `apt-get install lynx -y` adalah syntax untuk menginstall lynx pada server
- `apt-get install nginx  -y` adalah syntax untuk menginstall nginx pada server

Setelah itu kita bisa melakukan clone repository laravel dengan cara menulis syntax berikut:
```bash
git clone https://github.com/martuafernando/laravel-praktikum-jarkom.git
```     

Setelah itu kita bisa melakukan install composer dengan cara menulis syntax berikut:
```bash
apt-get update

apt-get install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2

curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg

sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'


apt-get update
apt-get install php8.0-mbstring php8.0-xml php8.0-cli php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y



wget https://getcomposer.org/download/2.0.13/composer.phar

chmod +x composer.phar

mv composer.phar /usr/bin/composer
```

- `apt-get update` adalah syntax untuk melakukan update terhadap server
- `apt-get install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2` adalah syntax untuk menginstall beberapa package yang dibutuhkan untuk menginstall php 
- `curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg` adalah syntax untuk mendownload keyring dari php
- `sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'` adalah syntax untuk menambahkan repository php kedalam server
- `apt-get update` adalah syntax untuk melakukan update terhadap server
- `apt-get install php8.0-mbstring php8.0-xml php8.0-cli php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y` adalah syntax untuk menginstall php dan beberapa package yang dibutuhkan untuk php

- `wget https://getcomposer.org/download/2.0.13/composer.phar` adalah syntax untuk mendownload composer 
- `chmod +x composer.phar` adalah syntax untuk mengubah permission dari composer
- `mv composer.phar /usr/bin/composer` adalah syntax untuk memindahkan composer ke dalam folder `/usr/bin`

Setelah itu kita bisa melakukan install laravel dengan cara menulis syntax berikut:
```
cd laravel-praktikum-jarkom
composer update 
composer install
```

- `cd laravel-praktikum-jarkom` adalah syntax untuk masuk kedalam folder `laravel-praktikum-jarkom`
- `composer update` adalah syntax untuk melakukan update terhadap composer
- `composer install` adalah syntax untuk menginstall composer

Setelah itu kita bisa melakukan konfigurasi laravel dengan cara menulis syntax berikut:
```bash
cp .env.example .env
php artisan key:generate
php artisan jwt:secret
```
- `cp .env.example .env` adalah syntax untuk mengcopy file `.env.example` menjadi `.env`
- php artisan key:generate` adalah syntax untuk membuat key pada laravel 
- php artisan jwt:secret` adalah syntax untuk membuat secret key pada laravel 

Setelah itu kita bisa melakukan konfigurasi nginx dengan cara menulis syntax berikut:
```
rm /etc/nginx/sites-available/default
```

dan ganti dengan file default dengan konfigurasi yang telah kita buat sebagai berikut : 

```bash
server {
    listen 80;
    listen [::]:80;
    server_name granz.channel.b13.com;
    root /var/www/laravel-praktikum-jarkom/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";

    index index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }

    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;

    }

    location ~ /\.(?!well-known).* {
        deny all;
    }

}
```

Apabila kita perhatikan pada konfigurasi diatas kita menggunakan `fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;` untuk melakukan proxy pass ke php-fpm. 

Jika proses diatas sudah selesai kita bisa melakukan restart nginx dengan cara menulis syntax berikut:
```
service nginx restart
```

Setelah itu kita bisa melakukan testing dengan cara menulis syntax berikut:
```
lynx localhost 
```
pada node `frieren`, `flamme`, dan `fern`

Hasilnya adalah sebagai berikut:

![testing](/assets/laravel_worker.png)

### Testing pada Laravel worker (15,16,17)
> Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.

#### Testing POST /auth/register (15)
Untuk melakukan testing tersebut kita bisa langsung menuliskan syntax berikut:
```
ab -n 100 -c 10 -r -p register.json -T application/json localhost/api/auth/register
```
dengan isi dari file `register.json` adalah sebagai berikut:
```
{
    "username": "kelompokb13",
    "password": "passwordb13"
}
```
Maka hasilnya adalah sebagai berikut:

![register](/assets/register.png)

Apabila kita perhatikan pada gambar diatas kita bisa melihat bahwa hasil dari testing register adalah sebagai berikut:
- Time taken for tests : 1.112 seconds
- Complete requests : 100
- Failed requests : 99
- Time per request : 111,2 [ms] (mean)

> Failed requests : 99 terjadi karena kita melakukan testing dengan menggunakan username dan password yang sama sehingga tidak bisa melakukan register lebih dari 1 kali


#### Testing POST /auth/login (16)

Untuk melakukan testing tersebut kita bisa langsung menuliskan syntax berikut:
```
ab -n 100 -c 10 -r -p login.json -T application/json http://localhost/api/auth/login
```

dengan file login.json berisikan sebagai berikut : 
```
{
    "username": "kelompokb13",
    "password": "passwordb13"
}
```

Kita bisa test dulu script tersebut dengan cara menuliskan syntax berikut : 
```
curl -X POST -H "Content-Type: application/json" -d @login.json http://riegel.canyon.b13.com/api/auth/login
```
Apabila sudah berhasil maka akan muncul hasil seperti berikut :
```
{"token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vMTAuMTUuNC4yL2FwaS9hdXRoL2xvZ2luIiwiaWF0IjoxNzAwNDIyOTEwLCJleHAiOjE3MDA0MjY1MTAsIm5iZiI6MTcwMDQyMjkxMCwianRpIjoiUjRrRUJsNXY4TXRmcE9sUiIsInN1YiI6IjEiLCJwcnYiOiIyM2JkNWM4OTQ5ZjYwMGFkYjM5ZTcwMWM0MDA4NzJkYjdhNTk3NmY3In0.2wHVHTIRvV9vHbUnLTHsuB35HLjC70Rc7NsWX7FNsqw"}
```

Artinya kita sudah berhasil melakukan login dan mendapatkan token. Waktunya kita melakukan testing dengan menggunakan apache benchmark.

Maka hasilnya adalah sebagai berikut:

![login](/assets/login.png)

Apabila kita perhatikan pada gambar diatas kita bisa melihat bahwa hasil dari testing login adalah sebagai berikut:
- Time taken for tests : 4.062 seconds
- Complete requests : 100
- Failed requests : 40
- Time per request : 406,6 [ms] (mean)

#### Testing GET /me (17)

Untuk melakukan testing tersebut kita perlu melakukan login terlebih dahulu dengan cara menulis syntax berikut:
```
curl -X POST -H "Content-Type: application/json" -d @login.json http://localhost/api/auth/login
```

Selanjutnya kita bisa gunakan token yang didapatkan untuk melakukan testing dengan cara menulis syntax berikut:

```
TOKEN=$(jq -r '.token' token.json)
```
Token yang didapatkan akan disimpan kedalam variable `TOKEN`

```
ab -n 100 -c 10 -H "Authorization: Bearer $TOKEN" http://localhost/api/me
```

Maka hasilnya adalah sebagai berikut:

![me](/assets/me.png)

Apabila kita perhatikan pada gambar diatas kita bisa melihat bahwa hasil dari testing me adalah sebagai berikut:
- Time taken for tests : 2.065 seconds
- Complete requests : 100
- Failed requests : 0
- Time per request : 206,1 [ms] (mean)

## Proxy Bind (18)
> Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Riegel Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern. (18)

Untuk melakukan konfigurasi tersebut kita bisa menulis syntax berikut pada file `riegal.channel.b13.com`:
```
upstream backend {
        server 10.15.4.1;

        server 10.15.4.2;

        server 10.15.4.3;
}

server {
        listen 80;
        server_name riegel.canyon.b13.com;

        location / {
                proxy_pass http://backend;
        }
}
```

Setelah itu kita bisa melakukan testing dengan cara menulis syntax berikut:
```
curl http://riegel.channel.b13.com/
```

Maka akan diperoleh pembagian yang adil 

## PHP FPM (19)
> Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan 
- pm.max_children
- pm.start_servers
- pm.min_spare_servers
- pm.max_spare_servers

> Pada Frieren, Flamme, dan Fern. (19)

Untuk melakukan konfigurasi tersebut kita bisa menulis syntax berikut : 

#### Script FPM 1
```
echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

pm = dynamic
pm.max_children = 10
pm.start_servers = 3
pm.min_spare_servers = 2
pm.max_spare_servers = 5' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart
```
#### Hasil Testing FPM 1
![fpm1](/assets/fpm-1.png)

#### Script FPM 2
```
echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off


pm = dynamic
pm.max_children = 20
pm.start_servers = 12
pm.min_spare_servers = 8
pm.max_spare_servers = 15' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart
```

#### Hasil Testing FPM 2

![fpm2](/assets/fpm-2.png)

#### Script FPM 3
```
echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off


pm = dynamic
pm.max_children = 40
pm.start_servers = 24
pm.min_spare_servers = 16
pm.max_spare_servers = 30' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart
```

#### Hasil Testing FPM 3
![fpm3](/assets/fpm-3.png)

### Grafik
![grafik](/assets/fpm-time.png)
![grafik](/assets/fpm-tpr.png)

### Kesimpulan
Berdasarkan hasil testing diatas kita bisa simpulkan bahwa semakin besar nilai maka semakin baik performa dari worker.

Hal tersebut dikarenakan semakin besar nilai dari `pm.max_children` maka semakin banyak request yang bisa ditangani oleh worker. 


## Load Balancer Using Least Connection (20)
> Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second. (20)

Untuk melakukan konfigurasi tersebut kita bisa menulis syntax berikut pada file `riegel.channel.b13.com`:
```
upstream backend {
        least_conn;
        server
        ...
}
```

Setelah itu kita bisa melakukan testing dengan cara menulis syntax berikut:
``` bash
ab -n 100 -c 10 -H "Authorization: Bearer $TOKEN" http://riegel.canyon.b13.com/api/me
```

Maka hasilnya adalah sebagai berikut:
![least](/assets/least_conn_20.png)


Apabila kita perhatikan pada gambar diatas kita bisa melihat bahwa hasil dari testing least connection adalah sebagai berikut:
- Time taken for tests : 1.098 seconds
- Time per request : 109,8 [ms] (mean)
- Complete requests : 100
- Failed requests : 12

Kesimpulan :
Kita bisa melihat bahwa hasil dari testing least connection memberikan hasil yang lebih baik dibandingkan dengan menggunakan round robin. Hal tersebut dikarenakan least connection akan mengarahkan request ke server yang memiliki jumlah koneksi yang paling sedikit. Sehingga server tersebut akan memiliki beban yang lebih ringan dibandingkan dengan server yang lainnya.

