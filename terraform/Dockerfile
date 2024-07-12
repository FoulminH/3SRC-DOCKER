# Manual install of glibc on alpine 3.20 image (111MB)

FROM debian:12 AS base
WORKDIR /usr/src
RUN apt update && apt install golang-go ca-certificates wget -y && \
    wget https://github.com/hashicorp/terraform/archive/refs/tags/v1.5.7.tar.gz -P /usr/src && \
    tar -xzvf v1.5.7.tar.gz -C /usr/src
WORKDIR /usr/src/terraform-1.5.7
RUN go generate . && go build

FROM alpine:3.20 as final
RUN apk update && apk add --no-cache wget && \
    wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub && \
    wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.34-r0/glibc-2.34-r0.apk && \
    wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.34-r0/glibc-bin-2.34-r0.apk && \
    apk add --force-overwrite --no-cache glibc-2.34-r0.apk glibc-bin-2.34-r0.apk && \
    rm /etc/apk/keys/sgerrand.rsa.pub && \
    rm glibc-2.34-r0.apk glibc-bin-2.34-r0.apk
COPY --from=base /usr/src/terraform-1.5.7/terraform /bin/
ENTRYPOINT ["/bin/terraform"]
CMD ["-help"]