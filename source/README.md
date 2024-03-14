# InsituVizApp

Insitu-visualization app

## Project setup
```
npm install
```
- Vue3を利用しています。 `npm install`によってインストールされます。
- このコマンドは最初に一度だけでよいです。
- Windows/Mac/Linuxで動作します。

### Compiles and hot-reloads for development
```
npm run serve
```
- これで開発用のWEBサーバーが起動されます。プレビューに利用してください
  http://localhost:8080/ にアクセス可能

- 注意点として、ファイル数に制限があります。合計で約3万ファイル以上はエラーになるようです。入力するファイル数にはきをつけてください。

- 大量のファイルを表示したい場合、表示テストは、枚数の少ないCSVのファイルで行い、あとから枚数の多いCSVに差し替えることで、大量のファイルも表示可能です。


### Compiles and minifies for production
```
npm run build
```
- このコマンドで、WEBサーバーに配置するためのファイル群が生成されます。(distフォルダ)
- サーバーには、distフォルダ一式をアップロードしてください。

### フォルダ構成

- public: 静的ファイル。distフォルダにそのままコピーされます。
- src: ビルドされるvueソースコード一式
- *.json: 設定ファイル一式

## 基本構造

### 概要

このプロジェクトは Vue[https://ja.vuejs.org/] を用いて作られています。
Insituの結果のCSVを読み込んで表示するVueのコンポーネントInsituVizApp.vue があります。
このVueコンポーネントは、様々なVueフレームワークを採用する環境で動作します。

このサンプルプロジェクトでは、App.vueで、
- import InsituVizApp from './components/InsituVizApp.vue'
- components: { InsituVizApp }
- <InsituVizApp title="Single" src="data/single/data.csv"/>

と追加するだけで、新しいデータを追加できることがわかります。

あなたが、あたらしいVueプロジェクトを作って components/InsituVizApp.vueをコピーするだけで
同様の機能を組み込むことができます。

### データの指定方法
`src="data/single/data.csv"` の設定ですが、これは、静的なファイルを参照しており、ビルドには必要ありません。
publicフォルダにデータを配置することで、データを参照することができます。
重要なことは、データを差し替える変えるたびに、ビルドを行う必要はありません。
あくまでファイルパスの指定なので、ここでは決め打ちのパスを設定しておき、ビルド。
後から、csvの内容を変更することで、任意のデータの表示が可能です。
たとえば、
```
<InsituVizApp title="My Data" src="data/data.csv"/>
```
としておき、
publicフォルダに、data/data.csvがあれば、それを差し替えて表示するプロジェクトを作ることも可能です。
毎回data.csvだけ更新すれば、再ビルドは不要です。


### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
