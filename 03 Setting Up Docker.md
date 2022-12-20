```Dockerfile
# Dockerfile

FROM node:18-alpine
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
EXPOSE 5000
CMD [ "node", "dist/server.js" ]
```

```sh
# .dockerignore

node_modules
*.log
git
.gitignore
Dockerfile
.dockerignore
src
.prettierrc
tsconfig.json
```

```yml
// docker-compose.yml

version: '3.9'
services:
  db:
    image: postgres
    ports:
      - 5432:5432
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: 123
      POSTGRES_DB: node
  redis:
    image: redis
    ports:
      - 6379:6379
```

```json
// package.json

"build-image": "npm run build && docker compose build"
```

```
docker compose up -d
docker compose down -d
```
