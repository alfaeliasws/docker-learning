FROM golang:1.18-alpine

RUN mkdir /app
RUN addgroup -S alfagroup
RUN adduser -S -D -h /app alfauser alfagroup
USER alfauser

COPY main.go /app

EXPOSE 8080

CMD go run /app/main.go