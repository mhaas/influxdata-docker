# TODO: this does not work - there is no "static" build of influxdb,
# which is what the original Dockerfile used for amd64. The non-static
# version fails with an error message about influxd not being found
# The Debian-based version works!

# fail the build...
TODO_BROKEN
FROM armhf/alpine:3.4
ENV INFLUXDB_VERSION 0.13.0
# is there an official env variable for this?
ENV TARGET_ARCH armhf
RUN apk add --no-cache --virtual .build-deps wget gnupg tar ca-certificates && \
    update-ca-certificates && \
    gpg --keyserver hkp://ha.pool.sks-keyservers.net \
        --recv-keys 05CE15085FC09D18E99EFB22684A14CF2582E0C5 && \
    wget  https://dl.influxdata.com/influxdb/releases/influxdb-${INFLUXDB_VERSION}_linux_${TARGET_ARCH}.tar.gz.asc && \
    wget https://dl.influxdata.com/influxdb/releases/influxdb-${INFLUXDB_VERSION}_linux_${TARGET_ARCH}.tar.gz && \
    gpg -v -v --batch --verify influxdb-${INFLUXDB_VERSION}_linux_${TARGET_ARCH}.tar.gz.asc influxdb-${INFLUXDB_VERSION}_linux_${TARGET_ARCH}.tar.gz && \
    mkdir -p /usr/src && \
    tar -C /usr/src -xzf influxdb-${INFLUXDB_VERSION}_linux_${TARGET_ARCH}.tar.gz && \
    rm -f /usr/src/influxdb-*/influxdb.conf && \
    chmod +x /usr/src/influxdb-*/* && \
    cp -a /usr/src/influxdb-*/* /usr/bin/ && \
    rm -rf *.tar.gz* /usr/src /root/.gnupg && \
    apk del .build-deps
COPY influxdb.conf /etc/influxdb/influxdb.conf

EXPOSE 8083 8086

VOLUME /var/lib/influxdb

COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["influxd"]
