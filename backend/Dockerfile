FROM node:18

WORKDIR /app

# Install backend dependencies
COPY backend/package*.json ./
RUN npm install

# Copy backend source
COPY backend .

# 🔥 COPY FRONTEND BUILD (THIS WAS MISSING)
COPY frontend/dist frontend/dist

EXPOSE 5000

CMD ["node", "server.js"]
