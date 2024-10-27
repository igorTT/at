FROM nginx:alpine AS development
COPY packages/api-gateway/nginx.conf /etc/nginx/nginx.conf
COPY packages/api-gateway/conf.d/default.conf /etc/nginx/conf.d/default.conf
CMD ["nginx", "-g", "daemon off;"]

FROM nginx:alpine AS production
COPY packages/api-gateway/nginx.conf /etc/nginx/nginx.conf
COPY packages/api-gateway/conf.d/default.conf /etc/nginx/conf.d/default.conf
CMD ["nginx", "-g", "daemon off;"]
