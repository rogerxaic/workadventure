FROM thecodingmachine/nodejs:12-apache

ENV NODE_ENV=production
ENV APACHE_DOCUMENT_ROOT=dist/

USER root

COPY . .

RUN yarn install


RUN sudo a2enmod headers

RUN yarn run prod

RUN chgrp -R 0 . \
 && chmod -R g=u .

USER 1001
