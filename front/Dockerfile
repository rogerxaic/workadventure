# protobuf build
ARG MESSAGES_IMAGE
FROM ${MESSAGES_IMAGE} as messages


# STAGE 1
FROM node:12 as builder

WORKDIR /usr/src

COPY . .
COPY --from=messages /usr/src/generated ./src/Messages/generated

RUN yarn install 
RUN ./templater.sh
RUN yarn run build

# STAGE 2
FROM nginx:1.20.0

WORKDIR /usr/share/nginx/html

COPY --from=builder /usr/src/dist .
RUN rm iframe.html

RUN touch /run/nginx.pid \
  && chgrp -R 0 /var/cache/nginx /var/log/nginx /run/nginx.pid \
  && chmod -R g+rwx /var/cache/nginx /var/log/nginx /run/nginx.pid

RUN sed -i 's/80;/8080;/g' /etc/nginx/conf.d/default.conf \
  && sed -i 's/index.htm;/index.htm;\n        try_files $uri \/index.html;/' /etc/nginx/conf.d/default.conf

EXPOSE 8080

CMD [ "nginx", "-g", "daemon off;" ]
