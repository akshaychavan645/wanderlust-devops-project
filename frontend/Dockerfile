FROM node:21 AS deps
WORKDIR /app
COPY package*.json ./
RUN --mount=type=cache,target=/root/.npm npm ci --no-audit --no-fund

FROM deps AS ci
ENV CI=true
COPY . .
RUN npm run lint && npm run build

FROM node:21-slim
WORKDIR /app
COPY --from=ci /app .
EXPOSE 5173
CMD ["npm", "run", "dev", "--", "--host"]
