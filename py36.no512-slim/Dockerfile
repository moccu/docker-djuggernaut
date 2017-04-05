FROM python:3.6-slim

ENV NODE_VERSION=5.12.0 PHANTOMJS_VERSION=2.1.14

LABEL com.moccu.type="djuggernaut" \
    com.moccu.version.node="${NODE_VERSION}" \
    com.moccu.version.phantom="${PHANTOMJS_VERSION}" \
    com.moccu.version.python="${PYTHON_VERSION}"

# Some basic stuff
RUN apt-get -qq update && apt-get -qq install -y --no-install-recommends \
        git \
        curl \
        bzip2 \
        locales \
        gettext \
        make \
        gcc \
        g++ \
        libpq-dev \
        libjpeg62-turbo-dev \
        python2.7-minimal \
        xz-utils \
        libfreetype6 \
        libfontconfig \
        fontforge \
        ttfautohint \
        graphicsmagick \
    && rm -rf /var/lib/apt/lists/*

RUN echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
    && locale-gen \
    && update-locale LANG=en_US.UTF-8

RUN set -ex \
    && for key in \
        9554F04D7259F04124DE6B476D5A82AC7E37093B \
        94AE36675C464D64BAFA68DD7434390BDBE9B9C5 \
        0034A06D9D9B0064CE8ADF6BF1747F4AD2306D93 \
        FD3A5288F042B6850C66B31F09FE44734EB7990E \
        71DCFD284A79C3B38668286BC97EC7A07EDE3FC1 \
        DD8F2338BAE7501E3DD5AC78C273792F7D83545D \
        B9AE9905FFD7803F25714661B63B535A4C206CA9 \
        C4F0DFFF4E8C1A8236409D08E73BC641CC11F4C8 \
    ; do gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$key"; done

RUN curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.xz" \
    && curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
    && gpg --batch --decrypt --output SHASUMS256.txt SHASUMS256.txt.asc \
    && grep " node-v$NODE_VERSION-linux-x64.tar.xz\$" SHASUMS256.txt | sha256sum -c - \
    && tar -xJf "node-v$NODE_VERSION-linux-x64.tar.xz" -C /usr/local --strip-components=1 \
    && rm "node-v$NODE_VERSION-linux-x64.tar.xz" SHASUMS256.txt.asc SHASUMS256.txt \
    && ln -s /usr/local/bin/node /usr/local/bin/nodejs \
    && npm install -g jspm grunt-cli phantomjs-prebuilt@$PHANTOMJS_VERSION

CMD ["bash"]
