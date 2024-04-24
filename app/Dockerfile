FROM docker.io/library/node:21-alpine3.18 AS BUILDER

WORKDIR /app

# 把這四個放到/app
COPY package.json ./
COPY package-lock.json ./
COPY src/ ./
COPY tsconfig.json ./

# typescript編譯
RUN npm ci
RUN npm run build

FROM docker.io/library/node:21-alpine3.18 AS RUNNER

EXPOSE 3000

WORKDIR /app

COPY package.json ./
COPY package-lock.json ./

# 從上一步 拿編譯完的(at /app/dist) 放到./ ( == /app)
COPY --from=BUILDER /app/dist ./

RUN npm ci --omit=dev

CMD [ "node", "/app/app.js" ]