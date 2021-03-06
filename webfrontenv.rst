=====================================
ウェブフロントエンドの環境構築
=====================================

ウェブフロントエンドが今のJavaScript/TypeScriptの主戦場です。本章ではそのウェブフロントエンドの環境構築について紹介します。しかし、本書を書き始めたときに比べて、TypeScriptユーザーが増えるにつれて環境構築のサポートがどんどん手厚くなっているため、章の内容は減っています。そのため、1章でメジャーなフレームワークをすべて紹介することにしました。

本書で取り上げるのはReactとVue.jsです。React以外にReactをベースにした統合フロントエンドフレームワークとなっているNext.jsも取り上げます。
Agularは2以降からTypeScriptに書き直されて、TypeScript以外では書けなくなり、最初からTypeScriptが有効な状態でプロジェクトが作成されるため、説明は割愛します。

ウェブアプリケーションにおけるビルドツールのゴール
==================================================

ウェブアプリケーションの環境構築は、ただコンパイルができるというだけではなく、TypeScriptからJavaScriptにトランスパイルされたファイルを1つや複数のチャンクにまとめるバンドラーというツールも必要です。また、開発サーバなどの設定も必要でしょう。開発サーバーはHTTPサーバーとして起動し、JavaScriptやHTML、CSSを配信するサーバーとしてブラウザからは見えます。その裏では、ファイルシステムのソースコードを監視し、変更があったら即座にビルドをして動作確認までのリードタイムを短くします。それだけではなく、その開発中のウェブサイトを見ているブラウザに対して強制リセットをしかけたり（ホットリロード）、起動中にJavaScriptのコードを差し替えたり（ホットモジュールリプレースメント）といったことを実現します。また、TypeScriptだけではなく、CSSでも、事前コンパイルでコーディングの効率をあげる方法が一般化しているため、この設定も必要でしょう。

Reactの環境構築
=====================================

ReactはFacebook製のウェブフロントエンドのライブラリで、宣言的UI、仮想DOMによる高速描画などの機能を備え、現在のフロントエンドで利用されるライブラリの中では世界シェアが一位となっています。Reactによって広まった宣言的UIはいまやウェブを超え、iOSのSwift UIやAndroidのJetpack Compose、Flutterなど、モバイルアプリの世界にも波及しており、このコミュニティが近年のムーブメントの発信源となることも増えています。

JavaScriptは組み合わせが多くて流行がすぐに移り変わっていつも環境構築させられる（ように見える）とよく言われますが、組み合わせが増えても検証されてないものを一緒に使うのはなかなか骨の折れる作業で、結局中のコードまで読まないといけなかったりとか、環境構築の難易度ばかりが上がってしまいます。特にRouterとかすべてにおいて標準が定まっていないReactはそれが顕著です。それでも、もう2013年のリリースから長い期間が経ち、周辺ライブラリも自由競争の中で淘汰されたりして、定番と言われるものもかなり定まってきています。環境構築においてもほぼ全自動で済むツール\ ``create-react-app``\ が登場しましたし、オールインワンなNext.jsも利用できます。そろそろ「枯れつつあるフレームワーク」としてReactを選ぶこともできるようになると感じています。

create-react-appによる環境構築
----------------------------------------------------

Reactは標準で環境構築ツールcreate-react-appコマンドを提供しています。これを使って環境構築を行う場合、 ``--template typescript`` オプションをつけるとTypeScript用のプロジェクトが作成できます。

.. code-block:: bash

   $ npx create-react-app --template typescript myapp

   Creating a new React app in /Users/shibukawa/work/myapp.

   Installing packages. This might take a couple of minutes.
   Installing react, react-dom, and react-scripts...
   :
   Happy hacking!

これで開発サーバーも含めて設定は終わりです。次のコマンドが使えます。

* ``npm start``: 開発サーバー起動
* ``npm run build``: ビルドしてHTML/CSS/JSファイルを生成
* ``npm test``: jestによるテストの実行

構築した環境は、すべてをバンドルした\ ``react-scripts``\ というコマンドが開発サーバー、コンパイル、バンドルなどすべてを行います。このスクリプトはカスタマイズのポイントがなく、\ ``tsconfig.json``\ があるだけです。このスクリプトはときに厄介な動きをすることもあります。例えば、\ ``tsconfig.json``\ の変更を勝手に戻したりします。\ ``npm run eject``\ を実行すると、このスクリプトが分解されて\ ``config``\ フォルダに出力されます。これを変更することで、出力先からwebpackの設定まで、細かい内容が変更できるようになります。また、どのような動作が設定されていたのかも確認できます。

Next.js
------------------------------

Next.jsはVercel社が開発しているReactのオールインワンパッケージです。\ ``next``\ コマンドでプロジェクトを作成すると、webpackによるビルドサーバーやコンパイルに必要な設定だけではなく、フロントエンド側の便利機能のCSS in JS、シングルページアプリケーションのためのRouterなどの開発環境整備が完了した環境が一発で作成できます。バージョン9からはTypeScriptにも対応済みのプロジェクトが作成されるようになりました。よく使う部品が最初から設定されているため、ツールやライブラリの調整に手間がかからないのが良い点です。

デフォルトではサーバーサイドレンダリングを行うフロントエンド機能のみですが、カスタムサーバー機能を使えば、Express.jsなどのNode.jsのAPIサーバーにサーバーサイドレンダリング機能などを乗せることができます。Express.jsへの薄いラッパーになりますので、Express.jsの知識を利用して、APIサーバー機能も同一のサーバー上に追加できます。また、サーバーサイドレンダリングを使わずに静的なHTMLとJavaScriptコードを生成することも可能です。

Next.jsの良いところは、よく使うツールやライブラリ一式が検証された状態で最初からテストされている点にくわえ、issueのところでもアクティブな中の人がガンガン回答してくれていますし、何よりも多種多様なライブラリとの組み合わせをexampleとして公開してくれている\ [#]_\ のが一番強いです。
お仕事でやっていて一番ありがたいのはこの相性問題の調査に取られる時間が少なくて済む点です。

それにプラスして、自分で設定すると相当難易度の高いサーバーサイドレンダリング、静的コンテンツの生成など、さまざまなパフォーマンス改善のための機能に取り組んでいます。

.. [#] https://github.com/zeit/next.js/tree/canary/examples

本書執筆時点のバージョンは9.4です。バージョンが変わると、方法が変わる可能性があります。

次のようにコマンドをタイプし、質問に答えると（プロジェクト名、標準構成で作るかサンプルを作るか）、プロジェクトフォルダが作成されます。

.. code-block:: bash

   $ npx create-next-app

TypeScriptには対応していますが、設定ファイルを置いて拡張子を変える必要があります。作成されたプロジェクトフォルダの中で次のコマンドをタイプします。

.. code-block:: bash

   $ touch tsconfig.json
   $ npm install --save-dev typescript @types/react @types/node

次のコマンドが使えます。

* ``npm run dev``: 開発サーバー起動
* ``npm run build``: ビルドして本番モードのHTML/CSS/JSファイルを生成
* ``npm start``: ビルドしたアプリを本番モードのアプリケーションを起動

一度、開発サーバーを起動すると、\ ``tsconfig.json``\ を認知して、それに初期値を設定したり、\ ``next-env.d.ts``\ というアンビエント型を書くファイルを作成します。あとは手動で、\ ``.js``\ ファイルをリネームしていけば設定完了です。JSXが含まれるファイルは\ ``.tsx``\ に、それ以外のファイルは\ ``.ts``\ にします。

``tsconfig.json``\ は今までと少し異なります。後段でBabelが処理してくれる、ということもあって、モジュールタイプはES6 modules形式、ファイルを生成することはせず、Babelに投げるので\ ``noEmit: true``\ 。
ReactもJSX構文をそのまま残す必要があるので"preserve"となっています。JSで書かれたコードも一部あるので、\ ``allowJs: true``\ でなければなりません。

Next.jsは\ `CSS Modules <https://github.com/css-modules/css-modules>`_\ に対応しているため、button.tsxの場合、button.module.cssといった名前にすることで、そのファイル専用のCSSを作成できます。
もし、SCSSを使う場合は次のコマンドをタイプすると.module.scssが使えるようになります。

.. code-block:: bash

   $ npm install sass

詳しくはNext.jsの\ `組み込みCSSサポートページ（英語） <https://nextjs.org/docs/basic-features/built-in-css-support>`_\ を参照してください。

Reactの周辺ツールのインストールと設定
--------------------------------------

create-react-appの方はすでに設定済みですが、Next.jsはESLintやテストの設定が行われませんので、品質が高いコードを実装するために環境整備をしましょう。
ESLintを入れる場合は、ReactのJSXに対応させるために、\ ``eslint-plugin-react``\ を忘れないようにしましょう。

.. code-block:: bash

   # テスト関係
   $ npm install --save-dev jest ts-jest @types/jest

   # ESLint一式
   $ npm install --save-dev prettier　eslint
   　　 @typescript-eslint/eslint-plugin eslint-plugin-prettier
       eslint-config-prettier eslint-plugin-react npm-run-all 

ESLintはJSX関連の設定や、.tsxや.jsxのコードがあったらJSXとして処理する必要があるため、これも設定に含めます。
あと、next.config.jsとかで一部Node.jsの機能をそのまま使うところがあって、CommonJSのrequireを有効にしてあげないとエラーになるので、そこも配慮します。

.. code-block:: json
   :caption: .eslintrc

   {
     "plugins": [
       "prettier"
     ],
     "extends": [
       "plugin:@typescript-eslint/recommended",
       "plugin:prettier/recommended",
       "plugin:react/recommended"
     ],
     "rules": {
       "no-console": 0,
       "prettier/prettier": "error",
       "@typescript-eslint/no-var-requires": false,
       "@typescript-eslint/indent": "ingore",
       "react/jsx-filename-extension": [1, {
         "extensions": [".ts", ".tsx", ".js", ".jsx"]
       }]
     }
   }

最後にnpmから実行できるように設定します。

.. code-block:: json
   :caption: package.json

   {
     "scripts": {
       "test": "jest",
       "watch": "jest --watchAll",
       "lint": "eslint .",
       "fix": "eslint --fix ."
     }
   }

UI部品の追加
-------------------------

ReactやNext.jsにはかっこいいUI部品などはついておらず、自分でCSSを書かないかぎりは真っ白なシンプルなHTMLになってしまいます。React向けによくメンテナンスされているMaterial Designのライブラリである、Material UIを入れましょう。ウェブ開発になると急に必要なパッケージが増えますね。

* https://material-ui.com/

.. code-block:: bash

   $ npm install --save @material-ui/core @material-ui/icons

create-react-appで作成したアプリケーションの場合の設定方法は以下にサンプルがあります。

* https://github.com/mui-org/material-ui/tree/master/examples/create-react-app-with-typescript

まずは ``src/theme.tsx``\ をダウンロードしてきて同じパスに配置します。これがテーマ設定を行うスクリプトなので色のカスタマイズなどはこのファイルを操作することで行ます。次に\ ``src/index.tsx``\ のルート直下に\ ``ThemeProvider``\ コンポーネントを起き、テーマを設定します。すべてのUIはこのルートの下に作られることになりますが、このコンポーネントが先祖にいると、すべての部品が同一テーマで描画されるようになります。

.. code-block:: ts
   :caption: src/index.tsx

   import React, { StrictMode } from 'react';
   import { render } from 'react-dom';
   import CssBaseline from '@material-ui/core/CssBaseline';
   import { ThemeProvider } from '@material-ui/core/styles';
   import App from './App';
   import * as serviceWorker from './serviceWorker';
   import theme from './theme';

   render(
     <StrictMode>
       <ThemeProvider theme={theme}>
         <CssBaseline />
         <App />
       </ThemeProvider>
     </StrictMode>,
     document.getElementById('root')
   );

Next.jsも同じようなことをする必要がありますが、サーバーサイドレンダリングをする都合上、Next.jsでは少し別の設定が必要になります。下記のサイトにサンプルのプロジェクトがあります。

* https://github.com/mui-org/material-ui/tree/master/examples/nextjs-with-typescript

行うべきは作業は3つです。

* ``pages/_app.tsx``\ をダウンロードしてきて同じパスに配置
* ``pages/_document.tsx``\ をダウンロードしてきて同じパスに配置
* ``src/theme.tsx``\ をダウンロードしてきて同じパスに配置（必要に応じてカスタマイズ）

以上により、ページ内部で自由にMaterial UIの豊富なUI部品が使えるようになります。

Material UI以外の選択肢としては、React専用でないWeb Components製のUI部品もあります。

* Material Web Compoennts: https://github.com/material-components/material-components-web-components
* Ionic: https://ionicframework.com/
* Fast: https://github.com/microsoft/fast

React+Material UI+TypeScriptのサンプル
----------------------------------------------

ページ作成のサンプルです。Next.jsベースになっていますが、このサンプルに関してはcreate-react-appとの差はごく一部です。

* Next.jsはpages以下の.tsxファイルがページになります。このファイルは\ ``pages/index.tsx``\ なので、\ ``http://localhost:3000``\ でアクセスできます。このファイルは\ ``export default``\ でReactコンポーネントを返す必要があります。create-react-app製のコードは\ ``src/index.tsx``\ がルートになっていますが、そこからインポートされている\ ``src/App.tsx``\ がアプリケーションとしてはトップページなので、ここに書くと良いでしょう。
* ``next/head``\ は\ ``<head>``\ タグを生成するコンポーネントになりますが、create-react-appの場合は\ `react-helment <https://www.npmjs.com/package/react-helmet>`_\ などの別パッケージが必要でしょう。
* ``next/link``\ はシングルページアプリケーションのページ間遷移を実現する特殊なリンクを生成するコンポーネントです。create-react-appでシングルページアプリケーションを実現する場合は\ `React Router <https://reactrouter.com/>`_\ などの別パッケージが必要となります。

TypeScriptだからといって特殊なことはほとんどなく、世間のJavaScriptのコードのほとんどそのままコピーでも動くでしょう。唯一補完が聞かない\ ``any``\ が設定されていたのが\ ``makeStyle``\ でした。これはCSSを生成する時にパラメータとして任意の情報を設定できるのですが、今回はMaterial UIのテーマをそのまま渡すことにしたので、\ ``Theme``\ を型として設定しています。

.. code-block:: ts
   :caption: pages/index.tsx

   import { useState } from 'react';
   import Head from 'next/head';
   import Link from 'next/link';

   import { useTheme, makeStyles, Theme } from "@material-ui/core/styles";
   import { 
     Toolbar,
     Typography,
     AppBar,
     Button,
     Dialog,
     DialogActions,
     DialogContent,
     DialogContentText,
     DialogTitle,
   } from "@material-ui/core";

   const useStyle = makeStyles({
     root: (props: Theme) => ({
       paddingTop: props.spacing(10),
       paddingLeft: props.spacing(5),
       paddingRight: props.spacing(5),
     })
   });

   export default function Home() {
     const [ dialogOpen, setDialogOpen ] = useState(true);
     const classes = useStyle(useTheme());
     return (
       <div className={classes.root}>
         <Head>
           <title>My page title</title>
           <meta name="viewport" content="initial-scale=1.0, width=device-width" />
           <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700&display=swap" />
         </Head>
         <Dialog open={dialogOpen} onClose={() => {setDialogOpen(false)}}>
           <DialogTitle>Dialog Sample</DialogTitle>
           <DialogContent>
             <DialogContentText>
               Easy to use Material UI Dialog.
             </DialogContentText>
           </DialogContent>
           <DialogActions>
             <Button
               color="primary"
               onClick={() => {setDialogOpen(false)}}
             >OK</Button>
           </DialogActions>
         </Dialog>
         <AppBar>
           <Toolbar>
             <Typography variant="h6" color="inherit">
               TypeScript + Next.js + Material UI Sample
             </Typography>
           </Toolbar>
         </AppBar>
         <Typography variant="h1" gutterBottom={true}>
           Material-UI
         </Typography>
         <Typography variant="subtitle1" gutterBottom={true}>
           example project
         </Typography>
         <Typography gutterBottom={true}>
           <Link href="/about">
             <a>Go to the about page</a>
           </Link>
         </Typography>
         <Button
           variant="contained"
           color="secondary"
           onClick={() => { setDialogOpen(true)}}
         >Shot Dialog</Button>
         <style jsx={true}>{`
           .root {
             text-align: center;
           }
         `}</style>
       </div>
     );
   }

.. figure:: images/next-sample.png

   Next.js + Material UI + TypeScriptのサンプル

Vue.jsの環境構築
===========================

Vue.jsは日本で人気のあるウェブフロントエンドのフレームワークです。柔軟な設定のできるCLIツールが特徴です。本書では3系についてとりあげます。

.. note::

   今の執筆時点ではまだ正式リリースはまだです。

.. code-block:: bash

   $ npx @vue/cli@next create myapp

作成時に最初に聞かれる質問でdefaultのpreset（babelとeslint）ではなく、Manually select featuresを選択します。

.. figure:: images/vue-cli-1.png

   TypeScriptを選択する場合はManually select featuresを選択

次のオプションで必要なものをスペースキーで選択して、エンターで次に進みます。選んだ項目によって追加の質問が行われます。Routerやステート管理などのアプリケーション側の機能に関する項目以外にも、LinterやユニットテストフレームワークやE2Eテストの補助ツールなど、さまざまなものを選択できます。

.. figure:: images/vue-cli-2.png

   必要な機能を選択する

途中で、クラスベースかそうではないか、という質問が出てきます。以前ではクラスベースのAPIの方がTypeScriptとの相性がよかったのですが、Vue.js 3からの新しいAPIで、クラスベースでない時も型チェックなどに優しいAPIに改善されました。そこはチームで好きな方を選択すれば良いですし、あとから別のスタイルにすることもできます。

.. figure:: images/vue-cli-3.png

  クラススタイルのコンポーネントを利用するか？

現在のVue.jsのプロジェクトのほとんどは、\ ``.vue``\ ファイルに記述するシングルファイルコンポーネント（SFC）を使っていると思いますが、TypeScriptを使う場合、スクリプトタグの\ ``lang``\ 属性を\ ``ts``\ になっています。



.. code-block:: html

   <template>
     HTMLテンプレート
   </template>
   <script lang="ts">
     コンポーネントのソースコード(TypeScript)
   </script>
   <style>
     CSS
   </style>

クラスベースのコンポーネント
-----------------------------------------

クラスベースのコンポーネントはvue-class-componentパッケージを使い、デコレータをつけたクラスとして実装します。クラスのインスタンスのフィールドがデータ、ゲッターが算出プロパティになっているなど、TypeScriptのプレーンな文法とVueの機能がリンクしており、ウェブフロントエンドを最初に学んだのではなく、言語としてのTypeScriptやJavaScript、他の言語の経験が豊富な人には親しみやすいでしょう。環境構築のCLIのオプションではデフォルトでこちらになるような選択肢になっています。

以下のコードはVue.js 3系のクラスベースのコンポーネント実装です。

.. code-block:: ts

   <script lang="ts">
   import { Options, Vue } from "vue-class-component";
   import HelloWorld from "./components/HelloWorld.vue";

   @Options({
     components: { // テンプレート中で利用したい外部のコンポーネント
       HelloWorld
     }
   })
   export default class Counter extends Vue {
     // フィールドがデータ
     count = 0;

     // 産出プロパティはゲッターとして実装
     get message() {
       return `カウントは${this.count}です`;
     }

     // メソッドを作成するとテンプレートから呼び出せる
     increment() {
       this.count++;
     }

     decrement() {
       this.count--;
     }

     // ライフサイクルメソッドもメソッド定義でOK
     beforeMount() {
     }
   }
   </script>

これをラップしてより多くのデコレータを追加したvue-property-decoratorというパッケージもあります。こちらの方が、\ ``@Prop``\ や\ ``@Emit``\ でプロパティやイベント送信も宣言できて便利でしょう。

   * https://www.npmjs.com/package/vue-property-decorator

.. warning::

   ただし、現時点で3.0系で変わったvue-class-componentの変更にはまだ追従していないように見えます。

関数ベースのコンポーネント作成
-----------------------------------------

Vue本体で提供されている\ ``defineComponent()``\ 関数を使いコンポーネントを定義します。今までのオブジェクトをそのまま公開する方法と違い、この関数の引数のオブジェクトの型は定まっているため、以前よりもTypeScriptとの相性が改善されています。このオブジェクトの属性で名前や他の依存コンポーネント、Propsなどを定義するとともに、\ ``setup()``\ メソッドでコンポーネント内部で利用される属性などを定義します。

.. code-block:: ts

   <script lang="ts">
   import { defineComponent, SetupContext, reactive } from "vue";
   import HelloWorld from "./components/HelloWorld.vue";

   type Props = {
     name: string;
   }

   export default defineComponent({
     name: "App",
     components: {
       HelloWorld
     },
     props: {
       name: {
         type: String,
         default: "hello world"
       }
     },
     setup(props: Props, context: SetupContext) {
       const state = reactive({
         counter: 0,
       });
       const greeting = () => {
         context.emit("greeting", `Hello ${props.name}`);
       };

       return {
         state,
         greeting
       }
     }
   });
   </script>

.. note::

   **Nuxt.jsを使ったプロジェクト作成**

   Vue.jsにも、Vue.jsをベースにしてサーバーサイドレンダリングなどの自分で設定すると大変な機能がプリセットになっているNuxt.jsがあります。
   Nuxt.jsの場合は、通常の設定の後に、いくつか追加のパッケージのインストールや設定が必要です。日本語によるガイドもあります。

   * https://typescript.nuxtjs.org/ja/guide/setup.html

まとめ
============================

これで一通り、ReactやVue.jsを使う環境ができました。最低限の設定ですが、TypeScriptを使ったビルドや、開発サーバーの起動などもできるようになりました。

フロントエンドは開発環境を整えるのが大変、すぐに変わる、みたいなことがよく言われますが、ここ10年の間、やりたいこと自体は変わっていません。1ファイルでの開発は大変なので複数ファイルに分けて、デプロイ用にはバンドルして1ファイルにまとめる。ブラウザにロードしてデバッグする以前にコード解析で問題をなるべく見つけるようにする。ここ5年ぐらいは主要なのコンポーネントもだいたい固定されてきているように思います。State of JavaScript Surveyという調査をみると、シェアが高いライブラリはますますシェアを高めていっており、変化は少なくなってきています。

* https://2019.stateofjs.com/

CoffeeScriptや6to5に始まり、Babel、TypeScriptと、AltJSもいろいろ登場してきましたが、TypeScriptの人気は現在伸び率がナンバーワンです。それに応じて、各種環境構築ツールも、TypeScriptをオプションの一つに加えています。本章の内容も、最初に書いたときよりも、どんどんコンパクトになってきています。もしかしたら、将来みなさんが環境構築をする時になったら本書の内容のほとんどの工程は不要になっているかもしれません。それはそれで望ましいので、早くそのような時代がきて、お詫びの訂正をしたいと思います。
