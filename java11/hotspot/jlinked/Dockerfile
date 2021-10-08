# Stage 1: Download & Build Spring-PetClinic
FROM adoptopenjdk/openjdk11:latest as builder
LABEL maintainer "Ioannis L. Kolaxis"

RUN apt-get update
RUN apt-get install -y \
	git \
	maven

WORKDIR /tmp
RUN git clone https://github.com/spring-projects/spring-petclinic.git

WORKDIR /tmp/spring-petclinic
RUN mvn install

# Build a custom Java Runtime, for Spring-PetClinic
WORKDIR /opt
RUN ["jlink", "--no-header-files", \
     "--no-man-pages", \
     "--compress=2", \
     "--strip-debug", \
     "--add-modules", "java.sql,java.naming,java.desktop,java.management,java.security.jgss,java.instrument,jdk.unsupported", \
     "--output", "java-light"]

# Stage 2: Build the image that will be used for the Performance Tests
FROM alpine:3.11.12

# Following command was copied from AdoptOpenJDK Dockerfile 
RUN apk --update add --no-cache --virtual .build-deps curl binutils \
    && GLIBC_VER="2.29-r0" \
    && ALPINE_GLIBC_REPO="https://github.com/sgerrand/alpine-pkg-glibc/releases/download" \
    && GCC_LIBS_URL="https://archive.archlinux.org/packages/g/gcc-libs/gcc-libs-8.2.1%2B20180831-1-x86_64.pkg.tar.xz" \
    && GCC_LIBS_SHA256=e4b39fb1f5957c5aab5c2ce0c46e03d30426f3b94b9992b009d417ff2d56af4d \
    && ZLIB_URL="https://archive.archlinux.org/packages/z/zlib/zlib-1%3A1.2.9-1-x86_64.pkg.tar.xz" \
    && ZLIB_SHA256=bb0959c08c1735de27abf01440a6f8a17c5c51e61c3b4c707e988c906d3b7f67 \
    && curl -Ls https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub -o /etc/apk/keys/sgerrand.rsa.pub \
    && curl -Ls ${ALPINE_GLIBC_REPO}/${GLIBC_VER}/glibc-${GLIBC_VER}.apk > /tmp/${GLIBC_VER}.apk \
    && apk add /tmp/${GLIBC_VER}.apk \
    && curl -Ls ${GCC_LIBS_URL} -o /tmp/gcc-libs.tar.xz \
    && echo "${GCC_LIBS_SHA256}  /tmp/gcc-libs.tar.xz" | sha256sum -c - \
    && mkdir /tmp/gcc \
    && tar -xf /tmp/gcc-libs.tar.xz -C /tmp/gcc \
    && mv /tmp/gcc/usr/lib/libgcc* /tmp/gcc/usr/lib/libstdc++* /usr/glibc-compat/lib \
    && strip /usr/glibc-compat/lib/libgcc_s.so.* /usr/glibc-compat/lib/libstdc++.so* \
    && curl -Ls ${ZLIB_URL} -o /tmp/libz.tar.xz \
    && echo "${ZLIB_SHA256}  /tmp/libz.tar.xz" | sha256sum -c - \
    && mkdir /tmp/libz \
    && tar -xf /tmp/libz.tar.xz -C /tmp/libz \
    && mv /tmp/libz/usr/lib/libz.so* /usr/glibc-compat/lib \
    && apk del --purge .build-deps \
    && rm -rf /tmp/${GLIBC_VER}.apk /tmp/gcc /tmp/gcc-libs.tar.xz /tmp/libz /tmp/libz.tar.xz /var/cache/apk/*

COPY --from=builder /opt/java-light /opt/java-light/

ENV JAVA_VERSION jdk-11.0.2+9
ENV JAVA_HOME=/opt/java-light
ENV JAVA_HOME=/opt/java-light \
    PATH="/opt/java-light/bin:$PATH"
ENV JAVA_TOOL_OPTIONS=""

RUN mkdir /opt/spring-petclinic
COPY --from=builder /tmp/spring-petclinic/target/spring-petclinic-2.1.0.BUILD-SNAPSHOT.jar /opt/spring-petclinic
CMD ["/opt/java-light/bin/java","-jar","/opt/spring-petclinic/spring-petclinic-2.1.0.BUILD-SNAPSHOT.jar"]
