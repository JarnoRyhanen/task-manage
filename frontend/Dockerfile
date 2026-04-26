ARG NODE_VERSION=24-alpine
ARG VITE_API_URL

FROM node:${NODE_VERSION} AS build
WORKDIR /app

ARG VITE_API_URL
ENV VITE_API_URL=${VITE_API_URL}

RUN test -n "$VITE_API_URL" || (echo "VITE_API_URL build arg is required" && exit 1)

COPY package*.json ./
RUN --mount=type=cache,target=/root/.npm npm install --legacy-peer-deps

COPY . .
RUN npm run build

# Serve static assets with Nginx.
FROM nginx:stable-alpine AS production
COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]