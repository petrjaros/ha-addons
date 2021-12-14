ARG BUILD_FROM
FROM $BUILD_FROM as builder

ENV LANG C.UTF-8

RUN apk add --no-cache \
    build-base \
    argp-standalone \
    git \
    mosquitto-dev \
    cmake
    
WORKDIR /build

RUN git clone https://github.com/john30/ebusd.git /build && cmake . && make

FROM $BUILD_FROM

RUN apk add --no-cache \
    git

LABEL Description="ebusd"

RUN apk add --no-cache \
    build-base \
    argp-standalone \
    mosquitto-dev

COPY --from=builder /build/src/ebusd/ebusd /usr/local/bin/ebusd
COPY --from=builder /build/src/tools/ebusctl /usr/local/bin/ebusctl

RUN git clone https://github.com/john30/ebusd-configuration.git /config
RUN ln -s /config/latest/en/vaillant/15.f43.csv /config/latest/en/vaillant/15.f45.csv

WORKDIR /

COPY run.sh /
RUN chmod a+x /run.sh
CMD [ "/run.sh" ]
