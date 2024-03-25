# ベースイメージをnode:20-alpineに設定して、Reactアプリのイメージを作成
FROM node:20-alpine

# アプリを実行するための権限を持つユーザーを作成
# -S -> システムユーザーを作成
# -G -> ユーザーをグループに追加
# これはrootユーザーとしてアプリを実行しないようにするためです
# アプリがrootユーザーで実行された場合、アプリの脆弱性を悪用してホストシステムへのアクセス権を得ることができてしまいます
# 非rootユーザーとしてアプリを実行することが良い習慣です
RUN addgroup app && adduser -S -G app app

# アプリを実行するユーザーを設定
USER app

# 作業ディレクトリを/appに設定
WORKDIR /app

# package.jsonとpackage-lock.jsonを作業ディレクトリにコピー
# これはDockerのキャッシュを活用するためです
# package.jsonとpackage-lock.jsonファイルが変更されていない場合、Dockerはキャッシュされた依存関係を使用します
COPY package*.json ./

# 作業ディレクトリ内のファイルの所有権がrootに変更されることがあり、
# そのためアプリがファイルにアクセスできず、EACCES: permissiondeniedエラーが発生します
# これを避けるため、ファイルの所有権をrootユーザーに変更します
USER root

# /appディレクトリの所有権をappユーザーに変更
# chown -R <user>:<group> <directory>
# chownコマンドは、指定されたファイルのユーザーおよび/またはグループの所有権を変更します
RUN chown -R app:app .

# ユーザーをappに変更
USER app

# 依存関係をインストール
RUN npm install

# 残りのファイルを作業ディレクトリにコピー
COPY . .

# コンテナが実行時にリッスンするネットワークポートとして5173をDockerに伝える
EXPOSE 5173

# アプリを実行するコマンド
CMD npm run dev