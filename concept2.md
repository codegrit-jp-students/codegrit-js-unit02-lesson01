## NPM

Node.jsにおいてデフォルトのパッケージマネージャーとなっているのがNPMです。

**1. Node.js、NPMのインストール**

npmを利用するにはnode.jsをパソコンにインストールする必要がありますので、以下のリンクよりインストールを行って下さい。インストールのページには2つインストールのリンクがあるので左側の安定版を選んで下さい。

- [Node.jsのインストール](https://nodejs.org/ja/)

インストールが完了したらバージョンを確認してみましょう。

```bash
$ node -v
v8.11.3

$ npm -v
5.6.0 

// 出力結果はインストールされているバージョンで異なります。
```

**2. package.jsonファイルの作成**

これで準備が整ったので実際に試してみましょう。先ほど述べたとおり、JavaScriptのプロジェクトではpackage.jsonというファイルを通してパッケージ管理を行います。package.jsonは`npm init`というコマンドで生成することが出来ます。(自分で作成しても良いのですが自動生成のほうが楽です。)

```bash
$ mkdir npm-test
$ cd npm-test
$ npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg>` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
package name: (npm-test) 

以下インタラクティブに設定が出来ます。
```

設定をインタラクティブにするのが面倒という場合でしたら`npm init -y`として、後から設定するのが良いでしょう。デフォルトのpackage.jsonファイルは以下のようになるはずです。

```bash
$ cat package.json
{
  "name": "npm-test",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

**3. NPMパッケージのインストール**

package.jsonファイルが生成出来たら、パッケージをインストールしてみましょう。ここでは試しにlodashというパッケージをインストールしてみます。インストールには`npm install (パッケージ名)`というコマンドを使います。

```bash
$ npm install lodash
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN npm-test@1.0.0 No description
npm WARN npm-test@1.0.0 No repository field.

+ lodash@4.17.4
added 1 package in 1.822s
```

インストールが完了すると、npm-testディレクトリ内にnode_modulesというフォルダが新しく作成されていることに気づくはずです。このnode_modeulsというフォルダにインストールしたパッケージと、それに関連するパッケージ(依存パッケージ)が保存されていきます。

パッケージは一つだけでなく複数インストールすることも可能です。例えばreactとreduxという2つのパッケージをインストールするには次のようにします。

```bash
$ npm install react redux
```

インストールが完了したらpackage.jsonを見てみましょう。"dependencies"以下の部分にインストールしたパッケージとそのバージョンが追加されているはずです。

```bash
$ cat package.json
{
  ...
  "dependencies": {
    "lodash": "^4.17.4",
    "react": "^16.2.0",
    "redux": "^3.7.2"
  }
}
```