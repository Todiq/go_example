# syntax=docker/dockerfile:1

ARG OUTPUT="/bin/hello"

FROM --platform=$BUILDPLATFORM golang:1.26.4-alpine AS build

ARG OUTPUT
ARG TARGETOS
ARG TARGETARCH

WORKDIR /src

RUN --mount=type=bind,target=. \
    --mount=type=cache,target=/root/.cache/go-build \
    --mount=type=cache,target=/go/pkg \
    GOOS=$TARGETOS GOARCH=$TARGETARCH go build -o "${OUTPUT}" .

FROM scratch

ARG OUTPUT

COPY --from=build "${OUTPUT}" "${OUTPUT}"

USER 10001
# ARG and ENV are not usable, since no shell is present
# We pass the direct location of the binary
ENTRYPOINT [ "/bin/hello" ]

CMD [ "-help" ]