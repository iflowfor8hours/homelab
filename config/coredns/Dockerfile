FROM golang as gobuild

COPY healthcheck.go /healthcheck.go
RUN CGO_ENABLED=0 go build -o /healthcheck /healthcheck.go

FROM coredns/coredns:1.2.0

COPY Corefile /Corefile

ADD https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts /block.hosts

COPY --from=gobuild /healthcheck /healthcheck
EXPOSE 9153
HEALTHCHECK --interval=1s --timeout=1s --start-period=2s --retries=3 CMD [ "/healthcheck" ]

ENTRYPOINT ["/coredns", "-conf", "/Corefile"]
