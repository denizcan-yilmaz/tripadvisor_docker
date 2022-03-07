FROM node:17-slim

RUN apt-get update \
    && apt-get install -y wget gnupg bash \
    && wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
    && sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list' \
    && apt-get update \
    && apt-get install -y google-chrome-stable fonts-ipafont-gothic fonts-wqy-zenhei fonts-thai-tlwg fonts-kacst fonts-freefont-ttf libxss1 \
      --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*

# https://github.com/Yelp/dumb-init A minimal init system for Linux containers
ARG DUMB_INIT_VERSION=1.2.5
ADD https://github.com/Yelp/dumb-init/releases/download/v${DUMB_INIT_VERSION}/dumb-init_${DUMB_INIT_VERSION}_x86_64 /usr/local/bin/dumb-init
RUN chmod +x /usr/local/bin/dumb-init

WORKDIR /app

# first npm install layer to cache in docker
COPY package*.json /app/
RUN npm i

COPY *.js /app/

ENTRYPOINT ["dumb-init", "--"]

ENV CITY="eskisehir"

CMD ["sh","-c","node index.js ${CITY}"]


