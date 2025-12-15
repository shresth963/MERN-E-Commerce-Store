FROM node:18

WORKDIR /app

# Backend dependencies (ROOT level)
COPY package*.json ./
RUN npm install

# Backend source code
COPY . .

# Frontend build already copied by Jenkins (frontend/dist exists)
EXPOSE 5000

CMD ["node", "server.js"]
