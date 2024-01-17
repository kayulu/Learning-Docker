FROM node

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

ENV MONGODB_USER=kayhan

ENV MONGODB_PASSWORD=secret

CMD [ "node", "app.js" ]