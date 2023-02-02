# 環境構築編~github~

1. github でプロジェクトを作成
2. git init で.git ファイルを作ることは忘れないようにして、github の案内通り first commit と push を行う
3. .gitignore のファイルも生成しとくと便利かな

# 環境構築編~~

1. Dockerfile と docker-compose.yml を空だけど作った。
2. 設定をコピーして使う
3. 1 の記述を反映。docker compose build --no-cache
4. docker compose up してコンテナを立てる。create-react-app してないから空のコンテナが立ち上がる
5. コンテナ内で下記コマンドを打って react の開発フォルダを生成する
   1. npm install -g create-react-app && create-react-app react-todo-pwa-practice "(--template typescript)"
6. 5 の影響で、react-todo-pwa-practice フォルダが Dockerfile と同じ層にできる。
   1. Dockerfile の mkdir でも volumes でもコンテナ内にディレクトリはできるみたい。
7. docker-compsose.yml に`command: sh -c "cd react-todo-pwa-practice && npm start"`が書いてあるか確認する。
   1. npm start の記述がないと compose up しても react のコンパイルが自動でされないので localhost で表示されない。

# 疑問

1. Dockerfile や docker-compose.yml のもっといい書き方をしりたい。
2.

# 深く調べた

1. react とは「React は動的な Web ページ (アプリケーション) を構築するためのライブラリですが、React では 「React 要素 (React Element)」を操作することによって HTML を操作します。」

2. js の関数について
   下記のソースコードの場合、引数として渡される const price がコールバック関数
   関数を引数として受け取る関数 function tomato が高階関数
   https://www.javadrive.jp/javascript/function/index16.html

   ```js
   function tomato(price, func){
   const name = 'トマト';
   func(name, price);
   }

   const price = function(name, price){
   console.log(name + ' の値段は ' + price + ' 円です。');
   }

   tomato(100, price);
   >> トマト の値段は 100 円です。
   ```

3. 無名関数、コールバック関数、アロー関数での無名関数
   下記の kensyo 関数は引数 text に関数を受け取り、処理の中で引数で受け取った関数を発火させる
   kensyo 関数=高階関数
   console.log('test!test!') = 無名関数 + コールバック関数

   ```js
   const kensyo = (text) => {text() ,console.log('うしろにでる')};
   kensyo(() => {console.log('test!test!')})
   >test!test!
   >うしろにでる
   ```

