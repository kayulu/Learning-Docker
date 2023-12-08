FROM node

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

ARG DEFAULT_PORT=8000

ENV PORT ${DEFAULT_PORT}

ENV PORT 80

EXPOSE $PORT

# anonymous volume
# VOLUME [ "/app/feedback" ]

CMD [ "node", "server.js" ]