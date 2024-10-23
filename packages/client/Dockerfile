# Stage 1: Build the client application
FROM node:20-alpine AS builder

WORKDIR /app

# !WIP
# Copy workspace configuration files
COPY ../../package.json ../../package-lock.json ./
COPY ../../nx.json ./
COPY ../../tsconfig.base.json ./

# Copy the project files
COPY . ./packages/client/

# Install dependencies
RUN npm install

# Build the client application
RUN npx nx build client

# Stage 2: Serve the application with Nginx
FROM nginx:alpine

# Copy the Nginx configuration file
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy the built client application files into the Nginx html directory
COPY --from=builder /app/dist/packages/client /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
