# syntax=docker/dockerfile:1
# CI build environment for the Atlantis fork.
# Used for lint, test, and go build jobs in GitLab CI.
# Docker image builds use the Kaniko executor directly — not this image.
#
# renovate: datasource=docker depName=golang versioning=docker
ARG GOLANG_TAG=1.25.4-alpine@sha256:d3f0cf7723f3429e3f9ed846243970b20a2de7bae6a5b66fc5914e228d831bbb

FROM golang:${GOLANG_TAG}

# renovate: datasource=github-releases depName=golangci/golangci-lint
ARG GOLANGCI_LINT_VERSION=1.64.4

RUN apk add --no-cache \
        bash \
        curl \
        git \
        ca-certificates \
        unzip \
        jq \
        aws-cli

# Install golangci-lint with checksum verification
RUN curl -fsSL "https://github.com/golangci/golangci-lint/releases/download/v${GOLANGCI_LINT_VERSION}/golangci-lint-${GOLANGCI_LINT_VERSION}-linux-amd64.tar.gz" -o golangci-lint.tar.gz && \
    curl -fsSL "https://github.com/golangci/golangci-lint/releases/download/v${GOLANGCI_LINT_VERSION}/golangci-lint-${GOLANGCI_LINT_VERSION}-checksums.txt" -o checksums.txt && \
    grep "golangci-lint-${GOLANGCI_LINT_VERSION}-linux-amd64.tar.gz" checksums.txt | sha256sum -c && \
    tar -C /usr/local/bin -xzf golangci-lint.tar.gz --strip-components=1 "golangci-lint-${GOLANGCI_LINT_VERSION}-linux-amd64/golangci-lint" && \
    rm golangci-lint.tar.gz checksums.txt

RUN mkdir -p /workspace
WORKDIR /workspace

CMD ["/bin/bash"]
