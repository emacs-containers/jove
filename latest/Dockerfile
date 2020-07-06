# Need full buster for build: buster-slim cannot find groff driver (ascii).

FROM debian:buster AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
      ca-certificates curl \
      groff make gcc libc-dev libncurses-dev \
 && rm -rf /var/lib/apt/lists/*
WORKDIR /build
COPY checksum checksum
RUN curl --fail --silent --show-error -o jove.tgz ftp://ftp.cs.toronto.edu/cs/ftp/pub/hugh/jove-dev/experimental/jove4.16.0.74.tgz
RUN sha256sum jove.tgz && sha256sum -c checksum
RUN mkdir jove && tar -C jove --strip-components 1 -xf jove.tgz
WORKDIR /build/jove
RUN make
RUN mkdir /usr/local/man/man1
RUN make install

FROM debian:buster-slim
RUN apt-get update && apt-get -y --no-install-recommends install \
      lsb-base \
 && rm -rf /var/lib/apt/lists/*
COPY --from=build /usr/local/ /usr/local/
CMD ["jove"]
