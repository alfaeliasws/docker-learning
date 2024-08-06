FROM golang:1.18-alpine
WORKDIR /app
COPY main.go /app

EXPOSE 8080

# It should be like this not like before (CMD go run app/main.go) because app is the base directory in WORKDIR
CMD go run main.go