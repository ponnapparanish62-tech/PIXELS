# Pixel Dash - static game served via nginx
FROM nginx:alpine

# Remove default nginx welcome page
RUN rm -rf /usr/share/nginx/html/*

# Copy our game into nginx's web root
COPY index.html /usr/share/nginx/html/index.html

# nginx listens on 80 by default - Azure App Service / Container Apps
# will map this automatically
EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
