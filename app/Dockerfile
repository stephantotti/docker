FROM node:12-alpine
WORKDIR /app
RUN addgroup dev && adduser -S -G dev stephan

RUN apk update && apk add --no-cache python3 g++ make

COPY . .
RUN yarn install --production

RUN mkdir -p /etc/todos && chown -R stephan:dev /etc/todos
USER stephan
CMD ["node", "src/index.js"]

EXPOSE 3000
