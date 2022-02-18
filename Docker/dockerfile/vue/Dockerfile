FROM node:14.15.0-alpine3.10 AS build
WORKDIR /app

COPY package.json package-lock.json ./
RUN npm install

ENV PATH="./node_modules/.bin:$PATH"
COPY . .
RUN npm run build
RUN ls -al /app/dist

# production stage
FROM nginx:stable-alpine as production-stage
COPY --from=build /app/dist/ /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
RUN ls -al /usr/share/nginx/html
CMD ["nginx", "-g", "daemon off;"]