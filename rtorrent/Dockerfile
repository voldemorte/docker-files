FROM alpine:3.16.2

RUN apk add --no-cache \
      rtorrent

COPY entrypoint /usr/local/bin/entrypoint

ENTRYPOINT ["/usr/local/bin/entrypoint"]
