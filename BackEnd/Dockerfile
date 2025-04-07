# Use an official Node.js runtime as a parent image
FROM node:18

# Set the working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json to the working directory
COPY package*.json ./

# Install dependencies
RUN npm ci --silent

# Copy the rest of the application
COPY . .

# Expose the port that the app runs on
EXPOSE 4000

# Start the server
CMD ["node", "index.js"]