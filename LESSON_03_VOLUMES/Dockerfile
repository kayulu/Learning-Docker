FROM node

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

# anonymous volume
# VOLUME [ "/app/feedback" ]

CMD [ "node", "server.js" ]