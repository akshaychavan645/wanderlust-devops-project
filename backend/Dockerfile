FROM node:21 AS deps
WORKDIR /app
COPY package*.json ./
RUN --mount=type=cache,target=/root/.npm npm ci --no-audit --no-fund

FROM deps AS ci
ENV CI=true
COPY . .
RUN npx jest tests/unit --runInBand --watchAll=false --coverage=false

FROM node:21-slim
WORKDIR /app
COPY --from=ci /app .
EXPOSE 8080
CMD ["npm", "start"]
