# ─────────────────────────────────────────────
# ETAPA: dev
# ─────────────────────────────────────────────
FROM node:19-alpine3.15 as dev
WORKDIR /app
COPY package.json ./
RUN yarn install
CMD [ "yarn","start:dev" ]


# ─────────────────────────────────────────────
# ETAPA: dev-deps
# ─────────────────────────────────────────────
FROM node:19-alpine3.15 as dev-deps
WORKDIR /app
COPY package.json package.json
RUN yarn install --frozen-lockfile


# ─────────────────────────────────────────────
# ETAPA: builder
# ─────────────────────────────────────────────
FROM node:19-alpine3.15 as builder
WORKDIR /app
COPY --from=dev-deps /app/node_modules ./node_modules
COPY . .
RUN yarn build


# ─────────────────────────────────────────────
# ETAPA: prod-deps
# ─────────────────────────────────────────────
FROM node:19-alpine3.15 as prod-deps
WORKDIR /app
COPY package.json package.json
RUN yarn install --prod --frozen-lockfile


# ─────────────────────────────────────────────
# ETAPA: prod
# ─────────────────────────────────────────────
FROM node:19-alpine3.15 as prod
EXPOSE 3000
WORKDIR /app
ENV APP_VERSION=${APP_VERSION}
COPY --from=prod-deps /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist

CMD [ "node","dist/main.js"]