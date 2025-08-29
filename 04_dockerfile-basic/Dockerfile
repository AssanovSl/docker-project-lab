# За основу взял образ c nginx
FROM nginx:alpine

# Копирую необходимые файлы
COPY ./server/server.c /app/server.c
COPY ./nginx/nginx.conf /etc/nginx/nginx.conf

# Устанавливаю рабочую дирикторию
WORKDIR /app

# Устанавливаю gcc и spawn-fcgi
RUN apk update && \
    apk add --no-cache  musl-dev gcc spawn-fcgi fcgi-dev && \
    mkdir -p /run/nginx && \
    gcc server.c -o my-server -lfcgi


# Используемый контейнером порт
EXPOSE 8080

# Запускаю spawn-fcgi и nginx
CMD spawn-fcgi -p 8080 /app/my-server && nginx -g 'daemon off;'