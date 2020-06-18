
Пример работающего  файла настроек NGINX для GeoIP с PowerDNS.


Оптимизация Nginx или другие настройки будут добавлены позднее...




Версия nginx nginx/1.18.0 ( Ubuntu 20.04 LTS ) 

Библиотека Geoip2  - ngx_http_geoip2_module https://github.com/leev/ngx_http_geoip2_module
	
Базы городов и стран , можно скачат сайте https://dev.maxmind.com. После регистрации на сайте.

Пример конфигурации pdns.conf ( pdns-backend-mysql  ) 

Компиляция nginx:

./configure --prefix=/ --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules --conf-path=/etc/nginx/nginx.conf \
 --error-log-path=/var/log/nginx/error.log \
 --http-log-path=/var/log/nginx/access.log \
 --pid-path=/var/run/nginx.pid \
 --lock-path=/var/run/nginx.lock  \
 --http-client-body-temp-path=/var/cache/nginx/client_temp \
 --http-proxy-temp-path=/var/cache/nginx/proxy_temp \
 --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp \
 --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp \
 --http-scgi-temp-path=/var/cache/nginx/scgi_temp \
 --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module \
 --with-http_auth_request_module --with-http_dav_module --with-http_flv_module \
 --with-http_gunzip_module --with-http_gzip_static_module \
 --with-http_mp4_module --with-http_random_index_module \
 --with-http_realip_module --with-http_secure_link_module \
 --with-http_slice_module --with-http_ssl_module \
 --with-http_stub_status_module --with-http_sub_module \
 --with-http_v2_module \
 --with-mail --with-mail_ssl_module \
 --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module \
 --with-http_geoip_module=dynamic \
 --with-http_geoip_module \
 --with-http_image_filter_module=dynamic \
 --with-stream_geoip_module=dynamic \
 --with-stream_geoip_module \
 --with-debug \
 --with-cc-opt='-g -O2 -fdebug-prefix-map=/data/builder/debuild/nginx-1.18.0/debian/debuild-base/nginx-1.18.0=. -fstack-protector-strong -Wformat -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2 -fPIC' --with-ld-opt='-Wl,-Bsymbolic-functions -Wl,-z,relro -Wl,-z,now -Wl,--as-needed -pie' --add-dynamic-module=/tmp/nginx/ngx_http_geoip2_module 



  
Для связи , в общую VPN сеть использую wireguard https://www.wireguard.com/




Сервер : 

/etc/wireguard/wg0.conf   ( Название файла - это название интерфейса !! , если вам нужно несколько различных сетей , можно придумать 
			   wg1.conf , wg2.conf  и т.д. )


[Interface]
Address = 10.1.0.10/24
PrivateKey =  "Секретный ключ сервера  №1  "


# На одном интерфейсе wg0 можно разместить 2 сети , или их можно разнести в разные файлы . wg0.conf и wg2.conf 
[Interface]
Address = 10.2.0.10/24
PrivateKey =  "Секретный ключ сервера  №2  "

# Входящий порт , на IP X.X.X.X
ListenPort = 51258  

[Peer]
#  pdns_en
PublicKey =  "Публичный ключ pdns_en сервера" 
AllowedIPs = 10.1.0.1/32

[Peer]
# pdns_ru
PublicKey =   "Публичный ключ pdns_ru сервера" 
AllowedIPs = 10.2.0.1/32



Клиенты:

pdns_en
/etc/wireguard/wg0.conf

[Interface]
PrivateKey = "Секретный ключ pdns_en " 
# внутренний адрес pdns_en на интерфейсе wg0 , который виден для VPN сервера 
Address = 10.1.0.1/32 


[Peer]
# 
PublicKey = "Публичный ключ для VPN Сервера , генерируется из ключа №2 "

# Разрешенные  маршруты сетей

AllowedIPs = 10.2.0.0/24
# Внешний IP адрес и порт сервера 
Endpoint = X.X.X.X:51258  
PersistentKeepalive = 25


По аналогии делаем и для 

pdns_ru
/etc/wireguard/wg0.conf

[Interface]
PrivateKey = "Секретный ключ pdns_ru " 
# внутренний адрес pdns_en на интерфейсе wg0 , который виден для VPN сервера 
Address = 10.2.0.1/32 


[Peer]
#
PublicKey = "Публичный ключ для VPN Сервера , генерируется из ключа №1 "

# Разрешенные  маршруты сетей

AllowedIPs = 10.1.0.0/24
# Внешний IP адрес и порт сервера 
Endpoint = X.X.X.X:51258  
PersistentKeepalive = 25

