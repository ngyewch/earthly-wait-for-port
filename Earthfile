VERSION 0.8

build:
    FROM golang:1.24.1

    RUN apt-get update && apt-get install -y --no-install-recommends musl-dev musl-tools

    ARG VERSION=v1.0.8

    RUN git clone https://github.com/bitnami/wait-for-port.git
    WORKDIR wait-for-port
    RUN git checkout ${VERSION}
    RUN CGO_ENABLED=1 CC=musl-gcc go build --ldflags '-linkmode=external -extldflags=-static' -o wait-for-port ./

    SAVE ARTIFACT wait-for-port

docker:
    FROM scratch
    
    ARG VERSION=v1.0.8

    COPY (+build/wait-for-port --VERSION=${VERSION}) /usr/local/bin/
    
    ENTRYPOINT ["/usr/local/bin/wait-for-port"]
    
    SAVE IMAGE wait-for-port:${VERSION}
