FROM nginx:alpine

COPY ./nginx-custom.conf /etc/nginx/nginx.conf

WORKDIR /usr/share/nginx/html
COPY dist/ .
