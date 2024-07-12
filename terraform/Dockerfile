FROM debian:12 AS base
WORKDIR /usr/src
RUN apt update && apt install golang-go ca-certificates wget -y && \
    wget https://github.com/hashicorp/terraform/archive/refs/tags/v1.5.7.tar.gz -P /usr/src && \
    tar -xzvf v1.5.7.tar.gz -C /usr/src
WORKDIR /usr/src/terraform-1.5.7
RUN go generate . && go build

FROM alpine:3 AS final
COPY --from=base /usr/src/terraform-1.5.7/terraform ./bin
ENTRYPOINT ["/bin/terraform"]
CMD ["-help"]