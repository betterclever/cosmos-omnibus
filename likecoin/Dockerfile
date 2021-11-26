FROM likecoin/likecoin-chain:fotan-1.1 as project

FROM debian:buster as base

RUN apt-get update && \
  apt-get install --no-install-recommends --assume-yes ca-certificates curl wget file unzip gnupg2 jq && \
  apt-get clean

FROM base as aws

RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
RUN unzip awscliv2.zip -d /usr/src

FROM base
LABEL org.opencontainers.image.source https://github.com/ovrclk/cosmos-omnibus


ENV PROJECT=likecoin
ENV PROJECT_BIN=liked
ENV PROJECT_DIR=.liked
ENV PROJECT_CMD="liked start"
ENV VERSION="fotan-1.1"
ENV REPOSITORY="https://github.com/likecoin/likecoin-chain"
ENV NAMESPACE=LIKECOIN
ENV WASMVM_VERSION=main

ENV MONIKER=my-omnibus-node

EXPOSE 26656 \
       26657 \
       1317  \
       9090  \
       8080

COPY --from=project /usr/bin/$PROJECT_BIN /bin/$PROJECT_BIN
COPY --from=aws /usr/src/aws /usr/src/aws
RUN /usr/src/aws/install --bin-dir /usr/bin
ADD https://raw.githubusercontent.com/CosmWasm/wasmvm/$WASMVM_VERSION/api/libwasmvm.so /lib/libwasmvm.so

COPY ./run.sh ./snapshot.sh /usr/bin/
RUN chmod +x /usr/bin/run.sh /usr/bin/snapshot.sh
ENTRYPOINT ["run.sh"]

CMD $PROJECT_CMD
