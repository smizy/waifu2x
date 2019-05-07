FROM local/torch7

RUN set -x \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        libsnappy-dev \
        graphicsmagick \
        libgraphicsmagick1-dev \
        libssl-dev \
        ca-certificates \
        git \
    && rm -rf /var/lib/apt/lists/*

ENV LUA_PATH=/root/.luarocks/share/lua/5.1/?.lua;/root/.luarocks/share/lua/5.1/?/init.lua;/root/torch/install/share/lua/5.1/?.lua;/root/torch/install/share/lua/5.1/?/init.lua;./?.lua;/root/torch/install/share/luajit-2.1.0-beta1/?.lua;/usr/local/share/lua/5.1/?.lua;/usr/local/share/lua/5.1/?/init.lua
ENV LUA_CPATH=/root/torch/install/lib/?.so;/root/.luarocks/lib/lua/5.1/?.so;/root/torch/install/lib/lua/5.1/?.so;./?.so;/usr/local/lib/lua/5.1/?.so;/usr/local/lib/lua/5.1/loadall.so
ENV PATH=/root/torch/install/bin:$PATH
ENV LD_LIBRARY_PATH=/root/torch/install/lib:/usr/local/nvidia/lib:/usr/local/nvidia/lib64
ENV DYLD_LIBRARY_PATH=/root/torch/install/lib:

RUN set -x \
    && luarocks install graphicsmagick  \
    && luarocks install lua-csnappy \
    && luarocks install md5  \
    && luarocks install uuid \
    && luarocks install csvigo  \
    && PREFIX=$HOME/torch/install luarocks install turbo

# suppress message `tput: No value for $TERM and no -T specified`
ENV TERM xterm

COPY . /root/waifu2x

WORKDIR /root/waifu2x  

# ------
# gcsfuse
# ------
RUN set -x \
    && export GCSFUSE_REPO=gcsfuse-`lsb_release -c -s` \
    && echo "deb http://packages.cloud.google.com/apt $GCSFUSE_REPO main" | sudo tee /etc/apt/sources.list.d/gcsfuse.list \
    && curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add - \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        gcsfuse \
    && rm -rf /var/lib/apt/lists/* \
    && mkdir /root/waifu2x/mnt
    



