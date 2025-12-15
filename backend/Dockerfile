FROM node:18

WORKDIR /app

COPY . .

WORKDIR /app/backend
RUN npm install

ENV NODE_ENV=production
EXPOSE 5000

CMD ["node", "index.js"]
