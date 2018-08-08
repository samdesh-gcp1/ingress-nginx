# Copyright 2018 The Kubernetes Authors. All rights reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM quay.io/kubernetes-ingress-controller/nginx-amd64:0.57

RUN clean-install \
  g++ \
  gcc \
  git \
  libc6-dev \
  make \
  wget \
  luarocks \
  pkg-config

ENV GOLANG_VERSION 1.10.3
ENV GO_ARCH        linux-amd64
ENV GOLANG_SHA     fa1b0e45d3b647c252f51f5e1204aba049cde4af177ef9f2181f43004f901035

RUN set -eux; \
  url="https://golang.org/dl/go${GOLANG_VERSION}.${GO_ARCH}.tar.gz"; \
  wget -O go.tgz "$url"; \
  echo "${GOLANG_SHA} *go.tgz" | sha256sum -c -; \
  tar -C /usr/local -xzf go.tgz; \
  rm go.tgz; \
  export PATH="/usr/local/go/bin:$PATH"; \
  go version

ENV GOPATH /go
ENV PATH $GOPATH/bin:/usr/local/go/bin:$PATH

RUN mkdir -p "$GOPATH/src" "$GOPATH/bin" && chmod -R 777 "$GOPATH"

WORKDIR $GOPATH

ENV RESTY_CLI_VERSION 0.22rc2
ENV RESTY_CLI_SHA     1ec64d204469a04da553c95eaafb2e5601a03acb8f26cc10ae16b15525228c12
RUN set -eux; \
  url="https://github.com/openresty/resty-cli/archive/v${RESTY_CLI_VERSION}.tar.gz"; \
  wget -O resty_cli.tgz "$url"; \
  echo "${RESTY_CLI_SHA} *resty_cli.tgz" | sha256sum -c -; \
  tar -C /tmp -xzf resty_cli.tgz; \
  rm resty_cli.tgz; \
  mv /tmp/resty-cli-${RESTY_CLI_VERSION}/bin/* /usr/local/bin/; \
  resty -V

RUN  luarocks install luacheck \
  && luarocks install busted 2.0.rc12

RUN  go get github.com/onsi/ginkgo/ginkgo \
  && go get golang.org/x/lint/golint