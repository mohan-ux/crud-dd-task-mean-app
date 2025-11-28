# Frontend Dockerfile for Angular App
# Stage 1: Build the Angular application
FROM node:18-alpine AS build

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies and fix @types/node version for Angular 15 compatibility
RUN npm install && npm install @types/node@18.19.0 --save-dev

# Copy application code
COPY . .

# Build the application for production
RUN npm run build -- --configuration=production

# Stage 2: Serve with Nginx
FROM nginx:alpine

# Copy the built application from build stage
COPY --from=build /app/dist/angular-15-crud /usr/share/nginx/html

# Copy custom nginx configuration
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
