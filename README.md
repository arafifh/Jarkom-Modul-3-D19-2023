# Jarkom-Modul-3-D19-2023

## Anggota Kelompok
| Nama | NRP |
|---------------------------|------------|
|Adrian Ismu Arifianto | 5025211116 |
|Ahmad Rafif Hikmatiar | 5025211247 |

## Topologi
![image](https://github.com/arafifh/Jarkom-Modul-3-D19-2023/assets/71255346/3e53c37e-3c90-45ac-b1e0-984da0b06a9a)

## Config 
- **Aura**
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.31.1.200
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.31.2.200
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.31.3.200
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 10.31.4.200
	netmask 255.255.255.0
```
- **Himmel**
```
auto eth0
iface eth0 inet static
	address 10.31.1.2
	netmask 255.255.255.0
	gateway 10.31.1.200
```

- **Heiter**
```
auto eth0
iface eth0 inet static
	address 10.31.1.3
	netmask 255.255.255.0
	gateway 10.31.1.200
```

- **Denken**
```
auto eth0
iface eth0 inet static
	address 10.31.2.2
	netmask 255.255.255.0
	gateway 10.31.2.200
```

- **Eisen**
```
auto eth0
iface eth0 inet static
	address 10.31.2.3
	netmask 255.255.255.0
	gateway 10.31.2.200
```

- **Lawine**
```
auto eth0
iface eth0 inet static
	address 10.31.3.4
	netmask 255.255.255.0
	gateway 10.31.3.200
```
- **Linie**
```
auto eth0
iface eth0 inet static
	address 10.31.3.3
	netmask 255.255.255.0
	gateway 10.31.3.200
```
- **Lugner**
```
auto eth0
iface eth0 inet static
	address 10.31.3.2
	netmask 255.255.255.0
	gateway 10.31.3.200
```
- **Frieren**
```
auto eth0
iface eth0 inet dhcp
hwaddress ether a6:a1:b9:74:6c:d8

```
- **Flamme**
```
auto eth0
iface eth0 inet static
	address 10.31.4.3
	netmask 255.255.255.0
	gateway 10.31.4.1
```
- **Fern**
```
auto eth0
iface eth0 inet dhcp
hwaddress ether 72:cd:11:a5:a6:47
```
- **Client**
```
auto eth0
iface eth0 inet dhcp
```

## Script.sh

- **Aura**
```bash
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.31.0.0/16
cat /etc/resolv.conf

apt-get update
apt-get install isc-dhcp-relay -y
service isc-dhcp-relay start

echo '
SERVERS="10.31.1.2"
INTERFACES="eth1 eth3 eth4"
OPTIONS=""
' > /etc/default/isc-dhcp-relay

echo '
net.ipv4.ip_forward=1
' > /etc/sysctl.conf

service isc-dhcp-relay restart
```
- **Himmel**
```bash
echo nameserver 192.168.122.1 > /etc/resolv.conf

apt-get update
apt-get install isc-dhcp-server -y
dhcpd --version

echo '
INTERFACES="eth0"
' > /etc/default/isc-dhcp-server

echo '
subnet 10.31.1.0 netmask 255.255.255.0 {
}

subnet 10.31.2.0 netmask 255.255.255.0 {
}

subnet 10.31.3.0 netmask 255.255.255.0 {
    range 10.31.3.16 10.31.3.32;
    range 10.31.3.64 10.31.3.80;
    option routers 10.31.3.200;
    option broadcast-address 10.31.3.255;
    option domain-name-servers 10.31.1.3;
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 10.31.4.0 netmask 255.255.255.0 {
    range 10.31.4.12 10.31.4.20;
    range 10.31.4.160 10.31.4.168;
    option routers 10.31.4.200;
    option broadcast-address 10.31.4.255;
    option domain-name-servers 10.31.1.3;
    default-lease-time 720;
    max-lease-time 5760;
}

host Frieren {
    hardware ethernet a6:a1:b9:74:6c:d8;
    fixed-address 10.31.4.1;
}

host Fern {
    hardware ethernet 72:cd:11:a5:a6:47;
    fixed-address 10.31.4.2;
}

host Lawine {
    hardware ethernet 56:a6:28:4e:55:18;
    fixed-address 10.31.3.1;
}

host Linie {
    hardware ethernet 7e:35:49:9a:8f:8f;
    fixed-address 10.31.3.3;
}

host Lugner {
    hardware ethernet fa:3f:e8:a5:05:bf;
    fixed-address 10.31.3.2;
}

host Sein {
    hardware ethernet de:75:57:5d:b7:a6;
    fixed-address 10.31.4.168;
}

host Stark {
     hardware ethernet ae:79:5c:3d:92:15;
     fixed-address 10.31.4.167;
}

host Revolte {
      hardware ethernet 46:7e:fd:e3:87:76;
      fixed-address 10.31.3.70;
}

host Richter {
       hardware ethernet 92:d5:fe:72:b4:fc;
       fixed-address 10.31.3.69;
}
' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
service isc-dhcp-server status
```

- **Heiter**
```bash
echo nameserver 192.168.122.1 > /etc/resolv.conf

apt-get update
apt-get install bind9 -y

echo '
zone "riegel.canyon.d19.com" {
        type master;
        file "/etc/bind/jarkom/riegel.canyon.d19.com";
};
zone "granz.channel.d19.com" {
        type master;
        file "/etc/bind/jarkom/granz.channel.d19.com";
};
' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/riegel.canyon.d19.com
cp /etc/bind/db.local /etc/bind/jarkom/granz.channel.d19.com
cp /root/named.conf.options /etc/bind/named.conf.options

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     riegel.canyon.d19.com. root.riegel.canyon.d19.com. (
                     2023111301         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      riegel.canyon.d19.com.
@       IN      A       10.31.2.3
WWW     IN      CNAME   riegel.canyon.d19.com.
' > /etc/bind/jarkom/riegel.canyon.d19.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     granz.channel.d19.com. root.granz.channel.d19.com. (
                     2023111302         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      granz.channel.d19.com.
@       IN      A       10.31.2.3
WWW     IN      CNAME   granz.channel.d19.com.
' > /etc/bind/jarkom/granz.channel.d19.com

service bind9 restart
```

- **Denken**
```bash
echo nameserver 192.168.122.1 > /etc/resolv.conf

apt-get update
apt-get install mariadb-server -y
service mysql start

echo '
[mysqld]
skip-networking=0
skip-bind-address
' > /etc/mysql/my.cnf

service mysql restart

mysql -u root -p
```

- **Eisen**
```bash
echo nameserver 192.168.122.1 > /etc/resolv.conf

apt-get update
apt-get install bind9 -y
apt-get install nginx -y
apt-get install apache2-utils -y
apt-get install php7.3 php-fpm -y
service nginx start

mkdir /etc/nginx/rahasisakita/
htpasswd -c -cb /etc/nginx/rahasisakita/.htpasswd netics ajkd19

echo '
upstream backend {
ip_hash;
server 10.31.3.4 weight=25; #IP Lawine
server 10.31.3.3 weight=8; #IP Linie
server 10.31.3.2 weight=1; #IP Lugner
}
 
server {
listen 80; 
server_name granz.channel.d19.com;

location / {
allow 10.31.3.69;
allow 10.31.3.70;
allow 10.31.4.167;
allow 10.31.4.168;
deny all;

proxy_pass http://backend;
proxy_set_header        X-Real-IP $remote_addr;
proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for; 
proxy_set_header        Host $http_host;

auth_basic "Administrator's Area"; 
auth_basic_user_file /etc/nginx/rahasisakita/.htpasswd;
}

location ~ /\.ht { 
    deny all;
}


location /its {
allow 10.31.3.69;
allow 10.31.3.70;
allow 10.31.4.167;
allow 10.31.4.168;
deny all;

proxy_pass https://www.its.ac.id; 
proxy_set_header Host $host; 
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
proxy_set_header X-Forwarded-Proto $scheme;

auth_basic "Administrator's Area"; 
auth_basic_user_file /etc/nginx/rahasisakita/.htpasswd;
}
error_log /var/log/nginx/granz_error.log; 
access_log /var/log/nginx/granz_access.log;


}
' > /etc/nginx/sites-available/granz.channel.d19.com

ln -s /etc/nginx/sites-available/granz.channel.d19.com /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart
```

- **Lawine, Lugner, Linie**
```bash
echo nameserver 192.168.122.1 > /etc/resolv.conf

apt-get update
apt-get install nginx php7.3 php-fpm ca-certificates openssl unzip wget -y

unzip /root/granz.channel.d19.com.zip

mv modul-3 /var/www/granz.channel.d19.com

echo '
server {
    listen 80;

    root /var/www/granz.channel.d19.com;
    index index.php index.html index.htm; server_name granz.channel.d19.com;

    location / {
    try_files $uri $uri/ /index.php?$query_string;
    }

    # pass PHP scripts to FastCGI server 
    location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
    }

    location ~ /\.ht {
    deny all;
    }

    error_log /var/log/nginx/granz_error.log;
    
    access_log /var/log/nginx/granz_access.log;
}
' > /etc/nginx/sites-available/granz.channel.d19.com 

rm -rf /etc/nginx/sites-enabled/default

ln -s /etc/nginx/sites-available/granz.channel.d19.com /etc/nginx/sites-enabled

service php7.3-fpm start
service nginx restart
```
- **Frieren**
```
auto eth0
iface eth0 inet dhcp
hwaddress ether a6:a1:b9:74:6c:d8

```
- **Flamme, Frieren, Fern**
```bash
echo nameserver 192.168.122.1 > /etc/resolv.conf

echo '
auto eth0
iface eth0 inet dhcp
hwaddress ether a6:a1:b9:74:6c:d8
' > /etc/network/interfaces

apt-get update
apt-get install mariadb-client -y

apt-get update
apt-get install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2
curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg
sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'

apt-get update
apt-get install php8.0-mbstring php8.0-xml php8.0-cli php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y
apt-get install nginx -y

wget https://getcomposer.org/download/2.0.13/composer.phar
chmod +x composer.phar
mv composer.phar /usr/bin/composer
composer -v

apt-get install git -y
cd /var/www
git clone https://github.com/martuafernando/laravel-praktikum-jarkom
cd laravel-praktikum-jarkom
composer install
cp .env.example .env
```
- **Client**
```
apt-get update
apt-get install lynx -y
apt-get install dnsutils -y
apt-get install isc-dhcp-server -y
apt-get install apache2-utils -y

echo '
auto eth0
iface eth0 inet dhcp
hwaddress ether [HARDWARE ADDRESS MASING-MASING CLIENT]
' > /etc/network/interfaces
```

## 1
### Script
Berikut adalah script.sh di node Heiter

```
echo nameserver 192.168.122.1 > /etc/resolv.conf

apt-get update
apt-get install bind9 -y

echo '
zone "riegel.canyon.d19.com" {
	type master;
	file "/etc/bind/jarkom/riegel.canyon.d19.com";
};
zone "granz.channel.d19.com" {
	type master;
	file "/etc/bind/jarkom/granz.channel.d19.com";
};
' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/riegel.canyon.d19.com
cp /etc/bind/db.local /etc/bind/jarkom/granz.channel.d19.com
cp /root/named.conf.options /etc/bind/named.conf.options

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     riegel.canyon.d19.com. root.riegel.canyon.d19.com. (
                     2023111301         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      riegel.canyon.d19.com.
@       IN      A       10.31.4.1
WWW     IN      CNAME   riegel.canyon.d19.com.

' > /etc/bind/jarkom/riegel.canyon.d19.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     granz.channel.d19.com. root.granz.channel.d19.com. (
                     2023111302         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      granz.channel.d19.com.
@       IN      A       10.31.3.1
WWW     IN      CNAME   granz.channel.d19.com.

' > /etc/bind/jarkom/granz.channel.d19.com

service bind9 restart
```

### Output
<img width="1552" alt="Screenshot 2023-11-16 at 14 10 13" src="https://github.com/arafifh/Jarkom-Modul-3-D19-2023/assets/89500557/17c8779c-4ba7-489b-b0ba-578d8894d6a9">

## 2
### Script
Berikut adalah script.sh untuk node Himmel

```
subnet 10.31.3.0 netmask 255.255.255.0 {
    range 10.31.3.16 10.31.3.32;
    range 10.31.3.64 10.31.3.80;
    option routers 10.31.3.1;
}
```
Jadi, untuk node yang mempunyai ip 10.31.3.x atau melewati switch 3 akan mendapat ip random dengan constraint yang sudah ditentukan

## 3
### Script
Berikut adalah script.sh untuk node Himmel
```
subnet 10.31.4.0 netmask 255.255.255.0 {
    range 10.31.4.12 10.31.4.20;
    range 10.31.4.160 10.31.4.168;
    option routers 10.31.4.1;
}
```
Jadi, untuk node yang mempunyai ip 10.31.4.x atau melewati switch 4 akan mendapat ip random dengan constraint yang sudah ditentukan

## 4
Melakukan testing dengan cara ping google dari client dengan nameserver mengarah ke Heiter (DNS Server)
<img width="805" alt="image" src="https://github.com/arafifh/Jarkom-Modul-3-D19-2023/assets/89500557/5cad5da6-52ea-49b9-8c29-e3d34d6b91a7">

### Output
<img width="1552" alt="image" src="https://github.com/arafifh/Jarkom-Modul-3-D19-2023/assets/89500557/7e118cf5-d35a-4934-8404-d9a7ad0c43d8">

## 5
Set default lease time dan max lease time dari switch 3 adalah 180 dan 5760, sedangkan untuk switch 4 yaitu 720 dan 5760
### Script
```
subnet 10.31.3.0 netmask 255.255.255.0 {
    range 10.31.3.16 10.31.3.32;
    range 10.31.3.64 10.31.3.80;
    option routers 10.31.3.1;
    option broadcast-address 10.31.3.255;
    option domain-name-servers 10.31.1.3;
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 10.31.4.0 netmask 255.255.255.0 {
    range 10.31.4.12 10.31.4.20;
    range 10.31.4.160 10.31.4.168;
    option routers 10.31.4.1;
    option broadcast-address 10.31.4.255;
    option domain-name-servers 10.31.1.3;
    default-lease-time 720;
    max-lease-time 5760;
}
```
### Output
<img width="584" alt="image" src="https://github.com/arafifh/Jarkom-Modul-3-D19-2023/assets/89500557/c58b81f4-5dd9-4810-bc8d-dbca8c8260dc">

## 6
Dilakukan testing dengan cara lynx ke setiap php worker yaitu Lugner, Linie, Lawine
### Script
```
apt-get update
apt-get install nginx php7.3 php-fpm ca-certificates openssl unzip wget -y

# wget dalam directory root agar tersimpan terus
unzip /root/granz.channel.d19.com.zip

mv modul-3 /var/www/granz.channel.d19.com

echo '
server {
	listen 80;

	root /var/www/granz.channel.d19.com;
	index index.php index.html index.htm; server_name granz.channel.d19.com;

	location / {
	try_files $uri $uri/ /index.php?$query_string;
	}
	
	# pass PHP scripts to FastCGI server location ~ \.php$ {
	include snippets/fastcgi-php.conf;
	fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
	}
	 
	location ~ /\.ht {
	deny all;
	}
	error_log /var/log/nginx/granz_error.log; 
	access_log /var/log/nginx/granz_access.log;
	}
' > /etc/nginx/sites-available/granz.channel.d19.com

rm -rf /etc/nginx/sites-enabled/default

ln -s /etc/nginx/sites-available/granz.channel.d19.com /etc/nginx/sites-enabled

service php7.3-fpm start
service nginx restart
```
### Output
<img width="1552" alt="image" src="https://github.com/arafifh/Jarkom-Modul-3-D19-2023/assets/89500557/913f41cd-e159-4953-b816-49974990900c">
<img width="1552" alt="image" src="https://github.com/arafifh/Jarkom-Modul-3-D19-2023/assets/89500557/837da71f-2aa6-4085-a4f0-59b269b581b7">
<img width="1552" alt="image" src="https://github.com/arafifh/Jarkom-Modul-3-D19-2023/assets/89500557/e38f4bd5-849d-4f94-a9f3-957ce64f21c1">

## 7
### Script
```
echo '
upstream backend {
	server 10.31.3.4 weight=25;	#IP Lawine
	server 10.31.3.3 weight=8;	#IP Linie
	server 10.31.3.2 weight=1;	#IP Lugner
}

server {
	listen 80;
	server_name granz.channel.d19.com;

	location / {
        proxy_pass http://worker;
    }
} > /etc/nginx/sites-available/granz.channel.d19.com

ln -s /etc/nginx/sites-available/granz.channel.d19.com /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart
```
### Output
```
ab -n 1000 -c 100 http://granz.channel.d19.com/
```
<img width="1552" alt="image" src="https://github.com/arafifh/Jarkom-Modul-3-D19-2023/assets/89500557/9bded79e-a7cf-4086-b1f1-2e140ccf9a1b">

## 8
### Robin Round
```
upstream backend {
	server 10.31.3.4	#IP Lawine
	server 10.31.3.3	#IP Linie
	server 10.41.3.2	#IP Lugner
```
<img width="1552" alt="image" src="https://github.com/arafifh/Jarkom-Modul-3-D19-2023/assets/89500557/9bded79e-a7cf-4086-b1f1-2e140ccf9a1b">

### Least-Connection
```
upstream backend {
	least_conn;
	server 10.31.3.4	#IP Lawine
	server 10.31.3.3	#IP Linie
	server 10.41.3.2	#IP Lugner
```
<img width="1552" alt="image" src="https://github.com/arafifh/Jarkom-Modul-3-D19-2023/assets/89500557/42e14c5b-8a49-458b-9a58-2d65b624ec3a">

### IP Hash
```
upstream backend {
	ip_hash;
	server 10.31.3.4	#IP Lawine
	server 10.31.3.3	#IP Linie
	server 10.41.3.2	#IP Lugner
```
<img width="1552" alt="image" src="https://github.com/arafifh/Jarkom-Modul-3-D19-2023/assets/89500557/909b7a09-c51d-496a-bc64-fe7aa191bb29">

## 9
## 10
## 11
## 12
## 13
## 14
## 15
## 16
## 17
## 18
## 19
## 20
