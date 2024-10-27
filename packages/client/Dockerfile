# Base stage for shared dependencies
FROM node:20-alpine AS base
WORKDIR /app

# Development stage
FROM base AS development
# Install tools for debugging
RUN apk add --no-cache python3 make g++ netcat-openbsd curl
# Copy workspace configuration files
COPY package.json package-lock.json ./
COPY nx.json ./
COPY tsconfig.base.json ./
# Copy the project files
COPY . .
# Install all dependencies (including devDependencies) with legacy peer deps
RUN npm install --legacy-peer-deps
# Keep container running for development
CMD ["npm", "run", "client:serve"]

# Build stage
FROM base AS builder
# Copy workspace configuration files
COPY package.json package-lock.json ./
COPY nx.json ./
COPY tsconfig.base.json ./
# Copy the project files
COPY . .
# Install dependencies and build with legacy peer deps
RUN npm ci --legacy-peer-deps
RUN npx nx build client

# Production stage
FROM nginx:alpine AS production
# Copy the Nginx configuration file
COPY packages/client/nginx.conf /etc/nginx/conf.d/default.conf
# Copy the built client application files into the Nginx html directory
COPY --from=builder /app/dist/packages/client /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
