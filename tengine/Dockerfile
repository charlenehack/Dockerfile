FROM alpine:latest
LABEL maintainer="liucl <119215719@qq.com>"

ARG TENGINE_VERSION="-2.2.2"

ADD tengine${TENGINE_VERSION}.tar.gz /usr/local/
WORKDIR /usr/local

RUN apk add build-base && apk add openssl openssl-dev pcre-dev libxml2-dev libxslt-dev gd-dev libgd geoip geoip-dev perl-dev && \
addgroup -S www && adduser -D -S -H -s /sbin/nologin -G www www && \
cd tengine${TENGINE_VERSION} && ./configure \
	--enable-mods-static=all \
        --user=www \
        --group=www \
        --prefix=/usr/local/nginx \
        --conf-path=/etc/nginx/nginx.conf \
        --lock-path=/var/run/nginx.lock \
        --pid-path=/var/run/nginx.pid \
        --http-client-body-temp-path=/usr/local/nginx/body \
        --http-fastcgi-temp-path=/usr/local/nginx/fastcgi \
        --http-proxy-temp-path=/usr/local/nginx/proxy \
        --http-scgi-temp-path=/usr/local/nginx/scgi \
        --http-uwsgi-temp-path=/usr/local/nginx/uwsgi \
        --with-http_ssl_module \
        --with-http_gzip_static_module \
        --with-http_gunzip_module \
        --with-md5=/usr/include/openssl \
        --with-sha1-asm \
        --with-md5-asm \
        --with-http_auth_request_module \
        --with-http_image_filter_module \
        --with-http_addition_module \
        --with-http_dav_module \
        --with-http_realip_module \
        --with-http_ssl_module \
        --with-http_stub_status_module \
        --with-http_sub_module \
        --with-http_xslt_module \
        --with-http_upstream_ip_hash_module=shared \
        --with-http_upstream_least_conn_module=shared \
        --with-http_upstream_session_sticky_module=shared \
        --with-http_map_module=shared \
        --with-http_user_agent_module=shared \
        --with-http_mp4_module \
        --with-http_split_clients_module=shared \
        --with-http_access_module=shared \
        --with-http_user_agent_module=shared \
        --with-http_degradation_module \
        --with-http_upstream_check_module \
        --with-http_upstream_consistent_hash_module \
#        --with-file-aio \
        --with-mail \
        --with-mail_ssl_module \
        --with-pcre \
        --with-pcre-jit \
        --with-debug \
        --http-log-path=/var/log/access.log \
        --error-log-path=/var/log/error.log \
        --sbin-path=/usr/sbin/nginx && \
make && make install && mkdir /etc/nginx/conf.d && mkdir /etc/nginx/sites-enabled && \
for folder in body fastcgi proxy scgi uwsgi ; do mkdir /usr/local/nginx/$folder; done && \
rm -rf ../tengine${TENGINE_VERSION} && apk del build-base
COPY nginx.conf /etc/nginx/
EXPOSE 80 443
CMD ["nginx","-g","daemon off;"]
