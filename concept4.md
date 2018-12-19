## ビルドツール

![Webpackの図解](./images/webpack.png)

HTML/CSSコースでは、Sassを利用してCSSを書く方法を学びました。このSaasで書いたコードはブラウザは認識してくれませんので、CSSへとトランスパイルする必要があります。ES6のコードもブラウザによってはまだ対応が遅れている場合もあるため、Babelというライブラリを利用してES5のコードへのトランスパイルすることが必要です。

またWebサイトを本番環境に公開するときにはページの表示スピードが早くなるようにJavascriptのコードを一つにまとめたり、ファイルを圧縮してファイルサイズを小さくするようにする必要があります。こうした作業を一つ一つ手作業でやるのは大変ですし、作業を抜かしてしまう可能性もあります。そこでビルドツールを使ってコマンド一つで複数の作業を自動で行えるようにしていきます。

よく利用されるビルドツールには最近まで主流だった、**Gulp**や、先ほどインストールした**Parcel**、そして現在最も人気の高い**Webpack**などがあります。

CodeGritではWebpackを利用していきますが、Webpackのメリットを理解するのは、Javascriptの勉強を先に進めてからが良いでしょう。そのため、ここでは基本的な使い方のみ解説します。

**1. Webpackのインストール**

先ほどのyarn-testというプロジェクトにWebpackを追加します。

```bash
$ yarn add --dev webpack
...
✨  Done in 26.41s.
```

**2. ファイルの追加**

yarn-testディレクトリ内にsrcというディレクトリを作成し、その中にindex.jsというファイルを加えます。またindex.htmlというファイルをプロジェクト直下に作成します。

```bash
$ touch index.html
$ mkdir src
$ touch src/index.js
$ ls
index.html		package-lock.json	src
node_modules		package.json		yarn.lock
```

**3. index.htmlの編集**

index.htmlを以下のように編集して下さい。

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
</head>
<body>
  <div id="main"></div>
  <button id="button1">Click</button>
  <script src="./dist/bundle.js"></script>
</body>
</html>
```

**4. index.jsの編集**

先ほどインストールしたlodashを使ってみましょう。lodashはユーティリティライブラリと言われるもので配列やオブジェクト、Stringの操作に利用できる便利な関数が多く定義されています。インストールしたパッケージは`import (任意の名称) from 'ライブラリ名';`としてインポートすることが出来ます(Unit1のモジュールのレッスンも参考にしてください。)。lodashは慣習的に`_`という名称を使います。

```javascript
import _ from lodash; // lodashのインポート

/* 
0から10の間の数字をランダムで表示するファンクション
*/
function addRandomNum() {
  const mainEl = document.getElementById('main');
  let p = document.createElement('p');
  p.innerHTML = _.random(0, 10).toString();
  document.body.appendChild(p);
}

/*
ボタンがクリックされる度にaddRandomNumを呼び出すようにする即時関数。
*/
{
  const button1 = document.getElementById('button1');
  button1.addEventListener("click", addRandomNum);
  console.log("Ready");
}
```

**5. webpackのコンフィグファイルの設定**

さて上記の通りファイルを作成しましたが、index.jsファイルはES6で書かれているためそのままでは動きません。そのためWebpackを利用してES5にトランスパイルする必要があります。トランスパイルされたファイルは**build**というディレクトリ内に**bundle.js**という名前で保存します。

これを自動的に行ってくれるようにwebpackのコンフィグファイルを設定します。まずは**webpack.config.js**というファイルをプロジェクト直下に作成します。

```bash
$ touch webpack.config.js
$ atom ./webpack.config.js
```

ファイルが開いたら以下のように入力して下さい。

```javascript
// ファイルやディレクトリを操作するためのユーティリティパッケージpathを利用します。
const path = require('path'); 

module.exports = {
  mode: 'development', // ①
  entry: './src/index.js', // ②
  output: { // ③
    filename: './bundle.js', // ④
    path: path.resolve(__dirname, 'dist') // ⑤
  }
};
```
- ① モードを開発モードに設定

開発モードを適用することで、ブラウザのChrome Developer Toolsを使ってのデバッグがしやすくなります。

- ② エントリーポイントの指定

エントリーポイントは、プロジェクトのルートとなる場所です。entryポイントは複数設定することも出来ますが、今回の場合はindex.jsファイルのみを指定します。

- ③ 出力先の設定

トランスパイルされたファイルをどこに出力されたかを指定します。

- ④ 出力されるファイル名の指定

今回はbundle.jsという名前で保存するので、それをそのまま入力します。

- ⑤ path.resolve(__dirname, 'dist')

pathモジュールを使用して、現在いるディレクトリの下のdistというディレクトリ内を基準パスとします。

**6. Webpackでのビルドを実行する**

準備ができましたので、実際にトランスパイルしてみましょう。

```bash
$ node_modules/.bin/webpack
Hash: ae02219f23c45de6eee1
Version: webpack 3.10.0
Time: 430ms
      Asset    Size  Chunks                    Chunk Names
./bundle.js  544 kB       0  [emitted]  [big]  main
   [0] ./src/index.js 362 bytes {0} [built]
   [2] (webpack)/buildin/global.js 509 bytes {0} [built]
   [3] (webpack)/buildin/module.js 517 bytes {0} [built]
    + 1 hidden module
```

これで、ビルドが完了しました。プロジェクト直下にdistというディレクトリが作成されて、その中のbundle.jsというファイルにトランスパイルされたコードが保存されているのが確認出来るはずです。

**7. index.htmlをブラウザで開く**

ビルドが完成したら、ブラウザで動作確認してみましょう。ボタンをクリックしてランダムに数字が追加されていくのが確認出来たら成功です。

**8. `hot module replacement`を使う**

さて、無事にビルドは出来ましたが開発中に、毎回毎回`node_modules/.bin/webpack`というコマンドを入れてビルドするのは面倒です。そこで、Webpackにはほっとモジュールリプレイスメントという機能があります。これは、エントリーポイントとインポートされているモジュールの変更をWatch(監視)し、変更が行われる度に自動で即座に再ビルドをしてくれる仕組みです。これを有効にするための設定をしましょう。

1. webpack-dev-serverのインストール

```bash
$ yarn add --dev webpack-dev-server
```

2. コンフィグファイルの設定

webpack-dev-serverをインストールしたら、コンフィグファイルを編集しましょう。

```javascript
const path = require('path');
const webpack = require('webpack');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: './bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  devServer: {　// ①
    hot: true,
    hotOnly: true,
    watchContentBase: true,
    watchOptions: {
      ignored: /node_modules/
    }
  },
  plugins: [ // ②
    new webpack.HotModuleReplacementPlugin()
  ],
};
```

- ① Webpack Dev Serverの設定

Webpack Dev Serverには、設定できるオプションが多数あります。ここでは、hot module replacementのための基本的なものを設定しています。

- ② Hot Module Replacement Pluginを追加

WebpackのHot Module Replacement Pluginを有効にしています。

**9. 実行する**

設定が出来たら実行してみましょう。

```
$ node_modules/.bin/webpack-dev-server
Project is running at http://localhost:8080/
...
webpack: Compiled successfully.
```

サーバーが無事立ち上がったら、`http://localhost:8080/`にアクセスして見て下さい。プログラムが動いているのが確認出来るはずです。確認が出来たら、index.jsのファイルを試しに変更してみましょう。変更が行われる度に自動的にコンパイルを行ってくれるのが分かるはずです。