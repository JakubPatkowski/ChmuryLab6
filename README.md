# ChmuryLab6
Kopiujemy repozytorium z Lab5:

    git clone https://github.com/JakubPatkowski/ChmuryLab5

Modyfikujemy Dockerfile:

    # syntax = docker/dockerfile:experimental

    FROM node:alpine AS builder

    ARG VERSION

    WORKDIR /app

    RUN apk add --no-cache git

    RUN --mount=type=ssh \
    git clone https://github.com/JakubPatkowski/ChmuryLab5

    ENV APP_VERSION=$VERSION

    FROM nginx:stable-alpine3.17-slim

    RUN apk add --no-cache npm curl

    WORKDIR /app

    COPY --from=builder /app/ChmuryLab5 .

    COPY --from=builder /app/ChmuryLab5/nginx.conf /etc/nginx/conf.d/default.conf

    EXPOSE 8080

    HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD curl -f http://localhost/ | | exit 1

    CMD ["node", "app.js"]

Budejemy obraz dockera:

    >docker buildx build -t lab6 .

Uruchamiamy obraz:

    docker run -p 8080:8080 lab6

Rezultat komendy run: 
    Aplikacja działa na http://localhost:8080