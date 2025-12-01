FROM node:18-alpine

WORKDIR /app

COPY package*.json ./

# Install only production dependencies (Node 18 ships with npm 10 which supports --omit=dev)
RUN npm install --omit=dev

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
