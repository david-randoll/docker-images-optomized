FROM node:23-alpine3.20 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --production
COPY . .
# use one of the following commands to build the app
RUN npx @vercel/ncc build app.js -o dist
# RUN npx esbuild app.js --bundle --platform=node --outfile=dist/index.js

FROM alpine:3.20
RUN apk add --no-cache nodejs && \
    addgroup --system --gid 1001 node && \
    adduser --system --uid 1001 node && \
    mkdir -p /app && \
    chown -R node:node /app
WORKDIR /app
COPY --from=builder --chown=node:node /app/dist/index.js .
USER node
EXPOSE 3000
ENV PORT=3000
CMD ["node", "index.js"]