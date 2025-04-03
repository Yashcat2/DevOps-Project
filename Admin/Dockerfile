FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 5173

# For development mode
CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0"]

# For production mode (uncomment this and comment the above line when ready for production)
# RUN npm run build
# EXPOSE 80
# CMD ["npm", "run", "preview", "--", "--host", "0.0.0.0", "--port", "80"]
