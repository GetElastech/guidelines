# Go Primer

## Learn basic go using flow-go

This document helps you to learn the basic skills to write, build, test and ship flow-go code in production.

It is an example project how to containerize and extract a microservice out of flow-go. It is a basic refactoring step.

You need to have basic `git` and `Linux` skills to do this.

## Steps

### You need to create a new Go repository that can be checked to GitHub.

- It requires a README.md file that describes how to use it.
- It requires a license matching the license of the source project.
- It requires a Makefile.

### Clone base codebase
Check out this branch https://github.com/GetElastech/flow-go/tree/feature/observer-prc-0427e

```
cd ~
git clone https://github.com/GetElastech/flow-go.git flow-go
cd ~/flow-go
git checkout feature/observer-prc-0427e
```

### Clone base codebase
Copy all the code in the following directory to your repository
```
mkdir ~/observer
cp -R ~/flow-go/ ~/observer/<dep>/
cp -R ~/flow-go/cmd/observer/* ~/observer/<src>/
```

### Initialize a Go module

```
cd ~/observer/<src>/
go mod init github.com/GetElastech/flow-observer/m/v2
```

### Ensure that we download dependencies

Make sure that the code builds by downloading all the dependencies Update the new go.mod file to be something like this:

```
module github.com/GetElastech/flow-observer/m/v2

go 1.16

require (
    github.com/HdrHistogram/hdrhistogram-go v1.1.2 // indirect
    github.com/desertbit/timer v0.0.0-20180107155436-c41aec40b27f // indirect
    github.com/libp2p/go-libp2p-core v0.11.0
    github.com/libp2p/go-libp2p-pubsub v0.6.0
    github.com/onflow/flow-go v0.18.0
    github.com/onflow/flow-go/crypto v0.24.3
    github.com/onflow/flow/protobuf/go/flow v0.2.5
    github.com/rs/zerolog v1.19.0
    github.com/spf13/pflag v1.0.5
    google.golang.org/grpc v1.44.0
)

replace github.com/onflow/flow-go => ../dep

replace mellium.im/sasl => github.com/mellium/sasl v0.2.1

```

### Optional download of all dependencies

Fetch the latest files. This step may be dangerous as it can break the existing behavior. Omit it if errors occur. This can be used later when the code is changed.

```
go mod tidy
```

### Create a Docker image

Create a Dockerfile to build the project. Example:

```
FROM golang:1.16

COPY <dep> /app/dep
WORKDIR /app/dep
RUN go mod download

COPY <src>/go.mod src/go.sum /app/src/
WORKDIR /app/src
RUN go mod download

COPY <src> /app/src

WORKDIR /app/dep
RUN apt update; apt install -y cmake
# This may be required for some projects
# RUN make install-tools
# This generates code marked by `go:build relic` and `+build relic`. See `combined_verifier_v3.go`.
RUN go mod download github.com/onflow/flow-go/crypto@v0.24.3
RUN cd $GOPATH/pkg/mod/github.com/onflow/flow-go/crypto@v0.24.3 && go generate

WORKDIR /app/src
RUN go generate -v
RUN go build -tags=relic -v -o /app/observer ./...

CMD ["/app/observer"]
```

Build
```
docker build -t <avdfd> .
```

Run
```
docker run -i -t <avdfd> bash -c '/observer || sleep 1000'
```

### Write a make file

Create the following make file entries
- `make install` should build the project into a local Docker image.
- `make start` should start the service in a docker container that exposes ports
- `make stop` should stop the container.
- `make test` should run all the unit tests in the image.

### Discussion. Additional steps

- The least amount of code is written the better it is.
- Update this documentation to reflect what you did that works.

### Reference

Example: https://github.com/onflow/full-observer-node-example
