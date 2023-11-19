# Jarkom Modul 3 B13 2023
## Topologi 

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
ab -n 1000 -c 1 -r http://granz.channel.b13.com/
```
- n adalah jumlah request yang dikirimkan
- c adalah jumlah request yang dikirimkan secara bersamaan
- r adalah request yang dikirimkan secara random

Kemudian hasilnya adalah sebagai berikut:

![1000](/assets/1000.png)


