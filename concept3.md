## Yarn

Yarnは、Facebookによって開発されたnpmと互換性のあるパッケージマネージャーです。NPMに比べてパッケージのインストール速度が早い点、パッケージのバージョンを固定しやすい点などがメリットとしており、現在ではnpmではなくyarnをメインで利用するのが主流となっています。CodeGritにおいても基本的にnpmではなくyarnを利用していきます。

**1. Yarnのインストール**

npmの説明の部分では省略しましたが、インストールするパッケージにはグローバルパッケージとローカルパッケージの2つの種類があります。先ほどのnpmの例ではローカルパッケージをインストールしました。これらのローカルパッケージは、一つのプロジェクト内のみで利用されます。それに対してグローバルパッケージでは、一つのプロジェクトだけではなく複数のプロジェクトで共用することが出来ます。Yarnパッケージは複数のプロジェクトで利用するのでグローバルパッケージとしてインストールします。グローバルパッケージのインストールは`-g`というオプションを付けて`npm install -g (パッケージ名)`というコマンドで出来ます。

```bash
$ npm install -g yarn
added 1 package in 1.225s
$ yarn --version
0.24.5
```

これでYarnのインストールが完了しました。

**2. Yarnを利用してpackage.jsonを生成**

npmと同様にyarnでも`yarn init`というコマンドを利用することでpackage.jsonを作成することが出来ます。

```bash
$ mkdir yarn-test
$ cd yarn-test
$ yarn init -y
yarn init v0.24.5
warning The yes flag has been set. This will automatically answer yes to all questions which may have security implications.
success Saved package.json
✨  Done in 0.08s.

$ cat package.json
{
  "name": "yarn-test",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT"
}
```

**3. yarnを利用してNPMパッケージをインストールする**

yarnを利用してパッケージをインストールするには`yarn add (パッケージ名)`というコマンドを利用します。npmと同様に複数パッケージを指定できます。

```bash
$ yarn add lodash react redux
yarn add v0.24.5
[1/4] 🔍  Resolving packages...
[2/4] 🚚  Fetching packages...
[3/4] 🔗  Linking dependencies...
[4/4] 📃  Building fresh packages...
success Saved lockfile.
...
✨  Done in 6.62s.
```

**4. グローバルパッケージをインストールする**

yarnを利用してグローバルパッケージをインストールするには`yarn global add (パッケージ名)`というコマンドを利用します。ここではparcelというビルドツールを試しにインストールしてみましょう。

```bash
$ yarn global add parcel-bundler
...
✨  Done in 19.35s.
```