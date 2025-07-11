# syntax=docker/dockerfile:1
ARG BASE_IMAGE
ARG PUREFTPD_VERSION=1.0.47

FROM ${BASE_IMAGE:-alpine-s6:local} AS builder
ARG PUREFTPD_VERSION

RUN apk --update --no-cache add curl patch tar
WORKDIR /dist/pureftpd
COPY patchs /dist
RUN curl -sSL "https://download.pureftpd.org/pub/pure-ftpd/releases/obsolete/pure-ftpd-${PUREFTPD_VERSION}.tar.gz" | tar xz --strip 1 \
  && patch -p1 < ../minimal.patch

RUN apk --update --no-cache add \
    autoconf \
    automake \
    binutils \
    build-base \
    gnu-libiconv-dev \
    libsodium-dev \
    mariadb-connector-c-dev \
    openldap-dev \
    postgresql-dev \
    openssl-dev \
    tree

RUN ./configure \
    --prefix=/pure-ftpd \
    --without-humor \
    --without-inetd \
    --without-pam \
    --with-altlog \
    --with-cookie \
    --with-ftpwho \
    --with-ldap \
    --with-mysql \
    --with-pgsql \
    --with-puredb \
    --with-quotas \
    --with-ratios \
    --with-rfc2640 \
    --with-throttling \
    --with-tls \
    --with-uploadscript \
    --with-brokenrealpath \
    --with-certfile=/data/pureftpd.pem \
  && make install-strip \
  && tree /pure-ftpd

FROM ${BASE_IMAGE:-alpine-s6:local}

ENV S6_BEHAVIOUR_IF_STAGE2_FAILS="2" \
  SOCKLOG_TIMESTAMP_FORMAT="" \
  PURE_PASSWDFILE="/data/pureftpd.passwd" \
  PURE_DBFILE="/data/pureftpd.pdb" \
  TZ="UTC"

RUN apk --update --no-cache add \
    bind-tools \
    gnu-libiconv \
    libldap \
    libpq \
    libsodium \
    mariadb-connector-c \
    mysql-client \
    openldap-clients \
    openssl \
    postgresql-client \
    tzdata \
    zlib \
  && rm -f /etc/socklog.rules/*

COPY --from=builder /pure-ftpd /
COPY rootfs /

RUN mkdir -p /data && pure-ftpwho --help

EXPOSE 2100 30000-30009
WORKDIR /data
VOLUME [ "/data" ]
