#Build
FROM node:18-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install --frozen-lockfile

COPY . .

ARG NEXT_PUBLIC_API_URL
ENV NEXT_PUBLIC_API_URL=$NEXT_PUBLIC_API_URL

RUN npm run build

#Prod
FROM node:18-alpine AS runner

WORKDIR /app

RUN addgroup -g 1001 nodegroup && \
    adduser -D -u 1001 -G nodegroup nodeuser

COPY --from=builder /app/.next ./.next
COPY --from=builder /app/package*.json ./

RUN npm install --production

ENV NODE_ENV=production
EXPOSE 3000

USER nodeuser

CMD ["npm", "start"]