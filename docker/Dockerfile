FROM golang:1.20.7 as builder

RUN apt-get update && apt-get install -y

WORKDIR /config
COPY . .

WORKDIR /go
RUN git clone https://github.com/multiversx/mx-chain-go.git && cd mx-chain-go && git checkout --force $(cat /config/binaryVersion)
WORKDIR /go/mx-chain-go

#MultiversX node
WORKDIR /go/mx-chain-go/cmd/node
RUN go build -v -ldflags="-X main.appVersion=$(git describe --git-dir /config/.git --tags --long --dirty)"
RUN cp /go/pkg/mod/github.com/multiversx/$(cat /go/mx-chain-go/go.mod | grep mx-chain-vm-v | sort -n | tail -n -1| awk -F '/' '{print$3}'| sed 's/ /@/g')/wasmer/libwasmer_linux_amd64.so /lib/libwasmer_linux_amd64.so
RUN cp /go/pkg/mod/github.com/multiversx/$(cat /go/mx-chain-go/go.mod | grep mx-chain-vm-go | sort -n | tail -n -1| awk -F '/' '{print$3}'| sed 's/ /@/g')/wasmer2/libvmexeccapi.so /lib/libvmexeccapi.so

WORKDIR /config
RUN cp -r * /go/mx-chain-go/cmd/node/config/

WORKDIR /go/mx-chain-go/cmd/node

# ===== SECOND STAGE ======
FROM ubuntu:22.04
RUN apt-get update -y && apt-get upgrade -y
COPY --from=builder "/go/mx-chain-go/cmd/node" "/go/mx-chain-go/cmd/node/"
COPY --from=builder "/lib/libwasmer_linux_amd64.so" "/lib/libwasmer_linux_amd64.so"
COPY --from=builder "/lib/libvmexeccapi.so" "/lib/libvmexeccapi.so"

WORKDIR /go/mx-chain-go/cmd/node/
EXPOSE 8080
ENTRYPOINT ["/go/mx-chain-go/cmd/node/node", "--rest-api-interface=:8080"]
