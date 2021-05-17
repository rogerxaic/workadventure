FROM thecodingmachine/nodejs:12-apache

USER root

COPY . .

RUN yarn install

ENV NODE_ENV=production
ENV STARTUP_COMMAND_1="yarn run build"
ENV APACHE_DOCUMENT_ROOT=dist/
RUN sudo a2enmod headers


RUN chgrp -R 0 . \\
 && chmod -R g=u .

USER 1001

ENTRYPOINT [ "yarn", "run", "prod" ]