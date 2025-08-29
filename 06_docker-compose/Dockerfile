# За основу взял образ
FROM alpine:3.18

# Копирую необходимые файлы
COPY ./server/server.c /app/server.c
COPY ./nginx/nginx.conf /etc/nginx/nginx.conf

# Создаю пользователя
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Устанавливаю рабочую дирикторию
WORKDIR /app

# Устанавливаю gcc и spawn-fcgi
RUN apk update && \
    apk add --no-cache nginx musl-dev gcc spawn-fcgi fcgi-dev curl && \
    gcc server.c -o my-server -lfcgi && \
    mkdir -p /var/lib/nginx/tmp && \
    mkdir -p /var/lib/nginx/logs && \
    mkdir -p /run/nginx && \
    chown -R appuser:appgroup /var/lib/nginx /var/log/nginx /run/nginx

# Указываю созданного пользователя
USER appuser

# Используемый контейнером порт
EXPOSE 8080

# HEALTHCHECK для Dockle
HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 \
CMD curl -f http://localhost/status || exit 1

# Запускаем сервисы
CMD ["sh", "-c", "spawn-fcgi -p 8080 /app/my-server && nginx -g 'daemon off;'"]