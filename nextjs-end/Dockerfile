# -------------------------------
# Stage 1: Builder
# -------------------------------
FROM node:20-alpine AS builder

WORKDIR /app

# Install dependencies
COPY package*.json ./
RUN npm ci --omit=dev

# Copy app source
COPY . .

# Build Next.js app
RUN npm run build


# -------------------------------
# Stage 2: Production Runtime
# -------------------------------
FROM node:20-alpine AS runner

WORKDIR /app

# Copy only required files to final image
COPY --from=builder /app/package*.json ./
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/next.config.js ./next.config.js

# Remove npm cache (reduces image size)
RUN rm -rf /root/.npm

EXPOSE 3000

CMD ["npm", "start"]

