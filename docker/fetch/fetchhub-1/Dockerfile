#
# Build: Cosmovisor
#
FROM golang:1.16 AS cosmovisor

ENV GOBIN=/go/bin
ENV GOPATH=/go
ENV CGO_ENABLED=0
ENV GOOS=linux

RUN go get github.com/cosmos/cosmos-sdk/cosmovisor/cmd/cosmovisor

#
# Build: Fetch
#
FROM golang:1.16 AS fetchd

ENV GOBIN=/go/bin
ENV GOPATH=/go
ENV CGO_ENABLED=0
ENV GOOS=linux


WORKDIR /fetchd
RUN git clone -b v0.8.7 https://github.com/fetchai/fetchd.git .
RUN go mod download
RUN make install

#
# Main
#
FROM alpine

ENV PACKAGES curl jq bind-tools expect

EXPOSE 1317
EXPOSE 26656
EXPOSE 26657

RUN apk add --update --no-cache $PACKAGES

# Copy the compiled binaires over.
COPY --from=cosmovisor /go/bin/cosmovisor /usr/bin/cosmovisor
COPY --from=fetchd /go/bin/fetchd /usr/bin/fetchd
