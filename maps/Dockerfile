# STAGE 1
FROM node:12 as BUILDER

ENV NODE_ENV=production

COPY . .

RUN npm install \
  && npm run prod

# STAGE 2
FROM nginx:1.20.0

WORKDIR /usr/share/nginx/html

COPY --from=BUILDER dist .
COPY . .

RUN touch /run/nginx.pid \
  && chgrp -R 0 /var/cache/nginx /var/log/nginx /run/nginx.pid /usr/share/nginx/html \
  && chmod -R g+rwx /var/cache/nginx /var/log/nginx /run/nginx.pid /usr/share/nginx/html

COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 8080

CMD [ "nginx", "-g", "daemon off;" ]
