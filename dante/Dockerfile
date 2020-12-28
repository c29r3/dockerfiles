FROM debian:jessie
MAINTAINER Devops <devops@sprucefinance.com>

ENV DANTE_VER 1.4.2
ENV DANTE_URL https://github.com/notpeter/dante/archive/dante-$DANTE_VER.tar.gz
ENV DANTE_SHA eb5890dab4a353bf8c4248a4bc8e3058d8ad474b
ENV DANTE_FILE dante.tar.gz
ENV DANTE_TEMP dante
ENV DANTE_DEPS build-essential curl

RUN set -xe \
    && apt-get update \
    && apt-get install -y $DANTE_DEPS \
    && mkdir $DANTE_TEMP \
        && cd $DANTE_TEMP \
        && curl -sSL $DANTE_URL -o $DANTE_FILE \
        && echo "$DANTE_SHA *$DANTE_FILE" | shasum -c \
        && tar xzf $DANTE_FILE --strip 1 \
        && cd dante \
        && ./configure \
        && make install \
        && cd ../.. \
        && rm -rf $DANTE_TEMP \
    && apt-get purge -y --auto-remove $DANTE_DEPS \
    && rm -rf /var/lib/apt/lists/*

COPY sockd.conf /etc/sockd.conf
COPY entrypoint.sh /entrypoint.sh

ENV CFGFILE /etc/sockd.conf
ENV PIDFILE /tmp/sockd.pid
ENV WORKERS 10

EXPOSE 55555

ENTRYPOINT /entrypoint.sh
CMD sockd -f $CFGFILE -p $PIDFILE -N $WORKERS

