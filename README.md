
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
  

