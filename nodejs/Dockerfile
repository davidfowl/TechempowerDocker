FROM node

WORKDIR /app

COPY /nodejs/handlers ./handlers
COPY /nodejs/*.js ./
COPY /nodejs/package.json ./

RUN npm install

ENTRYPOINT [ "node", "app.js" ]