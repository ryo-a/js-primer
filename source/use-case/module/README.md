---
author: laco
---

# モジュール {#module}

JavaScriptにおけるモジュールは、保守性・名前空間・再利用性のために使われます。

 * 保守性: 依存性の高いコードの集合を一箇所にまとめ、それ以外のモジュールへの依存性を減らすことができます
 * 名前空間: モジュールごとに分かれたスコープがあり、グローバルの名前空間を汚染しません
 * 再利用性: 便利な変数や関数を複数の場所にコピーアンドペーストせず、モジュールとして再利用できます

ひとつのJavaScriptのモジュールはひとつのJavaScriptファイルに対応します。
モジュールは変数や関数などを外部にエクスポートできます。また、別のモジュールで宣言された変数や関数などをインポートできます。
この章では **ECMAScriptモジュール（ESモジュール、JSモジュールとも呼ばれる）** について見ていきます。
ECMAScriptモジュールは、JavaScriptファイルをモジュール化する言語標準の機能です。

## ESモジュール {#es-modules}

ESモジュールは、[export文][]によって変数や関数などをエクスポートできます。
また、[import文][]を使って別のモジュールからエクスポートされたものをインポートできます。

まずは`export`文について見ていきましょう。

### export文 {#export-syntax}

`export`文は変数や関数などをエクスポートし、別のモジュールから参照できるようにします。
エクスポートの方法は、 **名前付きエクスポート** と **デフォルトエクスポート** の2種類があります。
名前付きエクスポートは、モジュールごとに複数の変数や関数などをエクスポートできます
デフォルトエクスポートは、モジュールごとにひとつしかエクスポートできません。
それぞれについて見ていきましょう。

#### 名前付きエクスポート {#named-export}

名前付きエクスポートには、すでに宣言した変数名と同じ名前でエクスポートする方法と、異なる名前でエクスポートする方法の2つがあります。
次の例は、すでに宣言されている変数をエクスポートする構文です。
`export`文のあとに続けて`{}`を書き、その中にエクスポートする変数を入れます。

[import, exportExample.js](src/export-1.js)

また、次のように、宣言とエクスポートを同時に行うこともできます。

[import, exportExample.js](src/export-3.js)

もうひとつの方法は、エクスポートする対象に **エイリアス**をつけて、宣言した変数名と違う名前でエクスポートする構文です。
エクスポートする時にエイリアスをつけるには、次のような構文を使います。`as`のあとにエクスポートしたい名前を記述します。

[import, exportExample.js](src/export-2.js)

#### デフォルトエクスポート {#default-export}

デフォルトエクスポートには、`export default`文を使ってエクスポートする方法と、エイリアスに`default`を指定して名前付きエクスポートする方法の2つがあります。

`export default`文は、後に続く式の評価結果をデフォルトエクスポートします。
次の例では、すでに宣言されている変数をデフォルトエクスポートします。

[import, exportExample.js](src/export-default-1.js)

また、次のように関数とクラスは宣言とデフォルトエクスポートを同時に行うことができます。
このとき関数やクラスは名前を省略できます。

[import, exportExample.js](src/export-default-2-invalid.js)

ただし、変数宣言は宣言とデフォルトエクスポートを同時に行うことはできません。
なぜなら、変数宣言はカンマ区切りで複数の変数を定義できてしまうためです。
次の例は実行できない不正なコードです。

[import, exportExample.js](src/export-default-variables-invalid.js)

エイリアスを使ってデフォルトエクスポートする方法では、次のように`as default`を付与します。
`default`というエイリアスがつけられた名前付きエクスポートは、デフォルトエクスポートと同じ意味になります。

[import, exportExample.js](src/export-default-as-default.js)

#### 再エクスポート {#re-export}

再エクスポートとは、別のモジュールからエクスポートされたものを、改めて自分自身からエクスポートしなおすことです。
複数のモジュールからエクスポートされたものをまとめたモジュールを作るときなどに使われます。

再エクスポートするは次のように`export`文のあとに`from`を続けて、別のモジュール名を指定します。

[import, exportExample.js](src/re-export-invalid.js)

### import文 {#import-syntax}

`import`文は、別のモジュールからエクスポートされた変数や関数などを自身のモジュールにインポートします。
インポートした変数や関数は、そのモジュールの先頭で宣言されたものと同じように扱えます。

エクスポートと同じように、インポートにも **名前付きインポート** と **デフォルトインポート**の2種類があります。
それらに加え、指定したモジュールからすべてのエクスポートをまとめてインポートする方法と、副作用のためのインポートがあります。
それぞれについて見ていきましょう。

#### 名前付きインポート {#named-import}

名前付きインポートは、指定したモジュールから名前を指定してインポートします。
名前を指定してインポートするには、`import`文のあとに続けて`{}`を書き、その中にインポートしたい名前付きエクスポートの名前を入れます。
次の例では、`./myModule.js`モジュールから名前付きエクスポートされた`foo`と`bar`をインポートしています。

[import, importExample.js](src/import-1.js)

エクスポートするときと同じように、インポートするときにも別の名前をつけることができます。

[import, importExample.js](src/import-2.js)

#### デフォルトインポート {#default-import}

デフォルトインポートは、指定したモジュールのデフォルトエクスポートをインポートします。
デフォルトインポートには、専用の構文を使う方法と、名前付きインポートで `default` を指定する方法の2つがあります。

デフォルトインポート専用の構文では、`import`文のあとに任意の名前をつけてデフォルトエクスポートをインポートします。

[import, importExample.js](src/import-default-1.js)

もうひとつの方法は、`default` を指定して名前付きインポートする方法です。
デフォルトエクスポートは`default`という名前の名前付きエクスポートとして扱うこともできます。
次のように、名前付きインポートの構文で`default`を指定し、エイリアスをつけてインポートできます。
ただし、`default`は予約語なので、この方法では必ず`as`構文を使ってエイリアスをつける必要があります。

[import, importExample.js](src/import-default-2.js)

これら2つの構文は同時に記述できます。
次のようにデフォルトインポートの構文と名前付きインポートを構文をカンマでつなげます。

[import, importExample.js](src/import-default-3.js)

ESモジュールでは、エクスポートされていないものはインポートできません。
なぜならESモジュールはJavaScriptのパース段階で依存関係が解決され、インポートする対象が存在しない場合はパースエラーとなるためです。
デフォルトインポートは、指定したモジュールがデフォルトエクスポートをしている必要があります。
同様に名前付きインポートは、指定したモジュールが指定した名前付きエクスポートをしている必要があります。

#### すべてをインポート {#namespace-import}

`import * as`構文は、すべての名前付きエクスポートをまとめてインポートします。
この方法では、モジュールごとの **名前空間** となるオブジェクトを宣言します。
エクスポートされた変数や関数などにアクセスするには、その名前空間オブジェクトのプロパティを使います。

[import, importExample.js](src/import-3.js)

#### 副作用のためのインポート {#import-for-side-effect}

モジュールの中には、グローバルのコードを実行するだけで何もエクスポートしないものがあります。
たとえば次のような、グローバル変数を操作するためのモジュールなどです。

[import, sideEffects.js](src/sideEffects.js)

このようなモジュールをインポートするには、副作用のためのインポート構文を使います。
この構文では、モジュールのグローバルコードを実行するだけで何もインポートしません。

[import, importExample.js](src/import-side-effects.js)

## ESモジュールを実行する {#run-es-modules}

作成したESモジュールを実行するためには、起点となるJavaScriptファイルをESモジュールとしてWebブラウザに読み込ませる必要があります。
Webブラウザは`script`タグによってJavaScriptファイルを読み込み、実行します。
次のように`script`タグに`type="module"`属性を付与すると、WebブラウザはJavaScriptファイルをESモジュールとして読み込みます。

```html
<!-- myModule.jsをECMAScriptモジュールとして読み込む -->
<script type="module" src="./myModule.js"></script>
<!-- インラインでも同じ -->
<script type="module">
import { foo } from "./myModule.js";
</script>
```

`type="module"`属性が付与されない場合は通常のスクリプトとして扱われ、ECMAScriptモジュールの機能は使えません。
スクリプトとして読み込まれたJavaScriptで`import`文や`export`文を使用すると、シンタックスエラーが発生します。

また、インポートされるモジュールの取得はネットワーク経由で解決されます。
そのため、モジュール名はJavaScriptファイルの絶対URLあるいは相対URLを指定します。
詳しくは[Todoアプリのユースケース](../todoapp/README.md)を参照してください。

## CommonJSモジュール {#commonjs-module}

[CommonJSモジュール][]とは、[Node.js][]環境で利用されているモジュール化の仕組みです。
CommonJSモジュールはESモジュールの仕様が策定されるよりもずっと古くから使われています。
Node.jsの標準パッケージや[NPM][]で配布されるサードパーティパッケージは、CommonJSモジュールとして提供されていることがほとんどです。

CommonJSモジュールはNode.jsのグローバル変数である`module`変数を使って変数や関数などをエクスポートします。
次のように`module.exports`プロパティに代入されたオブジェクトが、そのJavaScriptファイルからエクスポートされます。
複数の名前付きエクスポートが可能なESモジュールと違い、CommonJSでは`module.exports`プロパティの値だけがエクスポートの対象です。

[import, commonjsExport.js](src/cjs-export.js)

モジュールをインポートするには、`require`グローバル関数を使います。
次のように`require`関数にモジュール名を渡し、戻り値としてエクスポートされたオブジェクトを受け取ります。

[import, commonjsImport.js](src/cjs-import.js)

Node.jsではESモジュールもサポートする予定ですが、現在はまだ安定した機能としてサポートされていません。

## [コラム] モジュールバンドラー {#module-bundler}

**モジュールバンドラー**とは、JavaScriptのモジュール依存関係を解決し、複数のモジュールをひとつのJavaScriptファイルに結合するツールのことです。
モジュールバンドラーは起点となるモジュールが依存するモジュールを次々にたどり、適切な順序になるように結合（**バンドル**）します。

NPMによって多くのJavaScriptライブラリがNode.js向けに配布されていますが、これらはほぼすべてCommonJSモジュールです。
それらのライブラリを使ったアプリケーションをWebブラウザで実行するためには、CommonJSモジュールを解決し、ひとつのJavaScriptファイルに結合する必要がありました。
結果的に、Node.js向けでないアプリケーションもモジュール化することが一般的になり、モジュールバンドラーはJavaScript開発において無くてはならないものになりました。

モジュールバンドラーにはCommonJSだけでなくESモジュールにも対応したものもあります。
また、バンドルする際にJavaScriptコードの最適化を行うなどバンドル以外の機能をもつものもあります。
[JavaScriptモジュールについてのドキュメント][]では、
WebにおけるJavaScriptのモジュールと、バンドルする目的などについて詳しくまとめられています。

[export文]: https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/export
[import文]: https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/import
[CommonJSモジュール]: https://nodejs.org/docs/latest/api/modules.html
[Node.js]: https://nodejs.org/ja/
[NPM]: https://www.npmjs.com
[JavaScriptモジュールについてのドキュメント]: https://developers.google.com/web/fundamentals/primers/modules
