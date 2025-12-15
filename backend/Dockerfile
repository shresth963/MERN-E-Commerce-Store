FROM node:18

WORKDIR /app

COPY . .

RUN npm install

ENV NODE_ENV=production
EXPOSE 5000

CMD ["node", "backend/index.js"]
