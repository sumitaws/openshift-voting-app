FROM node:16-slim

# Add curl for healthcheck
RUN apt-get update \
    && apt-get install -y --no-install-recommends curl \
    && rm -rf /var/lib/apt/lists/*

# Add Tini for proper init of signals
ENV TINI_VERSION v0.19.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini

WORKDIR /app

# Install nodemon globally
RUN npm install -g nodemon

# Create a user
RUN useradd -u 1001 -r -g 0 -d /app -s /sbin/nologin \
    -c "Default Application User" default && \
    chown -R 1001:0 /app && \
    chmod -R g+rw /app

USER 1001

# Copy package.json and package-lock.json if exists
COPY src/package*.json ./

# Install app dependencies
RUN npm install --production && npm cache clean --force

# Copy app source code
COPY src/. .

# Set environment variable
ENV PORT 8080

# Expose the application port
EXPOSE 8080

# Run the application
CMD ["/tini", "--", "node", "server.js"]
