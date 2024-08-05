# BasePathAngular



### this is a dockerfile 

# Stage 1: Build the Angular application
FROM node:14 AS build
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
RUN npm run build -- --output-path=dist --base-href=/app/

# Stage 2: Serve the application with Nginx
FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html/app
COPY default.conf /etc/nginx/conf.d/default.conf 




### this is a default.conf

server {
    listen 80;
    server_name localhost;

    location /app/ {
        alias /usr/share/nginx/html/app/;
        try_files $uri $uri/ /app/index.html;
    }

    location / {
        root /usr/share/nginx/html;
        try_files $uri /index.html;
    }
}


### now you can deploy your angular app with subpath -> example.com/app/
