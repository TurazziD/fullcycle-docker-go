# Estágio de build
FROM golang:alpine AS builder

WORKDIR /app

# Copiando o código fonte
COPY main.go .

# Inicializando um módulo Go
RUN go mod init fullcycle

# Compilando o código com flags para reduzir o tamanho
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o app .

# Estágio final
FROM scratch

# Copiando o binário compilado
COPY --from=builder /app/app /

# Executando o programa quando o container iniciar
CMD ["/app"]
