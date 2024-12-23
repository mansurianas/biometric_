# Stage 1: Build React app
FROM node:18 AS build

WORKDIR /react

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

# Stage 2: Use Nginx to serve the React app
FROM nginx:1.25

COPY --from=build  /react/build/ /usr/share/nginx/html
COPY ./nginx/nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 6000
EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
