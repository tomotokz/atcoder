FROM pypy:3.10-7.3.12

RUN apt update &&  \
    apt-get install -y --no-install-recommends \
    zsh time tzdata tree git curl wget bzip2 gcc g++ gfortran \
    libopenblas-dev libatlas-base-dev liblapack-dev pkg-config \
    libgeos-dev nodejs npm clang dirmngr ca-certificates \
    software-properties-common apt-transport-https ruby-full \
    build-essential gdb lcov libbz2-dev libffi-dev libgdbm-dev \
    libgdbm-compat-dev liblzma-dev libncurses5-dev libreadline6-dev \
    libsqlite3-dev libssl-dev uuid-dev zlib1g-dev libgmp-dev \
    libmpfr-dev libmpc-dev lzma lzma-dev expect \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

WORKDIR /opt
RUN wget https://www.python.org/ftp/python/3.11.4/Python-3.11.4.tgz && \
    tar -xvf Python-3.11.4.tgz && \
    cd Python-3.11.4 && \
    ./configure --enable-optimizations && \
    make -j$(nproc) && \
    make install && \
    cd .. && rm -rf Python-3.11.4 Python-3.11.4.tgz

COPY ./requirements_cpy.txt ./requirements_pypy.txt ./.zshrc /tmp/

RUN pypy3 -m pip install --upgrade pip setuptools wheel && \
    pypy3 -m pip install -r /tmp/requirements_pypy.txt && \
    python3.11 -m pip install --upgrade pip setuptools wheel && \
    python3.11 -m pip install -r /tmp/requirements_cpy.txt

RUN npm install -g atcoder-cli && \
    acc config default-task-choice all && \
    mkdir -p $(acc config-dir)/py && \
    touch $(acc config-dir)/py/main.py && \
    echo '{"task":{"program": ["main.py"],"submit": "main.py"}}' > $(acc config-dir)/py/template.json && \
    acc config default-template py && \
    acc templates

COPY ./login.sh ./init_contest.sh ./init_practice.sh ./submit.sh ./submit_safe.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/*.sh


# TODO: dotfileはGitHubから撮ってくるようにする
RUN zsh -c "$(curl --fail --show-error --silent --location https://raw.githubusercontent.com/zdharma-continuum/zinit/HEAD/scripts/install.sh)" && \
    cat /tmp/.zshrc >> $HOME/.zshrc
