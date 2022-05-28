FROM node:14-alpine as builder

ENV NODE_ENV build

WORKDIR /home/node

COPY . /home/node

RUN npm ci \
    && npm run build \
    && npm prune --production

# ---

FROM node:14-alpine

ENV NODE_ENV production

RUN npm install pm2@latest -g

USER node
WORKDIR /home/node

COPY --from=builder /home/node/.env.prd /home/node/.env
COPY --from=builder /home/node/package*.json /home/node/
COPY --from=builder /home/node/node_modules/ /home/node/node_modules/
COPY --from=builder /home/node/dist/ /home/node/dist/
COPY --from=builder /home/node/ecosystem.config.js /home/node/ecosystem.config.js

CMD ["pm2-runtime", "ecosystem.config.js"]
