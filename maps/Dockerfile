# STAGE 1
FROM node:12 as BUILDER

ENV NODE_ENV=production

COPY . .

RUN yarn install \
  && yarn run prod

# STAGE 2
FROM nginx:1.20.0

COPY --from=BUILDER dist /usr/share/nginx/html
COPY . /usr/share/nginx/html

RUN touch /run/nginx.pid \
  && chgrp -R 0 /var/cache/nginx /var/log/nginx /run/nginx.pid \
  && chmod -R g+rwx /var/cache/nginx /var/log/nginx /run/nginx.pid

RUN sed -i 's/80;/8080;/g' /etc/nginx/conf.d/default.conf

EXPOSE 8080

CMD [ "nginx", "-g", "daemon off;" ]
