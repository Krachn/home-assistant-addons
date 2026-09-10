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

RUN npm install --no-save --os=linux --cpu=x64 sharp

# Corrected: no depth limit, and print the whole package contents too
RUN echo "--- sharp-libvips-linux-x64 contents ---" && \
    find /app/node_modules/@img/sharp-libvips-linux-x64 -type f && \
    echo "--- looking for the .so file ---" && \
    find /app/node_modules/@img -name 'libvips-cpp.so*'

COPY ha-puppet/ .

CMD ["node", "http.js"]
