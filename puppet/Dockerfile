FROM node:22-bookworm-slim

ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC

RUN apt-get update && apt-get install -y \
    chromium \
    fonts-noto-cjk \
    fonts-noto-color-emoji \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY ha-puppet/package*.json ./

ENV PUPPETEER_SKIP_DOWNLOAD=true

RUN npm ci --omit=dev

# Force-include optional deps for sharp, matching the exact
# locked version instead of installing a separate package at latest
RUN npm install --no-save --include=optional sharp

# Fail loudly at build time if the expected .so still isn't there,
# instead of only finding out at runtime
RUN find /app/node_modules/@img -maxdepth 2 -name 'libvips-cpp.so*' | tee /tmp/libvips-check.log && \
    test -s /tmp/libvips-check.log

COPY ha-puppet/ .

CMD ["node", "http.js"]
