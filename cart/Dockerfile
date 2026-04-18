# FROM node:20
# # this creates /app and move there
# WORKDIR /app
# COPY package.json .
# COPY *.js .
# RUN npm install
# ENV REDIS_HOST="redis" \
#     CATALOGUE_HOST="catalogue" \
#     CATALOGUE_PORT="8080"
# CMD ["node", "server.js"]


# Stage 1: Build dependencies
FROM node:20.20.2-alpine3.23 AS builder
WORKDIR /app
COPY package.json .
COPY *.js .
RUN npm install

# Stage 2: Runtime image
FROM node:20.20.2-alpine3.23
WORKDIR /app
EXPOSE 8080

# Copy built app from builder stage
COPY --from=builder /app /app

# Environment variables are handled via ConfigMap in Kubernetes
# ENV MONGO="true" \
#     MONGO_URL="mongodb://mongodb:27017/catalogue"

# Create non-root user
RUN addgroup -S roboshop && adduser -S roboshop -G roboshop
RUN chown -R roboshop:roboshop /app

USER roboshop

# Entrypoint and default command
ENTRYPOINT ["node"]
CMD ["server.js"]

