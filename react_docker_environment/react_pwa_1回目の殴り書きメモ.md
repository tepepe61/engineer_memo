# docker 導入時の memo

1. docker-compose.yml の下記記述は、docker-compose.yml が置いてあるパスを.としてる。
   そして:より右側が docker 内のどのコンテナを volumes 化したいかのパス
   volumes: - ./reactApp:/myapp

2. volumes とは外付け HDD にデータを保存するイメージ。volumes を行わないとコンテナを削除した際にコンテナ内の永続データが破棄されてしまうから
3. 今回の myapp は Dockerfile 内にて mkdir して作ってるディレクトリ。反映させるには docker-compose build --no-cache しないとダメ
4. 下記コマンドは
   docker-compose run --rm react-app sh -c "npm install -g create-react-app && create-react-app react-todo-pwa (--template typescript)"
5. docker-compose run --rm app はコンテナ起動して実行次第削除するコマンド
6. npm install -g の-g は node_modules のインストール先の path の違い?

# uesEffect 系についての memo

1. 用語 memo
   import React, { useState, useEffect} from "react";
   AuthContext = React.createContext();
   ↑ AuthContext は Provider と Consumer という概念を所持してる。Provider に囲まれた子コンポーネントは、提供してるデータにアクセスできる

# 追加の npm

docker compose exec react-app npm install object-dig
これって npm を dockerfile に書くべきかと最初は思ってたけど、Gemfile と同じで package.json が更新されるから無視でいいのか。

# ui

1. npm のバージョンが 7?だったためか、6 の時の依存関係で npm install できなかったので material-ui が適用できなかった。
   docker compose exec react-app npm install "@material-ui/core@4.\*" --save --legacy-peer-deps

2. makeStyles で css 当ててるけどフォルダを別のところで定義できるか?

# firebase にでブロイするために売った

1. npm install -g firebase-tools --no-localhost
   E73B1
   4/0AWtgzh4CkChYICK8yTc0kHrYL68OLRsVE7rl7XkuIajuVLPr4GHsV273TbwOAxTr_wdxjA

# deploy

1. docker compose exec react-app npm run build ができなかったので、
   docker compose exec react-app bin/ash の後下記ディレクトリ内で直で実行した。何がダメだったのか?
   /react-todo-pwa/react-todo-pwa # npm run build

# eslint 入れてみる

docker compose exec react-app /bin/ash -c "cd react-todo-pwa && npm install eslint --save-dev"
docker compose exec react-app /bin/ash -c "cd react-todo-pwa && npm init @eslint/config"

```
-> % docker compose exec react-app /bin/ash -c "cd react-todo-pwa && npm init @eslint/config"

Need to install the following packages:
  @eslint/create-config
Ok to proceed? (y) y
✔ How would you like to use ESLint? · style
✔ What type of modules does your project use? · esm
✔ Which framework does your project use? · react
✔ Does your project use TypeScript? · No / Yes
✔ Where does your code run? · browser
✔ How would you like to define a style for your project? · guide
✔ Which style guide do you want to follow? · airbnb
✔ What format do you want your config file to be in? · JavaScript
Checking peerDependencies of eslint-config-airbnb@latest
The config that you've selected requires the following dependencies:

eslint-plugin-react@^7.28.0 eslint-config-airbnb@latest eslint@^7.32.0 || ^8.2.0 eslint-plugin-import@^2.25.3 eslint-plugin-jsx-a11y@^6.5.1 eslint-plugin-react-hooks@^4.3.0
✔ Would you like to install them now? · No / Yes
✔ Which package manager do you want to use? · npm
```

# elint と prettier の競合を直す

1. eslintrc.js の extends の最後に prettier を記載
2. そうすることで、prettierrc の設定が最後上書きするから、競合に関して微調整ができる
3. extends を読み込むためには ↓ が必要っぽい
   npm install --save-dev eslint-config-prettier
4.

