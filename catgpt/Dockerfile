FROM golang:1.21 as base_build

WORKDIR /app

# pre-copy/cache go.mod for pre-downloading dependencies and only redownloading them in subsequent builds if they change
COPY go.mod go.sum ./
RUN go mod download && go mod verify

COPY . .


RUN CGO_ENABLED=0 go build -o /docker-go

# RUN go build -v -o /usr/local/bin/app ./...



FROM gcr.io/distroless/static-debian12:latest-amd64

WORKDIR /

COPY --from=base_build /docker-go /docker-go



EXPOSE 8080
EXPOSE 9090

ENTRYPOINT ["/docker-go"]
