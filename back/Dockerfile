# protobuf build
FROM image-registry.openshift-image-registry.svc:5000/roger-sandbox/messages as builder

# typescript build
FROM node:14.15.4-buster-slim as builder2
WORKDIR /usr/src
COPY yarn.lock package.json ./
RUN yarn install
COPY . .
COPY --from=builder /usr/src/generated src/Messages/generated
ENV NODE_ENV=production
RUN yarn run tsc

# final production image
FROM node:14.15.4-buster-slim
WORKDIR /usr/src
COPY yarn.lock package.json ./
COPY --from=builder2 /usr/src/dist /usr/src/dist
ENV NODE_ENV=production
RUN yarn install --production

USER 1001

EXPOSE 8080
EXPOSE 50051

CMD ["yarn", "run", "runprod"]
