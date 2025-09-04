FROM golang:1.24

COPY corporate-ca.crt /usr/local/share/ca-certificates/
RUN update-ca-certificates

WORKDIR /app

COPY go.mod go.sum ./

RUN if [ ! -f go.mod ]; then go mod init wongnok; fi
RUN go mod tidy || true
RUN go mod download || true

RUN go install github.com/pressly/goose/v3/cmd/goose@latest

COPY . .

EXPOSE 8080

CMD ["sh", "-c", "echo 'Running migrations...' && goose up && echo 'Starting Go server...' && exec go run cmd/server/main.go"]