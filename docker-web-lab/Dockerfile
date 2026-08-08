FROM nginx:alpine

RUN apk add --no-cache curl

ENV APP_NAME="docker-web-lab"

COPY app/index.html /usr/share/nginx/html/index.html

HEALTHCHECK --interval=30s --timeout=3s --retries=3 CMD curl -f http://localhost/ || exit 1
