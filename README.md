# Jarkom-Modul-3-D19-2023

Adrian Ismu Arifianto - 5025211116
<br>
Ahmad Rafif Hikmatiar - 5025211247

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
