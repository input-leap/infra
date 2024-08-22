FROM golang:1.17 as builder

WORKDIR /
RUN go install github.com/DarthSim/hivemind@v1.0.6

RUN git clone https://github.com/lineageos-infra/matterbridge && \
    cd matterbridge && \
    git checkout v1.26.0 && \
    go build . && \
    cp matterbridge /go/bin/

FROM ubuntu:noble

COPY --from=builder /go/bin/matterbridge /bin/matterbridge
COPY --from=builder /go/bin/hivemind /bin/hivemind

RUN apt update && apt install -y ca-certificates

COPY matterbridge.toml /etc/matterbridge.toml

CMD ["/bin/matterbridge", "-conf", "/etc/matterbridge.toml"]
