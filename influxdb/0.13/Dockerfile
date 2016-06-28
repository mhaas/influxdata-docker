FROM armhf/buildpack-deps:jessie-curl

RUN gpg \
    --keyserver hkp://ha.pool.sks-keyservers.net \
    --recv-keys 05CE15085FC09D18E99EFB22684A14CF2582E0C5

ENV INFLUXDB_VERSION 0.13.0
ENV TARGET_ARCH armhf
RUN wget -q https://dl.influxdata.com/influxdb/releases/influxdb_${INFLUXDB_VERSION}_${TARGET_ARCH}.deb.asc && \
    wget -q https://dl.influxdata.com/influxdb/releases/influxdb_${INFLUXDB_VERSION}_${TARGET_ARCH}.deb && \
    gpg --batch --verify influxdb_${INFLUXDB_VERSION}_${TARGET_ARCH}.deb.asc influxdb_${INFLUXDB_VERSION}_${TARGET_ARCH}.deb && \
    dpkg -i influxdb_${INFLUXDB_VERSION}_${TARGET_ARCH}.deb && \
    rm -f influxdb_${INFLUXDB_VERSION}_${TARGET_ARCH}.deb*
COPY influxdb.conf /etc/influxdb/influxdb.conf

EXPOSE 8083 8086

VOLUME /var/lib/influxdb

COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["influxd"]
