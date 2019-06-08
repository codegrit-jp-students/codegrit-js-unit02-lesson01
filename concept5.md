### NPMスクリプト

Webサイトの作成には、開発環境(development)、公開前環境(staging)、本番前環境(production)など様々なステージがあります。例えば、本番環境ではクラウド上に画像やCSSファイルをアップロードしたいかもしれません。こうした時にターミナル上で毎回必要なコマンドを打つのは面倒な上、ミスが発生しがちです。こうしたコマンドをNPMスクリプトを利用して簡単に実行出来るようにすると便利です。今回は試しにWebpackのコマンドをNPMスクリプト化しましょう。

1. package.jsonにスクリプトを追加する。

package.jsonを開いて、"dependencies"の部分の前に以下を追加しましょう。

```json
...
"scripts": {
  "dev-server": "webpack-dev-server"
},
"dependencies": {
...
```

2. スクリプトを実行する

追加したスクリプトは以下のようにして実行出来ます。すると、先ほどコマンドラインからwebpack-dev-serverを実行した際と同じようにサーバーが立ち上がります。

```bash
$ yarn run dev-server
...
webpack: Compiled successfully.
```

Yarn及びWebpackの設定のサンプルコードはこちら: 

[js-unit02-lesson01-sample01](https://github.com/codegrit-jp-students/js-unit02-lesson01-sample01)

## 更に学ぼう

### 記事で学ぶ

- [NPM公式(英語)](https://www.npmjs.com/)
- [Yarn公式(日本語あり)](https://yarnpkg.com/ja/)
- [Webpack公式](https://webpack.js.org/)
