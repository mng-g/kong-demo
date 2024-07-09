FROM golang AS build-stage

WORKDIR /app
COPY go.mod ./
#COPY go.sum ./
RUN go mod download
COPY *.go ./
#RUN go build -o /app
RUN CGO_ENABLED=0 GOOS=linux go build -o /api-hello-2

FROM ghcr.io/distroless/static
WORKDIR /
COPY --from=build-stage /api-hello-2 /api-hello-2
EXPOSE 9002
USER nonroot:nonroot

ENTRYPOINT ["/api-hello-2"]