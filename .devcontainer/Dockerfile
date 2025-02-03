FROM golang:1.23-bookworm

# 開発ツールのインストール
RUN apt-get update && apt-get install -y \
    git \
    curl \
    wget \
    zsh \
    vim \
    default-mysql-client \
    redis-tools \
    locales \
    && rm -rf /var/lib/apt/lists/*

# 日本語環境のセットアップ
RUN sed -i -E 's/# (ja_JP.UTF-8)/\1/' /etc/locale.gen \
    && locale-gen \
    && update-locale LANG=ja_JP.UTF-8

# 環境変数の設定
ENV LANG=ja_JP.UTF-8 \
    LANGUAGE=ja_JP:ja \
    LC_ALL=ja_JP.UTF-8 \
    TZ=Asia/Tokyo

# Go開発ツールのインストール
RUN go install golang.org/x/tools/gopls@latest \
    && go install github.com/go-delve/delve/cmd/dlv@latest \
    && go install honnef.co/go/tools/cmd/staticcheck@latest \
    && go install github.com/air-verse/air@latest

# zshのセットアップ
RUN sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" \
    && chsh -s $(which zsh)

WORKDIR /workspace