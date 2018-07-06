FROM golang:alpine as gobuild

RUN apk update; \
	apk add git; \
	go get -v github.com/coredns/coredns

WORKDIR /go/src/github.com/coredns/coredns
RUN CGO_ENABLED=0 go build -ldflags="-s -w -X github.com/coredns/coredns/coremain.GitCommit=$(git describe --dirty --always)" -o coredns

FROM alpine

RUN apk add --no-cache ca-certificates; \
    rm -rf /var/cache/apk/*;

COPY --from=gobuild /go/src/github.com/coredns/coredns/coredns /coredns
COPY Corefile /Corefile

ADD https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts /block.hosts
EXPOSE 53 53/udp
EXPOSE 9153

HEALTHCHECK CMD wget -q localhost:8080/health -O /dev/null || exit 1

CMD ["/coredns", "-conf", "/Corefile"]
