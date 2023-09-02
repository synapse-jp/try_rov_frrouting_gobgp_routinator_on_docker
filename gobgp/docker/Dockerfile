FROM golang:1.20.6-alpine3.18 AS builder
RUN apk update && apk add git \
  && git clone https://github.com/osrg/gobgp.git /go/src/github.com/osrg/gobgp \
  && cd /go/src/github.com/osrg/gobgp \
  && go mod download \
  && cd /go/src/github.com/osrg/gobgp/cmd/gobgp \
  && go build \
  && cd /go/src/github.com/osrg/gobgp/cmd/gobgpd \
  && go build \
  && mkdir /ribs

FROM scratch
COPY --from=builder /go/src/github.com/osrg/gobgp/cmd/gobgp/gobgp /gobgp
COPY --from=builder /go/src/github.com/osrg/gobgp/cmd/gobgpd/gobgpd /gobgpd
COPY --from=builder /ribs /ribs
CMD ["/gobgpd"]
