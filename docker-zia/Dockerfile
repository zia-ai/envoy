FROM debian:9 AS build-env

RUN apt-get update
RUN apt-get install -y git pkg-config zip g++ zlib1g-dev unzip python build-essential autoconf ninja-build libtool cmake wget
RUN wget https://github.com/bazelbuild/bazel/releases/download/0.24.1/bazel_0.24.1-linux-x86_64.deb
RUN apt-get install -y ./bazel_0.24.1-linux-x86_64.deb
ADD . /src
WORKDIR /src
RUN bazel build -c opt --copt="-O3" //source/exe:envoy-static
RUN strip /src/bazel-bin/source/exe/envoy-static

FROM debian:9
WORKDIR /app
COPY --from=build-env /src/bazel-bin/source/exe/envoy-static /app/envoy
