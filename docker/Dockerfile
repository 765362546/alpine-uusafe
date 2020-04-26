FROM frolvlad/alpine-java:jdk8-slim

# 包含 nginx qrencode ffmpeg nodejs mysql客户端 aapt依赖包
#
# 修改源
RUN echo "http://mirrors.aliyun.com/alpine/latest-stable/main/" > /etc/apk/repositories && \
    echo "http://mirrors.aliyun.com/alpine/latest-stable/community/" >> /etc/apk/repositories

# 安装需要的软件 nginx编译相关 qrencode ffmpeg nodejs mysql
RUN apk update && \
    apk add --no-cache ca-certificates && \
    apk add --no-cache curl bash tree tzdata  build-base pcre pcre-dev openssl openssl-dev zlib zlib-dev wget perl && \
    apk add --no-cache ffmpeg libqrencode nodejs mariadb-client && \
    cp -rf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
          
# 下载nginx相关安装包
# 源码包下载，采用提前下载好的方式，包括，注意src目录与Dockerfile平级
#nginx-1.16.1             nginx_upstream_check_module-master  pcre-8.37
#nginx-upload-module-2.2  openssl-1.0.2u                      zlib-1.2.11
COPY ./src/ /tmp/src/
RUN CONFIG="--prefix=/opt/nginx --user=nginx --group=nginx --with-http_ssl_module --with-http_stub_status_module --with-http_gzip_static_module  --with-stream --with-stream_ssl_module --with-stream_realip_module --with-http_realip_module --with-http_gunzip_module --with-http_degradation_module --with-mail --with-mail_ssl_module --with-http_slice_module --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_mp4_module --with-http_v2_module --with-http_secure_link_module --with-pcre-jit --with-http_sub_module --with-stream_ssl_preread_module --with-pcre=../pcre-8.37 --with-openssl=../openssl-1.0.2u --with-zlib=../zlib-1.2.11 --add-module=../nginx-upload-module-2.2 --add-module=../nginx_upstream_check_module-master" && \
cd /tmp/src/nginx-1.16.1/ && \
./configure $CONFIG  && \
make  -j$(getconf _NPROCESSORS_ONLN) && \
make install && \
rm -rf /tmp/src

#
EXPOSE 3000
EXPOSE 3001
EXPOSE 3002
EXPOSE 3003
EXPOSE 3008
EXPOSE 3020
