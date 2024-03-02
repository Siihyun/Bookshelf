# Chapter 01 - 도커 엔진

## 2.1 도커 이미지와 컨테이너

- 도커 엔진에서 사용하는 기본 단위는 이미지와 컨테이너이며, 이 두가지가 도커 엔진의 핵심
- 도커 이미지는 컨테이너를 생성할 때 필요한 요소이며, 가상 머신을 생성할 때 사용하는 iso 파일과 비슷한 개념
- 이미지는 여러 개의 계층으로 된 바이너리 파일로 존재

## 2.2 도커 컨테이너 다루기

- 일반적으로 도커는 컨테이너에 172.17.0.x IP를 순차적으로 할당
- 아무 설정도 하지 않으면 외부에서 접근 불가능, 포트바인딩을 해주면 접근 가능해짐

```bash
docker run -i -t --name myserver -p 80:80 ubuntu:14.04
```

- 도커 주요 명령어

```bash
# 돌아가는 docker container 확인
$ docker ps

# 도커 컨테이너 종료
$ docker kill ps-id

#tagname과 함께 빌드
docker built -t [tagname] [path]
$ docker built -t docker-test .

#docker 실행
docker run -d -p [external-port]:[internal-port] image-name
$ docker run -d -p 8080:3030 docker-test

$ docker run \
   -i \                                # 컨테이너에 키보드 입력이 필요한경우
   -t \                                # 컨테이너에 TTY할당하여 터미널 이용이 필요한 경우
   --rm \                              # 컨테이너 실행 종료후 자동 삭제가 필요할때
   -d \                                # 백그라운드로 실행하고 싶을 때
   --name hello-world \                # 이름을 지정하고 싶을때
   -p 80:80 \                          # 포트 바인딩을 하고 싶을 때
   -v /opt/example:/example \          # 볼륨 바인딩을 하고 싶을 때
   sihyun/hello-world:latest \         # 실행할 이미지 이름
   my-command                          # 컨테이너 내에서 실행할 명령어

# 중지된 컨테이너 종료
$ docker container prune

# docker 이미지 조회
$ docker images

```

## 2.3 도커 이미지

- 도커는 기본적으로 도커 허브라는 곳에서 이미지를 내려받음
- 이미지는 여러개의 layer로 구성됨
- 각 layer는 캐싱됨

## 2.4 DockerFile

- DockerFile을 이용하면 이미지를 직접 커밋하는 번거로운 과정을 하지 않고도 이미지를 만들 수 있음
- DOckerFile은 컨테이너 안에서 수행해야 할 작업을 명시

nextjs standalone dockerfile 예시

```bash
# FROM: 생성할 이미지의 베이스가 될 이미지를 말함
# alpine을 base 이미지로 사용
FROM node:18-alpine AS base

FROM base AS deps

# RUN: 컨테이너 내부에서 명령어를 실행
RUN apk add --no-cache libc6-compat

# WORKDIR: 명령어를 실행할 디렉터리를 명시
WORKDIR /app

# COPY: 복사
COPY package.json yarn.lock* package-lock.json* pnpm-lock.yaml* ./
RUN \
  if [ -f yarn.lock ]; then yarn --frozen-lockfile; \
  elif [ -f package-lock.json ]; then npm ci; \
  elif [ -f pnpm-lock.yaml ]; then corepack enable pnpm && pnpm i --frozen-lockfile; \
  else echo "Lockfile not found." && exit 1; \
  fi

FROM base AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .

RUN \
  if [ -f yarn.lock ]; then yarn run build; \
  elif [ -f package-lock.json ]; then npm run build; \
  elif [ -f pnpm-lock.yaml ]; then corepack enable pnpm && pnpm run build; \
  else echo "Lockfile not found." && exit 1; \
  fi

FROM base AS runner
WORKDIR /app

# 컨테이너 환경변수 생성. printenv 옵션으로 켜볼 수 있음
ENV NODE_ENV production
# Uncomment the following line in case you want to disable telemetry during runtime.
# ENV NEXT_TELEMETRY_DISABLED 1

# group ID: 1001
RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs

COPY --from=builder /app/public ./public

# Set the correct permission for prerender cache
RUN mkdir .next
RUN chown nextjs:nodejs .next

# Automatically leverage output traces to reduce image size
# https://nextjs.org/docs/advanced-features/output-file-tracing
COPY --from=builder --chown=nextjs:nodejs /app/.next/standalone ./
COPY --from=builder --chown=nextjs:nodejs /app/.next/static ./.next/static

USER nextjs

# EXPOSE: Dockerfile의 빌드로 생성된 이미지에서 노출할 포트를 설정
EXPOSE 3000

# ENV: 컨테이너 내의 환경변수 값 설정
ENV PORT 3000
ENV HOSTNAME "0.0.0.0"

# CMD: 컨테이너가 시작될 때마다 실행할 명령어를 설정. Dockerfile 내에서 한번만 설정가능
CMD ["node", "server.js"]
```

- 빌드 명령어는 Dockerfile에 기록된대로 컨테이너를 실행한 뒤 완성된 이미지를 만듬
- Dockerfile에서 명령어 한 줄이 실행될 때마다 이전 Step에서 생성된 이미지에 의해 새로운 컨테이너가 생성되며, 해당 라인을 수행하고 다시 새로운 이미지 레이어로 저장됨
- 따라서 이미지 빌드가 완료되면 Dockerfile의 명령어 줄 수 만큼의 레이어가 존재하게 됨
  -> 변경되지 않은 라인은 cache로 활용
