FROM node:12 as BUILDER

ENV NODE_ENV=production

COPY . .

RUN yarn install \
  && yarn run prod

FROM nginx:1.20.0

USER root

COPY --from=BUILDER dist .
COPY . .

RUN chgrp -R 0 . \
 && chmod -R g=u .

USER 1001

ENTRYPOINT [ "/usr/bin/nginx", "-g", "daemon: off;" ]
