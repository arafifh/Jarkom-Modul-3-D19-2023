# Jarkom-Modul-3-D19-2023

Adrian Ismu
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
