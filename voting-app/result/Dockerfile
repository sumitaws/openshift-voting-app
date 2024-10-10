FROM node:10-slim

# add curl for healthcheck
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Add Tini for proper init of signals
ENV TINI_VERSION v0.19.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini

WORKDIR /app

RUN npm install -g nodemon

# Create a user and set permissions
RUN useradd -u 1001 -r -g 0 -d /app -s /sbin/nologin \
    -c "Default Application User" default && \
    chown -R 1001:0 /app && \
    chmod -R g+rw /app

USER 1001

# Ensure the correct permissions on the copied files
COPY --chown=1001:0 src/package*.json ./
COPY --chown=1001:0 src/. .

RUN npm ci \
    && npm cache clean --force

ENV PORT 8080

EXPOSE 8080

CMD ["/tini", "--", "node", "server.js"]
