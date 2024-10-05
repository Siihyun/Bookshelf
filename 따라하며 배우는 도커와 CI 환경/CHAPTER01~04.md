# Chapter 01 - 도커란?

도커 이미지를 통해 도커 컨테이너를 만듬. 즉 도커 컨테이너는 도커 이미지의 인스턴스다.

docker stop => 해당 프로세스에 SIGTERM 보냄 -> Graceful shutdown으로 하던 작업 마무리 후 종료

docker kill => 해당 프로세스에 SIGKILL을 보냄 -> 바로 종료

## Nginx로 react project build 해서 띄우기

### dockerfile

FROM node:16-alpine AS build
WORKDIR /app
COPY package\*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:alpine
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/dist /usr/share/nginx/html

CMD ["nginx", "-g", "daemon off;"]
