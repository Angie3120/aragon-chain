FROM golang:stretch as build-env

# Install minimum necessary dependencies
ENV PACKAGES curl make git libc-dev bash gcc
RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y $PACKAGES

# Set working directory for the build
WORKDIR /go/src/github.com/aragon/aragon-chain

# Add source files
COPY . .

# build Aragon Chain
RUN make build-aragon-chain-linux

# Final image
FROM golang:1.14 as final

WORKDIR /

RUN apt-get update

# Copy over binaries from the build-env
COPY --from=build-env /go/src/github.com/aragon/aragon-chain/build/aragonchaind /usr/bin/aragonchaind
COPY --from=build-env /go/src/github.com/aragon/aragon-chain/build/aragonchaincli /usr/bin/aragonchaincli
COPY --from=build-env /go/src/github.com/aragon/aragon-chain/scripts/start.sh /

EXPOSE 26656 26657 1317 8545 8546

# Run aragonchaind by default, omit entrypoint to ease using container with aragonchaincli
ENTRYPOINT ["/bin/bash", "-c"]