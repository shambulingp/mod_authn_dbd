FROM debian:buster-slim as build

WORKDIR /build

RUN apt-get update && \
  apt-get install -y ca-certificates make automake git g++ libtool pkg-config autoconf libssl-dev check libjansson-dev libz-dev procps apache2 apache2-dev
 
RUN git clone https://github.com/shambulingp/mod_authn_dbd.git && \
    cd httpd && \
  #  git checkout tags/v$MOD_AUTHNZ_JWT_VERSION && \
    autoreconf -ivf && \
    PKG_CONFIG_PATH=/usr/local ./configure && \
    make && \
    make install
    
FROM httpd:2.4

COPY --from=build /usr/lib/apache2/modules/mod_authn_dbd.so /usr/local/apache2/modules/mod_authn_dbd.so

RUN echo "LoadModule authn_dbd_module modules/mod_authn_dbd.so" >> /usr/local/apache2/conf/httpd.conf

