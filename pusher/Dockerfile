# protobuf build
ARG MESSAGES_IMAGE
FROM ${MESSAGES_IMAGE} as messages

# typescript build
FROM node:14.15.4-buster-slim as builder
WORKDIR /usr/src
COPY yarn.lock package.json ./
RUN yarn install
COPY . .
COPY --from=messages /usr/src/generated src/Messages/generated
ENV NODE_ENV=production
RUN yarn run tsc

# final production image
FROM node:14.15.4-buster-slim
WORKDIR /usr/src
COPY yarn.lock package.json ./
COPY --from=builder /usr/src/dist /usr/src/dist
ENV NODE_ENV=production
RUN yarn install --production

USER 1001

EXPOSE 8080

CMD ["yarn", "run", "runprod"]
