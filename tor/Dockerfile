FROM alpine:3.11

RUN addgroup -g 1001 -S tor && \
    adduser -D -u 1001 -s /sbin/nologin -G tor -g tor tor && \
    apk update && \
    apk add --no-cache --virtual .builddeps gcc g++ make wget && \
    apk add libevent-dev openssl-dev zlib-dev && \
    wget -qO /srv/tor.tar.gz https://dist.torproject.org/tor-0.4.2.6.tar.gz && \
    cd /srv/ && \
    tar xzf tor.tar.gz && \
    cd tor-0.4.2.6 && \
    ./configure && make && \
    make install && \
    apk del --no-network .builddeps && \
    cd $HOME && \
    rm -fr /srv/tor* && \
    echo 'SOCKSPort 0.0.0.0:9050' | tee -a /usr/local/etc/tor/torrc

USER tor

ENTRYPOINT ["/usr/local/bin/tor", "-f", "/usr/local/etc/tor/torrc"]

skdjvn
