FROM nginx:1.15

COPY nginx.conf /etc/nginx/nginx.conf

RUN usermod -u 1000 www-data

CMD ["nginx"]

EXPOSE 80