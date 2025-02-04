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
    sudo \
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

# vscodeユーザーの作成とsudo権限の付与
RUN useradd -m -s /bin/zsh vscode \
    && echo "vscode ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/vscode \
    && mkdir -p /home/vscode/.ssh /home/vscode/.config \
    && chown -R vscode:vscode /home/vscode \
    && chmod 700 /home/vscode/.ssh

# vscodeユーザーとしてGo開発ツールをインストール
USER vscode
RUN go install golang.org/x/tools/gopls@latest \
    && go install github.com/go-delve/delve/cmd/dlv@latest \
    && go install honnef.co/go/tools/cmd/staticcheck@latest \
    && go install github.com/air-verse/air@latest

# zshのセットアップ（vscodeユーザー用）
RUN sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# Docker グループへの追加（Docker socketアクセス用）
USER root
RUN groupadd -f docker && usermod -aG docker vscode

# 作業ディレクトリの設定と権限付与
WORKDIR /workspace
RUN chown vscode:vscode /workspace

# デフォルトユーザーをvscodeに設定
USER vscode