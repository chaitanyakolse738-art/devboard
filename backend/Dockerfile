# Stage 1: Build
FROM golang:1.22.4-alpine AS builder

WORKDIR /app

# Download deps first for better layer caching.
COPY go.mod go.sum ./
RUN go mod download

# Build the binary.
COPY . .
RUN CGO_ENABLED=0 go build -o devboard-backend .

# Stage 2: Runtime
FROM alpine:latest

WORKDIR /app

# Copy binary from builder stage.
COPY --from=builder /app/devboard-backend .

# Run as a non-root user.
RUN adduser -D -u 10001 app && chown -R app:app /app
USER app

EXPOSE 8080
ENTRYPOINT ["./devboard-backend"]
