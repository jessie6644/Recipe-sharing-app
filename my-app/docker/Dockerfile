# https://dev.to/karanpratapsingh/dockerize-your-react-app-4j2e
FROM node:current-alpine3.15 AS build
ENV NODE_ENV production
# Add a work directory
WORKDIR /app
ENV PATH /app/node_modules/.bin:$PATH
COPY package.json ./
COPY package-lock.json ./
RUN npm ci --silent
COPY . ./
RUN npm run build

# Bundle static assets with nginx
FROM nginx:1.21.6-alpine as production
ENV NODE_ENV production
# Copy built assets from builder
COPY --from=build /app/build /usr/share/nginx/html
# Add your nginx.conf
COPY docker/nginx.conf /etc/nginx/conf.d/default.conf
# Expose port
EXPOSE 80
# Start nginx
CMD ["nginx", "-g", "daemon off;"]