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

# Force the linux-x64 sharp/libvips binaries explicitly, since npm ci's
# lockfile-only resolution appears to be missing this platform entirely
RUN npm install --no-save --os=linux --cpu=x64 sharp

# Fail the build loudly if it's still missing, rather than failing at runtime
RUN echo "--- contents of @img ---" && ls -la /app/node_modules/@img && \
    find /app/node_modules/@img -maxdepth 2 -name 'libvips-cpp.so*' | tee /tmp/libvips-check.log && \
    test -s /tmp/libvips-check.log

COPY ha-puppet/ .

CMD ["node", "http.js"]
