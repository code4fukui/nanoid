# nanoid

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

## ライセンス
このプロジェクトは [MIT License](LICENSE) のもとで公開されています。
