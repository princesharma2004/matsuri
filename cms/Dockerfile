# Stage 1: Building the code
FROM node:18-alpine AS builder

WORKDIR /usr/src/app

COPY package.json yarn.lock ./

RUN yarn install

COPY . .

# Set environment variable for build time
ARG NEXT_PUBLIC_API_URL
ENV NEXT_PUBLIC_API_URL=$NEXT_PUBLIC_API_URL

RUN yarn build

# Stage 2: Serve the production build
FROM node:18-alpine AS runner

WORKDIR /usr/src/app

ENV NODE_ENV production

COPY --from=builder /usr/src/app/next.config.js ./
COPY --from=builder /usr/src/app/public ./public
COPY --from=builder /usr/src/app/.next ./.next
COPY --from=builder /usr/src/app/node_modules ./node_modules
COPY --from=builder /usr/src/app/package.json ./package.json

EXPOSE 3000

CMD ["yarn", "start"]
