FROM frolvlad/alpine-glibc:alpine-3.8

# 包含  jdk nginx qrencode ffmpeg nodejs mysql客户端 aapt依赖包
#
# 修改源
RUN echo "http://mirrors.aliyun.com/alpine/v3.8/main/" > /etc/apk/repositories && \
    echo "http://mirrors.aliyun.com/alpine/v3.8/community/" >> /etc/apk/repositories

# 安装需要的软件 nginx编译相关 qrencode ffmpeg nodejs mysql
RUN apk update && \
    apk add --no-cache --virtual=build-dependencies ca-certificates && \
    apk add --no-cache zip unzip curl bash tree tzdata  build-base pcre pcre-dev openssl openssl-dev zlib zlib-dev wget perl && \
    apk add --no-cache ffmpeg libqrencode nodejs mariadb-client && \
    cp -rf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

# jdk8 采用提前下载好的方式，包括，注意jdk目录与Dockerfile平级
ENV JAVA_VERSION=8 \
    JAVA_UPDATE=231 \
    JAVA_HOME="/usr/lib/jvm/default-jvm"
COPY ./jdk/jdk-8u231-linux-x64.tar.gz /tmp/
RUN  cd "/tmp" && \
    tar -xzf jdk-8u231-linux-x64.tar.gz && \
    mkdir -p "/usr/lib/jvm" && \
    mv "/tmp/jdk1.${JAVA_VERSION}.0_${JAVA_UPDATE}" "/usr/lib/jvm/java-${JAVA_VERSION}-oracle" && \
    ln -s "java-${JAVA_VERSION}-oracle" "$JAVA_HOME" && \
    ln -s "$JAVA_HOME/bin/"* "/usr/bin/" && \
    rm -rf "$JAVA_HOME/"*src.zip && \
    rm -rf "$JAVA_HOME/lib/missioncontrol" \
           "$JAVA_HOME/lib/visualvm" \
           "$JAVA_HOME/lib/"*javafx* \
           "$JAVA_HOME/jre/lib/plugin.jar" \
           "$JAVA_HOME/jre/lib/ext/jfxrt.jar" \
           "$JAVA_HOME/jre/bin/javaws" \
           "$JAVA_HOME/jre/lib/javaws.jar" \
           "$JAVA_HOME/jre/lib/desktop" \
           "$JAVA_HOME/jre/plugin" \
           "$JAVA_HOME/jre/lib/"deploy* \
           "$JAVA_HOME/jre/lib/"*javafx* \
           "$JAVA_HOME/jre/lib/"*jfx* \
           "$JAVA_HOME/jre/lib/amd64/libdecora_sse.so" \
           "$JAVA_HOME/jre/lib/amd64/"libprism_*.so \
           "$JAVA_HOME/jre/lib/amd64/libfxplugins.so" \
           "$JAVA_HOME/jre/lib/amd64/libglass.so" \
           "$JAVA_HOME/jre/lib/amd64/libgstreamer-lite.so" \
           "$JAVA_HOME/jre/lib/amd64/"libjavafx*.so \
           "$JAVA_HOME/jre/lib/amd64/"libjfx*.so && \
    rm "/tmp/"* && \
    echo 'public class Main { public static void main(String[] args) { System.out.println("Java code is running fine!"); } }' > Main.java && \
    javac Main.java && \
    java Main && \
    rm -r "/tmp/"*

# 下载nginx相关安装包
# 源码包下载，采用提前下载好的方式，包括，注意src目录与Dockerfile平级
#nginx-1.16.1             nginx_upstream_check_module-master  pcre-8.37
#nginx-upload-module-2.2  openssl-1.0.2u                      zlib-1.2.11
COPY ./src/ /tmp/src/
#RUN CONFIG="--prefix=/opt/nginx --user=nginx --group=nginx --with-http_ssl_module --with-http_stub_status_module --with-http_gzip_static_module  --with-stream --with-stream_ssl_module --with-stream_realip_module --with-http_realip_module --with-http_gunzip_module --with-http_degradation_module --with-mail --with-mail_ssl_module --with-http_slice_module --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_mp4_module --with-http_v2_module --with-http_secure_link_module --with-pcre-jit --with-http_sub_module --with-stream_ssl_preread_module --with-pcre=../pcre-8.37 --with-openssl=../openssl-1.0.2u --with-zlib=../zlib-1.2.11 --add-module=../nginx-upload-module-2.2 --add-module=../nginx_upstream_check_module-master" && \
RUN CONFIG="--prefix=/opt/nginx --user=nginx --group=nginx --with-http_ssl_module --with-http_stub_status_module --with-http_gzip_static_module  --with-stream --with-stream_ssl_module --with-stream_realip_module --with-http_realip_module --with-http_gunzip_module --with-http_degradation_module --with-mail --with-mail_ssl_module --with-http_slice_module --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_mp4_module --with-http_v2_module --with-http_secure_link_module --with-pcre-jit --with-http_sub_module --with-stream_ssl_preread_module --with-pcre=../pcre-8.37 --with-openssl=../openssl-1.0.2u --with-zlib=../zlib-1.2.11 --add-module=../nginx-upload-module-2.2 " && \

cd /tmp/src/nginx-1.16.1/ && \
./configure $CONFIG  && \
make  -j$(getconf _NPROCESSORS_ONLN) && \
make install && \
rm -rf /tmp/src

# aapt依赖
RUN cd /tmp/ && \
wget https://mirrors.aliyun.com/alpine/v3.8/main/x86/zlib-1.2.11-r1.apk && \
wget https://mirrors.aliyun.com/alpine/v3.8/main/x86/zlib-dev-1.2.11-r1.apk && \
apk add zlib-1.2.11-r1.apk zlib-dev-1.2.11-r1.apk && \
apk del build-dependencies

EXPOSE 3000
EXPOSE 3001
EXPOSE 3002
EXPOSE 3003
EXPOSE 3008
EXPOSE 3020
