# Nano ID

Nano IDは、JavaScriptで使用できる軽量でユニークな文字列IDジェネレーターです。

## 機能
- **小さい.** 圧縮後130バイトと非常に小さい。依存関係がない。
- **安全.** 暗号化された乱数を使用し、クラスターでも使えます。 
- **短い.** UUIDより短い21文字のID。
- **移植性.** 20以上の言語に移植されています。

## 必要環境
- 現代的なブラウザ
- IE (Babel使用)
- Node.js

## 使い方
```js
import { nanoid } from 'nanoid'
model.id = nanoid() //=> "V1StGXR8_Z5jdHi6B-myT"
```

- [Русский](./README.ru.md)
- [简体中文](./README.zh-CN.md)
- [Bahasa Indonesia](./README.id-ID.md)
- [20 programming languages](./README.md#other-programming-languages)
- [Comparison with UUID](#comparison-with-uuid)
- [Benchmark](#benchmark)
- [Security](#security)
- [API](#api)
- [Blocking](#blocking)
- [Async](#async)
- [Non-Secure](#non-secure)
- [Custom Alphabet or Size](#custom-alphabet-or-size)
- [Custom Random Bytes Generator](#custom-random-bytes-generator)
- [Usage](#usage)
- [React](#react)
- [React Native](#react-native)
- [PouchDB and CouchDB](#pouchdb-and-couchdb)
- [Web Workers](#web-workers)
- [Jest](#jest)
- [CLI](#cli)
- [Other Programming Languages](#other-programming-languages)
- [Tools](#tools)
- [Tidelift security contact](https://tidelift.com/security)
- [custom alphabet](#custom-alphabet-or-size)
- [random generator](#custom-random-bytes-generator)
- [C#](https://github.com/codeyu/nanoid-net)
- [C++](https://github.com/mcmikecreations/nanoid_cpp)
- [Clojure and ClojureScript](https://github.com/zelark/nano-id)
- [ColdFusion/CFML](https://github.com/JamoCA/cfml-nanoid)
- [Crystal](https://github.com/mamantoha/nanoid.cr)
- [Dart & Flutter](https://github.com/pd4d10/nanoid-dart)
- [Deno](https://github.com/ianfabs/nanoid)
- [Go](https://github.com/matoous/go-nanoid)
- [Elixir](https://github.com/railsmechanic/nanoid)
- [Haskell](https://github.com/MichelBoucey/NanoID)
- [Haxe](https://github.com/flashultra/uuid)
- [Janet](https://sr.ht/~statianzo/janet-nanoid/)
- [Java](https://github.com/aventrix/jnanoid)
- [Nim](https://github.com/icyphox/nanoid.nim)
- [OCaml](https://github.com/routineco/ocaml-nanoid)
- [Perl](https://github.com/tkzwtks/Nanoid-perl)
- [PHP](https://github.com/hidehalo/nanoid-php)
- [Python](https://github.com/puyuan/py-nanoid)
- [dictionaries](https://pypi.org/project/nanoid-dictionary)
- [Extension](https://github.com/spa5k/uids-postgres)
- [Native Function](https://github.com/viascom/nanoid-postgres)
- [R](https://github.com/hrbrmstr/nanoid)
- [Ruby](https://github.com/radeno/nanoid.rb)
- [Rust](https://github.com/nikolay-govorov/nanoid)
- [Swift](https://github.com/antiflasher/NanoID)
- [Unison](https://share.unison-lang.org/latest/namespaces/hojberg/nanoid)
- [V](https://github.com/invipal/nanoid)
- [Zig](https://github.com/SasLuca/zig-nanoid)
- [README.ja.md](README.ja.md)

## ライセンス
このプロジェクトは [MIT License](LICENSE) のもとで公開されています。
