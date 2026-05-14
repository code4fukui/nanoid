# Nano ID


<img src="https://ai.github.io/nanoid/logo.svg" align="right"
     alt="Nano ID logo by Anton Lovchikov" width="180" height="94">

**English** | [Русский](./README.ru.md) | [简体中文](./README.zh-CN.md) | [Bahasa Indonesia](./README.id-ID.md)

JavaScript 用の、軽量で安全な、URL に適した一意の文字列 ID ジェネレーター。

> 「尊敬せずにはいられない、驚くべきレベルの無意味な完璧主義。」

* **軽量。** 130バイト（縮小およびgzip圧縮時）。依存関係はありません。[Size Limit] でサイズを管理しています。
* **安全。** ハードウェア乱数ジェネレーターを使用しています。クラスター環境でも使用可能です。
* **短い ID。** UUID よりも大きなアルファベット（`A-Za-z0-9_-`）を使用しています。そのため、ID のサイズが 36 文字から 21 文字に削減されました。
* **ポータブル。** Nano ID は [20のプログラミング言語](./README.md#other-programming-languages) に移植されています。

```js
import { nanoid } from 'nanoid'
model.id = nanoid() //=> "V1StGXR8_Z5jdHi6B-myT"
```

モダンブラウザ、IE（[with Babel]）、Node.js、React Native をサポートしています。

[online tool]: https://gitpod.io/#https://github.com/ai/nanoid/
[with Babel]:  https://developer.epages.com/blog/coding/how-to-transpile-node-modules-with-babel-and-webpack-in-a-monorepo/
[Size Limit]:  https://github.com/ai/size-limit

<a href="https://evilmartians.com/?utm_source=nanoid">
  <img src="https://evilmartians.com/badges/sponsored-by-evil-martians.svg"
       alt="Sponsored by Evil Martians" width="236" height="54">
</a>

## 目次

* [UUID との比較](#comparison-with-uuid)
* [ベンチマーク](#benchmark)
* [セキュリティ](#security)
* [API](#api)
  * [ブロッキング](#blocking)
  * [非同期](#async)
  * [非セキュア](#non-secure)
  * [カスタムアルファベットまたはサイズ](#custom-alphabet-or-size)
  * [カスタムランダムバイトジェネレーター](#custom-random-bytes-generator)
* [使い方](#usage)
  * [React](#react)
  * [React Native](#react-native)
  * [PouchDB と CouchDB](#pouchdb-and-couchdb)
  * [Web Workers](#web-workers)
  * [Jest](#jest)
  * [CLI](#cli)
  * [その他のプログラミング言語](#other-programming-languages)
* [ツール](#tools)

## UUID との比較

Nano ID は UUID v4 (ランダムベース) とほぼ同等です。
ID に含まれるランダムビット数がほぼ同じであるため (Nano ID は 126、UUID は 122)、衝突確率も同程度になります:

> 重複の確率が10億分の1になるには、103兆個のバージョン4 IDを生成する必要があります。

Nano ID と UUID v4 には、主に2つの違いがあります:

1. Nano ID はより大きなアルファベットを使用するため、同程度のランダムビット数が 36 文字ではなくわずか 21 文字に詰め込まれています。
2. Nano ID のコードは `uuid/v4` パッケージよりも **4倍小さく** なっています: 423 バイトではなく 130 バイトです。

## ベンチマーク

```rust
$ node ./test/benchmark.js
crypto.randomUUID         21,119,429 ops/sec
uuid v4                   20,368,447 ops/sec
@napi-rs/uuid             11,493,890 ops/sec
uid/secure                 8,409,962 ops/sec
@lukeed/uuid               6,871,405 ops/sec
nanoid                     5,652,148 ops/sec
customAlphabet             3,565,656 ops/sec
secure-random-string         394,201 ops/sec
uid-safe.sync                393,176 ops/sec
shortid                       49,916 ops/sec

Async:
nanoid/async                 135,260 ops/sec
async customAlphabet         136,059 ops/sec
async secure-random-string   135,213 ops/sec
uid-safe                     119,587 ops/sec

Non-secure:
uid                       58,860,241 ops/sec
nanoid/non-secure          2,744,615 ops/sec
rndm                       2,718,063 ops/sec
```

テスト構成: ThinkPad X1 Carbon Gen 9, Fedora 36, Node.js 18.9.

## セキュリティ

*乱数生成器の理論に関する優れた記事をご覧ください:
[Secure random values (in Node.js)]*

* **予測不可能性:** 安全ではない `Math.random()` を使用する代わりに、Nano ID
  は Node.js の `crypto` モジュールとブラウザの Web Crypto API を使用します。
  これらのモジュールは、予測不可能なハードウェア乱数生成器を使用しています。
* **均一性:** ID生成器をコーディングする際、`random % alphabet` はよくある間違いです。
  分布が均一にならず、一部の記号が他の記号に比べて出現する確率が低くなります。
  その結果、ブルートフォース攻撃時の試行回数を減らすことにつながります。
  Nano ID は [better algorithm] を使用しており、均一性についてテストされています。

  <img src="img/distribution.png" alt="Nano ID uniformity"
     width="340" height="135">

* **充実したドキュメント:** Nano ID のすべてのハック（工夫）はドキュメント化されています。
  [the source] のコメントを参照してください。
* **脆弱性:** セキュリティ上の脆弱性を報告するには、
  [Tidelift security contact](https://tidelift.com/security) を使用してください。
  Tidelift が修正と情報公開の調整を行います。

[Secure random values (in Node.js)]: https://gist.github.com/joepie91/7105003c3b26e65efcea63f3db82dfba
[better algorithm]:                  https://github.com/ai/nanoid/blob/main/index.js
[the source]:                        https://github.com/ai/nanoid/blob/main/index.js

## インストール

```bash
npm install --save nanoid
```

Nano ID 4 は、テストや Node.js スクリプトにおいて ESM プロジェクトでのみ動作します。CommonJS の場合は Nano ID 3.x を使用する必要があります（現在もサポートしています）:

```bash
npm install --save nanoid@3
```

ちょっとしたハックであれば、CDN から Nano ID を読み込むことができます。ただし、読み込みパフォーマンスが低いため、本番環境での使用は推奨されません。

```js
import { nanoid } from 'https://code4fukui.github.io/nanoid/nanoid.js'
```

Nano ID は ES モジュールを提供しています。webpack、Rollup、Parcel、または Node.js で Nano ID を ESM として使用するために、特別な設定を行う必要はありません。

```js
import { nanoid } from 'nanoid'
```

## API

Nano IDには、通常（ブロッキング）、非同期、非セキュアの3つのAPIがあります。

デフォルトでは、Nano IDはURLフレンドリーな記号（`A-Za-z0-9_-`）を使用し、21文字のIDを返します（UUID v4と同等の衝突確率を持たせるため）。


### ブロッキング

Nano IDを使用する上で、安全かつ最も簡単な方法です。

まれに、ハードウェア乱数生成器のためのノイズ収集中に、CPUが他の作業からブロックされる場合があります。

```js
import { nanoid } from 'nanoid'
model.id = nanoid() //=> "V1StGXR8_Z5jdHi6B-myT"
```

IDのサイズを小さくしたい（そして衝突確率を上げたい）場合は、引数としてサイズを渡すことができます。

```js
nanoid(10) //=> "IRFa-VaY2b"
```

[ID collision probability] 計算機で、IDサイズの安全性を確認することを忘れないでください。

[カスタムアルファベット](#custom-alphabet-or-size)や[乱数生成器](#custom-random-bytes-generator)を使用することもできます。

[ID collision probability]: https://zelark.github.io/nano-id-cc/


### 非同期

ハードウェアのランダムバイトを生成するために、CPUは電磁ノイズを収集します。ほとんどの場合、エントロピーはすでに収集されています。

同期APIでは、ノイズ収集の間CPUはビジー状態となり、他の有用な処理（例えば、別のHTTPリクエストの処理など）を行うことができません。

Nano IDの非同期APIを使用すると、エントロピーの収集中に別のコードを実行できます。

```js
import { nanoid } from 'nanoid/async'

async function createUser() {
  user.id = await nanoid()
}
```

エントロピー収集の詳細については、[`crypto.randomBytes`] のドキュメントを参照してください。

残念ながら、非同期APIを使用すると、ブラウザ環境におけるWeb Crypto APIの利点が失われます。したがって、現在のところブラウザでは、セキュリティ（`nanoid`）、非同期動作（`nanoid/async`）、またはドキュメントの次のセクションで説明する非セキュアな動作（`nanoid/non-secure`）のいずれかに制限されます。

[`crypto.randomBytes`]: https://nodejs.org/api/crypto.html#crypto_crypto_randombytes_size_callback


### 非セキュア

デフォルトでは、Nano IDはセキュリティと低い衝突確率のためにハードウェアによるランダムバイト生成を使用します。セキュリティをそれほど気にしない場合は、より高速な非セキュア生成器を使用できます。

```js
import { nanoid } from 'nanoid/non-secure'
const id = nanoid() //=> "Uakgb_J5m9g-0JDMbcJqLJ"
```


### カスタムアルファベットとサイズ

`customAlphabet` は、独自のアルファベットとIDサイズで `nanoid` を作成できる関数を返します。

```js
import { customAlphabet } from 'nanoid'
const nanoid = customAlphabet('1234567890abcdef', 10)
model.id = nanoid() //=> "4f90d13a42"
```

```js
import { customAlphabet } from 'nanoid/async'
const nanoid = customAlphabet('1234567890abcdef', 10)
async function createUser() {
  user.id = await nanoid()
}
```

```js
import { customAlphabet } from 'nanoid/non-secure'
const nanoid = customAlphabet('1234567890abcdef', 10)
user.id = nanoid()
```

[ID collision probability] 計算機で、カスタムアルファベットとIDサイズの安全性を確認してください。さらに多くのアルファベットについては、[`nanoid-dictionary`] のオプションを確認してください。

アルファベットは256文字以下である必要があります。そうでない場合、内部の生成アルゴリズムのセキュリティは保証されません。

デフォルトのサイズを設定するだけでなく、関数を呼び出す際にIDのサイズを変更することもできます。

```js
import { customAlphabet } from 'nanoid'
const nanoid = customAlphabet('1234567890abcdef', 10)
model.id = nanoid(5) //=> "f01a2"
```

[ID collision probability]: https://alex7kom.github.io/nano-nanoid-cc/
[`nanoid-dictionary`]:      https://github.com/CyberAP/nanoid-dictionary


### カスタム乱数生成器

`customRandom` を使用すると、`nanoid` を作成し、アルファベットとデフォルトのランダムバイト生成器を置き換えることができます。

この例では、シードベースの生成器が使用されています。

```js
import { customRandom } from 'nanoid'

const rng = seedrandom(seed)
const nanoid = customRandom('abcdef', 10, size => {
  return (new Uint8Array(size)).map(() => 256 * rng())
})

nanoid() //=> "fbaefaadeb"
```

`random` コールバックは配列のサイズを受け取り、乱数の配列を返す必要があります。

`customRandom` で同じURLフレンドリーな記号を使用したい場合は、`urlAlphabet` を使用してデフォルトのアルファベットを取得できます。

```js
const { customRandom, urlAlphabet } = require('nanoid')
const nanoid = customRandom(urlAlphabet, 10, random)
```

`customRandom` では、非同期APIおよび非セキュアAPIは利用できません。

なお、Nano IDのバージョン間で、乱数生成器の呼び出しシーケンスが変更される可能性があることに注意してください。シードベースの生成器を使用している場合、同じ結果になることは保証されません。

## 使い方

### React

Reactの `key` プロパティはレンダリング間で一貫している必要があるため、Nano IDを `key` プロパティに使用する正しい方法はありません。

```jsx
function Todos({todos}) {
  return (
    <ul>
      {todos.map(todo => (
        <li key={nanoid()}> /* DON’T DO IT */
          {todo.text}
        </li>
      ))}
    </ul>
  )
}
```

代わりに、リストアイテム内の安定したIDを使用するようにしてください。

```jsx
const todoItems = todos.map((todo) =>
  <li key={todo.id}>
    {todo.text}
  </li>
)
```

安定したIDがない場合は、`nanoid()` の代わりにインデックスを `key` として使用する方がよいでしょう。

```jsx
const todoItems = todos.map((text, index) =>
  <li key={index}> /* Still not recommended but preferred over nanoid().
                      Only do this if items have no stable IDs. */
    {text}
  </li>
)
```

ラベルと入力フィールドなどの要素をリンクするためにランダムなIDが必要なだけの場合は、[`useId`] が推奨されます。このフックはReact 18で追加されました。

[`useId`]: https://reactjs.org/docs/hooks-reference.html#useid


### React Native

React Nativeには組み込みの乱数ジェネレーターがありません。以下のポリフィルは、プレーンなReact Nativeおよび `39.x` 以降のExpoで機能します。

1. [`react-native-get-random-values`] のドキュメントを確認してインストールします。
2. Nano IDの前にそれをインポートします。

```js
import 'react-native-get-random-values'
import { nanoid } from 'nanoid'
```

[`react-native-get-random-values`]: https://github.com/LinusU/react-native-get-random-values


### PouchDB と CouchDB

PouchDBとCouchDBでは、IDをアンダースコア `_` で開始することはできません。Nano IDはデフォルトでIDの先頭に `_` を使用する可能性があるため、この問題を防ぐにはプレフィックスが必要です。

以下のオプションを使用して、デフォルトのIDを上書きします。

```js
db.put({
  _id: 'id' + nanoid(),
  …
})
```


### Web Workers

Web Workersはセキュアな乱数ジェネレーターにアクセスできません。

IDが予測不可能であるべき場合、IDのセキュリティは重要です。たとえば、「URLによるアクセス」のリンク生成などです。
予測不可能なIDは必要ないが、Web Workersを使用する必要がある場合は、非セキュアなIDジェネレーターを使用できます。

```js
import { nanoid } from 'nanoid/non-secure'
nanoid() //=> "Uakgb_J5m9g-0JDMbcJqLJ"
```

注意: 非セキュアなIDは、衝突攻撃（コリジョン攻撃）を受けやすくなります。


### Jest

`jest-environment-jsdom` を使用するJestテストランナーは、ブラウザ版のNano IDを使用します。そのため、Web Crypto APIのポリフィルが必要になります。

```js
import { randomFillSync } from 'crypto'

window.crypto = {
  getRandomValues(buffer) {
    return randomFillSync(buffer)
  }
}
```


### CLI

ターミナルで `npx nanoid` を呼び出すことで、一意のIDを取得できます。システムにNode.jsがあるだけで構いません。Nano IDをどこかにインストールしておく必要はありません。

```sh
$ npx nanoid
npx: installed 1 in 0.63s
LZfXLFzPPR4NNrgjlWDxn
```

生成されるIDのサイズは、`--size`（または `-s`）オプションで指定できます。

```sh
$ npx nanoid --size 10
L3til0JS4z
```

カスタムアルファベットは `--alphabet`（または `-a`）オプションで指定できます（この場合、`--size` が必須になることに注意してください）。

```sh
$ npx nanoid --alphabet abc --size 15
bccbcabaabaccab
```

### その他のプログラミング言語

Nano IDは多くの言語に移植されています。これらの移植版を使用することで、クライアント側とサーバー側で同じIDジェネレーターを利用できます。

* [C#](https://github.com/codeyu/nanoid-net)
* [C++](https://github.com/mcmikecreations/nanoid_cpp)
* [Clojure and ClojureScript](https://github.com/zelark/nano-id)
* [ColdFusion/CFML](https://github.com/JamoCA/cfml-nanoid)
* [Crystal](https://github.com/mamantoha/nanoid.cr)
* [Dart & Flutter](https://github.com/pd4d10/nanoid-dart)
* [Deno](https://github.com/ianfabs/nanoid)
* [Go](https://github.com/matoous/go-nanoid)
* [Elixir](https://github.com/railsmechanic/nanoid)
* [Haskell](https://github.com/MichelBoucey/NanoID)
* [Haxe](https://github.com/flashultra/uuid)
* [Janet](https://sr.ht/~statianzo/janet-nanoid/)
* [Java](https://github.com/aventrix/jnanoid)
* [Nim](https://github.com/icyphox/nanoid.nim)
* [OCaml](https://github.com/routineco/ocaml-nanoid)
* [Perl](https://github.com/tkzwtks/Nanoid-perl)
* [PHP](https://github.com/hidehalo/nanoid-php)
* [Python](https://github.com/puyuan/py-nanoid)
  （[dictionaries](https://pypi.org/project/nanoid-dictionary) 付き）
* Postgres [Extension](https://github.com/spa5k/uids-postgres)
  および [Native Function](https://github.com/viascom/nanoid-postgres)
* [R](https://github.com/hrbrmstr/nanoid) （dictionaries 付き）
* [Ruby](https://github.com/radeno/nanoid.rb)
* [Rust](https://github.com/nikolay-govorov/nanoid)
* [Swift](https://github.com/antiflasher/NanoID)
* [Unison](https://share.unison-lang.org/latest/namespaces/hojberg/nanoid)
* [V](https://github.com/invipal/nanoid)
* [Zig](https://github.com/SasLuca/zig-nanoid)

その他の環境では、コマンドラインからIDを生成できる [CLI] が利用可能です。

[CLI]: #cli

## ツール

* [ID size calculator] は、IDのアルファベットやサイズを調整した際の衝突確率を表示します。
* [`nanoid-dictionary`] は、[`customAlphabet`] で使用できる一般的なアルファベットを提供します。
* [`nanoid-good`] は、IDに卑猥な言葉が含まれないようにするためのものです。

[`nanoid-dictionary`]: https://github.com/CyberAP/nanoid-dictionary
[ID size calculator]:  https://zelark.github.io/nano-id-cc/
[`customAlphabet`]:    #custom-alphabet-or-size
[`nanoid-good`]:       https://github.com/y-gagar1n/nanoid-good
