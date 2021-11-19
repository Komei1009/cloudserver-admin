FROM node:16.13.0
WORKDIR /app

RUN yarn global add @vue/cli
COPY app/package*.json /app
RUN npm install
CMD bash

COPY . .