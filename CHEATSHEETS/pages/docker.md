---
title: 'Docker'
tags:
  - cheatsheet
  - docker
  - containers
---

# Docker Cheatsheet (Build + Run + Debug)

## Inventory + State

- `docker ps -a --no-trunc`
- `docker images --digests | head`
- `docker system df`

## Build

- Build with explicit tag:
  - `docker build -t <name>:<tag> .`
- Show build output without cache:
  - `docker build --no-cache -t <name>:<tag> .`

## Run

- Run interactively:
  - `docker run --rm -it <image> sh`
- Map port:
  - `docker run --rm -p 8080:8080 <image>`
- Env file (avoid printing secrets):
  - `docker run --rm --env-file .env <image>`

## Logs + Inspect

- `docker logs --tail 200 <container>`
- `docker logs -f <container>`
- `docker inspect <container> | head -n 80`

## Exec + Network

- `docker exec -it <container> sh`
- `docker network ls`
- `docker network inspect <net> | head -n 80`

## Cleanup (Be Careful)

- Remove stopped containers:
  - `docker container prune -f`
- Remove unused images:
  - `docker image prune -f`
- Remove unused volumes (danger):
  - `docker volume prune -f`

