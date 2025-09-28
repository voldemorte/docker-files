# ---------- Build stage ----------
FROM alpine:3.22 AS build

ARG TOR_VERSION=0.4.8.9
WORKDIR /src

# Build toolchain + headers only in this stage
RUN apk add --no-cache \
      build-base autoconf automake libtool pkgconfig \
      libevent-dev openssl-dev zlib-dev \
      wget ca-certificates

# Download, build, strip, and install to a private prefix
RUN wget -q https://dist.torproject.org/tor-${TOR_VERSION}.tar.gz \
 && tar -xzf tor-${TOR_VERSION}.tar.gz \
 && cd tor-${TOR_VERSION} \
 && ./configure --prefix=/opt/tor --disable-unittests --disable-dependency-tracking \
 && make -j"$(nproc)" \
 && make install-strip

# ---------- Runtime stage ----------
FROM alpine:3.22

# Runtime deps only (shared libs)
RUN apk add --no-cache \
      libevent openssl zlib ca-certificates

# Non-root user
RUN addgroup -g 1001 -S tor \
 && adduser -D -u 1001 -s /sbin/nologin -G tor -g tor tor

# Copy built artifacts
COPY --from=build /opt/tor/ /opt/tor/

# Minimal config + data dir
RUN mkdir -p /opt/tor/etc/tor /opt/tor/var/lib/tor \
 && printf "SOCKSPort 0.0.0.0:9050\n" > /opt/tor/etc/tor/torrc \
 && chown -R tor:tor /opt/tor

USER tor
EXPOSE 9050

# Use the privately installed tor and explicit data dir (owned by 'tor')
ENTRYPOINT ["/opt/tor/bin/tor", "-f", "/opt/tor/etc/tor/torrc", "--DataDirectory", "/opt/tor/var/lib/tor"]
