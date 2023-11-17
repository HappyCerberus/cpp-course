FROM gcc:13.2
ARG TARGETARCH

RUN set -ex && \
    apt-get update && \
    apt-get install -y gpg git lsb-release wget software-properties-common

ENV CC=/usr/local/bin/gcc

RUN wget https://ftp.gnu.org/gnu/gdb/gdb-13.2.tar.gz && \
	tar -xf gdb-13.2.tar.gz -C /tmp && rm gdb-13.2.tar.gz && \
	cd /tmp/gdb-13.2 && \
	./configure --prefix=/usr/local && make && make install && \
	rm -rf /tmp/gdb-13.2

RUN wget https://go.dev/dl/go1.21.0.linux-${TARGETARCH}.tar.gz && \
    tar -xf go1.21.0.linux-${TARGETARCH}.tar.gz -C /usr/local && \
    rm go1.21.0.linux-${TARGETARCH}.tar.gz

ENV PATH="${PATH}:/usr/local/go/bin"

RUN set -ex && \
    GOBIN=/usr/local/go/bin go install github.com/bazelbuild/bazelisk@latest && \
    GOBIN=/usr/local/go/bin go install github.com/bazelbuild/buildtools/buildifier@latest && \
    ln -s /usr/local/go/bin/bazelisk /usr/local/bin/bazel

RUN set -ex &&\
    wget https://apt.llvm.org/llvm.sh &&\
    chmod +x llvm.sh &&\
    ./llvm.sh 17 all || echo ""

# For whatever reason we have to manually update and run the llvm script again.
RUN set -ex &&\
    apt-get update &&\
    ./llvm.sh 17 all