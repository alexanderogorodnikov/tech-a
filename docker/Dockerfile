# Use the official NGINX base image
FROM nginx

# Copy the "hello-world" HTML file to the default NGINX web root directory
COPY hello-world.html /usr/share/nginx/html/index.html

# Expose port 80 for the NGINX web server
EXPOSE 80

# Command to start NGINX (this is the default CMD in the official NGINX image)
CMD ["nginx", "-g", "daemon off;"]
