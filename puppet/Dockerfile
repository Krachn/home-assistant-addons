FROM node:22-bookworm-slim

ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC

RUN apt-get update && apt-get install -y \
    chromium \
    fonts-noto-cjk \
    fonts-noto-color-emoji \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /app

# Copy package.json and package-lock.json first
COPY ha-puppet/package*.json ./

# Skip Puppeteer's Chrome download since we use system Chromium
ENV PUPPETEER_SKIP_DOWNLOAD=true

# Ensure a clean state before installing
RUN npm ci --omit=dev

# Work around npm arborist bug that skips sharp's libvips shared dependency
# (npm/cli#9238) — forces the actual .so file that sharp needs to install
RUN npm install --no-save @img/sharp-libvips-linux-x64

# Copy the rest of the project files
COPY ha-puppet/ .

# Run the application
CMD ["node", "http.js"]
