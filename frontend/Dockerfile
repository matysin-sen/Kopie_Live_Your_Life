FROM node:22-alpine AS build
WORKDIR /app
COPY package.json ./
RUN npm install --legacy-peer-deps
COPY . .
ARG VITE_API_BASE_URL
ENV VITE_API_BASE_URL=$VITE_API_BASE_URL
RUN npx vite build

FROM nginxinc/nginx-unprivileged:alpine AS runtime
# chown zorgt dat de eigenaar aangepast wordt --> eigenaar 101 is de standaard voor unprivileged maakt groep 0 aan. root rechten hiermee geen probleem
COPY --chown=101:0 --from=build /app/dist /usr/share/nginx/html
COPY --chown=101:0 nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 8080
