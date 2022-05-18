FROM golang:1.17-alpine3.15 as build

ARG VERSION=4.9.0
ARG DATADIR=/home/geoipupdate/GeoIP
ARG CONFFILE=/home/geoipupdate/GeoIP.conf

ENV GOARK=amd64
ENV GOOS=linux
ENV CGO_ENABLED=0

RUN apk add --no-cache make

COPY . /usr/lib/src/geoipupdate
WORKDIR /usr/lib/src/geoipupdate

RUN go mod download
RUN make build/geoipupdate



FROM alpine:3

COPY --from=build /usr/lib/src/geoipupdate/build/geoipupdate /usr/bin/
COPY docker/entry.sh /usr/bin/entry.sh

RUN addgroup -S -g 1000 geoipupdate \
    && adduser -S -u 1000 -G geoipupdate geoipupdate

ENTRYPOINT ["/usr/bin/entry.sh"]

USER 1000:1000
