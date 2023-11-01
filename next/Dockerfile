FROM node:20.9-alpine3.18 AS base

ENV LANG=C.UTF-8

WORKDIR /web

COPY --link package.json package-lock.json ./

RUN : \
&& yarn install \
&& apk add curl \
&& :

COPY --link next.config.js ./next.config.js

COPY --link app ./app

COPY --link components ./components

COPY --link public ./public

EXPOSE 3000

CMD ["npm", "run", "dev"]
