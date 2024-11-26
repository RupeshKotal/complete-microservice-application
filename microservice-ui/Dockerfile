# Stage 1: Build
FROM node:18 AS build
WORKDIR /app
COPY package.json .
RUN npm run install --production
COPY . .
RUN npm run build

# Stage 2 : Server with nginx
FROM nginx:stable-alpine AS production
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80

