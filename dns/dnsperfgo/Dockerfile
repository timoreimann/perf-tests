# build image
FROM golang:1.20.3 AS build-env
ARG gopkg=k8s.io/perf-tests/dns/dnsperfgo
ADD [".", "/go/src/$gopkg"]
WORKDIR /go/src/$gopkg

RUN CGO_ENABLED=1 go build -o /dnsperfcgo
RUN CGO_ENABLED=0 go build -o /dnsperfgo

# runtime image
FROM gcr.io/distroless/base-debian11:debug
COPY --from=build-env /dnsperfgo /dnsperfgo
COPY --from=build-env /dnsperfcgo /dnsperfcgo
ENTRYPOINT ["/dnsperfcgo"]
