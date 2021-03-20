FROM openjdk:8-jdk-alpine

# 解决alpine镜像缺少特定字体的问题
RUN apk add --update ttf-dejavu fontconfig && rm -rf /var/cache/apk/*