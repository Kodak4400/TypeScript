# TypeScript

## 1章_イントロダクション
### TypeScriptを導入する理由
```
型安全性（型を使って、プログラムが不正なことをしないように防ぐこと！）
```

## 2章_TypeScript:全体像
- TypeScriptからJavaScriptへのビルド手順

```
# TS:コンパイラー
1. TypeScriptソース -> TypeScript AST(abstract syntax tree - 抽象構文木)
2. ASTが型チェッカーによってチェックされる
# JS:コンパイラー
3. TypeScript AST -> JavaScriptソース
4. JavaScriptソース -> JavaScript AST
5. AST -> バイトコード
6. バイトコードがランタイム（V8エンジン等）によって評価される
```
※ AST(abstract syntax tree - 抽象構文木)とは？ ・・・ コメントや改行、タブを除いた文章

- tsconfig.json/tslint.jsonを作るべし！
tsconfing.jsonはデフォルトで作られる
tslint.jsonは以下コマンドで作成する
```
./node_modules/.bin/tslint --init
```

## 3章_型について
- unknown型、anyは極力使わない。型が不明な場合は「unknown型」を使うこと。
理由は、「unknown型」は型がなにか絞り込めるまで使えないから。（型安全性ではある）

```typescript
let a: unknown = 30;
let b = a === 123; // boolean
let c = a + 10; // unknown型のためエラー
if (typeof a === 'number') {
  let d = a + 10; // number
}
```

- bigint型、大きな数値を表すことのできる型（`target`がES2020以下では使えないので注意）

```typescript
let c = 1234n;
const d = 5678n;
let e = c < 1235;
let f: bigint = 100n;
let g: 100n = 100n;
let h: bigint = 100; // 100はbigint型に入れられないため、エラー
```

- symbol型、オブジェクトやマップにおいて文字列キーの代わりに使用される。

```typescript
let h = Symbol('a');
let i: symbol = Symbol('b');
let j = h === i;
let k = h + 'x'; // +演算子をSymbol型に適用できないエラー
```

- 合併型と交差型
```typescript
type Cat = {name: string, purrs: boolean}
type Dog = {name: string, barks: boolean, wags: boolean}
type CatOrDogOrBoth = Cat | Dog
type CatAndDog = Cat & Dog
```

- さまざまな種類のreadonly修飾子

```typescript
type A = readonly string[];
type B = ReadonlyArray<string>;
type C = Readonly<string[]>;

type D = readonly [number, string]; // タプル型になる
type E = Readonly<[number, string]>; // タプル型になる
```

- nullとundefined

```
* null: 値の欠如
* undefined: 未定義
* void: return文を持たない関数の戻り値
* never: 決して戻ることのない関数の型（例外スローや永久に実行される型）neberはunknown以外の型のサブタイプ型: ボトム型とも呼ばれる
```

- 列挙型: 文字から数値へのマップングするもの（数値列挙）と文字列から文字列へマップングするもの（文字列列挙）の2種類ある
TypeScriptでは、（値指定すれば）分割もできる

```typescript
enum Language {
  English = 0,
  Spanish = 1,
  Russian = 2
}

enum Language {
  Japanese = 3
}

let myFirstLanguage = Language.Russian;
let mySecondLanguage = Language['English'];
```

文字列も使うことができる
以下、TypeScriptは値とキーどちらでもenumにアクセス可能だが、取得できないはずの値がエラーを出さない

```typescript
enum Color {
  Red = '#c10000',
  Bule = '#007ac1',
  Pink = '0xc10050',
  White = 255
}

let red = Color.Red
let white = Color[255];
let hatena = Color[6]; // 値が無くエラーになるはずだが、エラーにはならない
```

`const`を使用すれば取得できないはずの値を取得しようとした場合にエラーを出すことができる
そのため、列挙型を使う場合は`const`を使用すること

```typescript
const enum Color2 {
  Red = '#c10000',
  Bule = '#007ac1',
  Pink = '0xc10050',
  White = 255
}

let hatena = Color2[6]; // const enumは逆引きを許可しないので、これはエラーになる
```

enumは、分割・マージができることや、更新するとそのenumを別の場所で使っている人に迷惑をかけることがある
TypeScriptではenumのバージョンの管理までは手が届かないため。
enumは可能な限り使わないこと！もし、使用する場合は`preserveConstEnums`を`true`にすること！

以下は、enumに1つでも数値があると、それだけでenum全体が安全でなくなる例

```typescript
const enum Flippable {
  Burger = 'Burger',
  Chair = 'Chair',
  Cup = 'Cup',
  Table = 1
}

function flip(f: Flippable) {
  return 'flipped it';
}

flip(Flippable.Burger);
flip(12); // エラーがでない enumは1つでも数値があると、それだけでenum全体が安全でなくなる
```

## 4章_関数


## 5章_クラスとインタフェース
## 6章_高度な型
## 7章_エラー処理
## 8章_非同期プログラミングと並行、並列処理
## 9章_フロントエンドとバックエンドのフレームワーク
## 10章_名前空間とモジュール
## 11章_JavaScriptとの相互運用
## 12章_TypeScriptのビルドと実行
## 13章_終わりに
## 付録

