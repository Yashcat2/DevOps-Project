# Stage 1: Build the React App
FROM node:18-alpine AS builder

# Set working directory
WORKDIR /app

# Copy only package.json and package-lock.json for caching dependencies
COPY package.json package-lock.json ./

# Install dependencies
RUN npm ci --silent

# Copy the rest of the application files
COPY . .

# Build the React app
RUN npm run build

# Stage 2: Serve with Nginx
FROM nginx:1.25.2-alpine

# Set working directory
WORKDIR /usr/share/nginx/html

# Remove default Nginx static content
RUN rm -rf ./*

# Copy build files from builder stage
COPY --from=builder /app/build .

# Copy custom Nginx configuration (for React routing support)
COPY nginx/nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]