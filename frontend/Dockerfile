# Use Node.js for building the app
FROM node:alpine AS build

# Set working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package.json package-lock.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application source code
COPY . .

# Build the application
RUN npm run build

# Use Nginx to serve the app
FROM nginx:alpine

# envsubst is provided by gettext
RUN apk add --no-cache gettext

# Copy built React app
COPY --from=build /app/dist /usr/share/nginx/html

# Copy Nginx templates (with ${BACKEND_URL})
COPY nginx.conf.template /etc/nginx/nginx.conf.https.template
COPY nginx.conf.http.template /etc/nginx/nginx.conf.http.template

# Expose HTTP + HTTPS
EXPOSE 80 443

# Substitute environment variables and start Nginx
CMD ["sh", "-c", "TEMPLATE=/etc/nginx/nginx.conf.${NGINX_CONFIG:-https}.template && envsubst '$BACKEND_URL $DOMAIN $CERT_NAME' < \"$TEMPLATE\" > /etc/nginx/nginx.conf && exec nginx -g 'daemon off;'"]
