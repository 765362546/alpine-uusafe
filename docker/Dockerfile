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
