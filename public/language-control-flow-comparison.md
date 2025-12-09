---
title: 【7言語比較】if文・for文の書き方と真偽値の違いで学ぶプログラミング言語の個性
tags:
  - プログラミング言語
  - 制御構文
  - C#
  - Go
  - Java
  - TypeScript
  - Ruby
  - Rust
  - Python
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

## はじめに

「Rubyでは `0` は偽ではない」

書籍『コーディングを支える技術』を読んで、言語によって「真偽値」の扱いが異なることを学び、各言語の設計思想の違いに興味を持ちました。

本記事では、7つのプログラミング言語の**if文・for文の書き方**と**真偽値の扱い**を比較します。新しい言語を学ぶ際や、複数言語を扱う仕事での「あれ、この言語ではどう書くんだっけ？」という混乱を解消する一助になれば幸いです。

## 比較する言語

| 言語 | 特徴 |
|------|------|
| **C#** | .NET の主力言語、型安全性重視 |
| **Java** | エンタープライズの定番、厳格な型システム |
| **Go** | シンプルさを追求、括弧を省略した構文 |
| **TypeScript** | JavaScript に型を追加、truthy/falsy の概念 |
| **Python** | インデントでブロックを表現、可読性重視 |
| **Ruby** | すべてがオブジェクト、`nil` と `false` のみが偽 |
| **Rust** | 安全性とパフォーマンス、式指向の設計 |

---

## 1. 真偽値の扱い

### 各言語で「偽」とみなされる値

これが言語間で最も混乱しやすいポイントです。

| 言語 | 偽（falsy）とみなされる値 |
|------|---------------------------|
| **C#** | `false` のみ（`0` は偽ではない） |
| **Java** | `false` のみ（`0` は偽ではない） |
| **Go** | `false` のみ（`0` は偽ではない） |
| **TypeScript/JS** | `false`, `0`, `""`, `null`, `undefined`, `NaN` |
| **Python** | `False`, `0`, `""`, `[]`, `{}`, `None`, etc. |
| **Ruby** | `false`, `nil` のみ（**`0` や `""` は真**） |
| **Rust** | `false` のみ（他の型は条件に使用不可） |

### コード例：0 は真か偽か？

```csharp
// C# - コンパイルエラー！int は bool に暗黙変換できない
if (0) { } // error CS0029
```

```java
// Java - コンパイルエラー！int は boolean に変換できない
if (0) { } // error: incompatible types
```

```go
// Go - コンパイルエラー！int は bool ではない
if 0 { } // non-bool 0 (type int) used as if condition
```

```typescript
// TypeScript/JavaScript - 0 は falsy なので "偽" と表示
if (0) {
    console.log("真");
} else {
    console.log("偽"); // こちらが実行される
}
```

```python
# Python - 0 は falsy なので "偽" と表示
if 0:
    print("真")
else:
    print("偽")  # こちらが実行される
```

```ruby
# Ruby - 0 は truthy なので "真" と表示！
if 0
  puts "真"  # こちらが実行される
else
  puts "偽"
end
```

```rust
// Rust - コンパイルエラー！bool 型のみ条件に使用可能
if 0 { } // expected `bool`, found integer
```

:::note warn
**Ruby の罠**: C言語経験者がRubyを書くと、`if count` のような条件で `count = 0` のときに真になってしまい、バグの原因になることがあります。
:::

---

## 2. if 文の構文比較

### 基本的な if-else

```csharp
// C#
if (条件) {
    // 処理
} else if (条件2) {
    // 処理
} else {
    // 処理
}
```

```java
// Java - C# とほぼ同じ
if (条件) {
    // 処理
} else if (条件2) {
    // 処理
} else {
    // 処理
}
```

```go
// Go - 条件の括弧が不要、波括弧は必須
if 条件 {
    // 処理
} else if 条件2 {
    // 処理
} else {
    // 処理
}
```

```typescript
// TypeScript - C# と同じ構文
if (条件) {
    // 処理
} else if (条件2) {
    // 処理
} else {
    // 処理
}
```

```python
# Python - インデントでブロックを表現、elif を使用
if 条件:
    # 処理
elif 条件2:
    # 処理
else:
    # 処理
```

```ruby
# Ruby - end で閉じる、elsif を使用
if 条件
  # 処理
elsif 条件2
  # 処理
else
  # 処理
end
```

```rust
// Rust - 条件の括弧は不要、波括弧は必須
if 条件 {
    // 処理
} else if 条件2 {
    // 処理
} else {
    // 処理
}
```

### 構文比較表

| 言語 | 条件の括弧 | ブロック | else if |
|------|-----------|----------|---------|
| C# | 必須 `()` | `{}` | `else if` |
| Java | 必須 `()` | `{}` | `else if` |
| Go | 不要 | `{}` 必須 | `else if` |
| TypeScript | 必須 `()` | `{}` | `else if` |
| Python | 不要 | インデント | `elif` |
| Ruby | 不要 | `end` | `elsif` |
| Rust | 不要 | `{}` 必須 | `else if` |

### 各言語の特徴・注意点

#### Go: if 文で変数宣言ができる

```go
// 変数のスコープを if ブロック内に限定できる
if err := doSomething(); err != nil {
    return err
}
// err はここでは使えない
```

#### Rust: if は式なので値を返せる

```rust
let result = if score >= 60 { "合格" } else { "不合格" };
```

#### Ruby: 後置 if が使える

```ruby
puts "成人です" if age >= 18
```

#### Python: 三項演算子の書き方が独特

```python
result = "合格" if score >= 60 else "不合格"
```

---

## 3. for 文（ループ）の構文比較

### 配列の要素を順に処理する

```csharp
// C# - foreach を使用
var items = new[] { 1, 2, 3 };
foreach (var item in items) {
    Console.WriteLine(item);
}

// 従来の for 文
for (int i = 0; i < items.Length; i++) {
    Console.WriteLine(items[i]);
}
```

```java
// Java - 拡張 for 文
int[] items = { 1, 2, 3 };
for (int item : items) {
    System.out.println(item);
}

// 従来の for 文
for (int i = 0; i < items.length; i++) {
    System.out.println(items[i]);
}
```

```go
// Go - for range を使用（while も for で表現）
items := []int{1, 2, 3}
for _, item := range items {
    fmt.Println(item)
}

// インデックスも必要な場合
for i, item := range items {
    fmt.Printf("%d: %d\n", i, item)
}

// 従来の for 文
for i := 0; i < len(items); i++ {
    fmt.Println(items[i])
}
```

```typescript
// TypeScript - for...of を使用
const items = [1, 2, 3];
for (const item of items) {
    console.log(item);
}

// 従来の for 文
for (let i = 0; i < items.length; i++) {
    console.log(items[i]);
}

// forEach メソッド
items.forEach(item => console.log(item));
```

```python
# Python - for...in を使用
items = [1, 2, 3]
for item in items:
    print(item)

# インデックスも必要な場合
for i, item in enumerate(items):
    print(f"{i}: {item}")

# range を使った繰り返し
for i in range(3):
    print(i)  # 0, 1, 2
```

```ruby
# Ruby - each メソッドが主流
items = [1, 2, 3]
items.each do |item|
  puts item
end

# 1行で書く場合
items.each { |item| puts item }

# インデックス付き
items.each_with_index do |item, i|
  puts "#{i}: #{item}"
end

# 回数指定の繰り返し
3.times { |i| puts i }  # 0, 1, 2
```

```rust
// Rust - for...in を使用
let items = [1, 2, 3];
for item in items {
    println!("{}", item);
}

// イテレータを明示的に使用
for item in items.iter() {
    println!("{}", item);
}

// インデックス付き
for (i, item) in items.iter().enumerate() {
    println!("{}: {}", i, item);
}

// 範囲指定
for i in 0..3 {
    println!("{}", i);  // 0, 1, 2
}
```

### ループ構文比較表

| 言語 | 配列ループ | キーワード | while の代替 |
|------|-----------|-----------|--------------|
| C# | `foreach (var x in arr)` | foreach/in | while あり |
| Java | `for (var x : arr)` | for/:（拡張for） | while あり |
| Go | `for _, x := range arr` | for/range | for で代用 |
| TypeScript | `for (const x of arr)` | for/of | while あり |
| Python | `for x in arr:` | for/in | while あり |
| Ruby | `arr.each { \|x\| }` | each メソッド | while あり |
| Rust | `for x in arr` | for/in | loop/while あり |

### 各言語の特徴・注意点

#### Go: while 文がない

```go
// while の代わりに for を使う
for 条件 {
    // 処理
}

// 無限ループ
for {
    // break で抜ける
}
```

#### Ruby: ループが豊富

```ruby
# times, upto, downto, step など多彩
5.times { |i| puts i }        # 0, 1, 2, 3, 4
1.upto(5) { |i| puts i }      # 1, 2, 3, 4, 5
5.downto(1) { |i| puts i }    # 5, 4, 3, 2, 1
0.step(10, 2) { |i| puts i }  # 0, 2, 4, 6, 8, 10
```

#### Rust: 範囲の書き方

```rust
for i in 0..5 { }   // 0, 1, 2, 3, 4（5は含まない）
for i in 0..=5 { }  // 0, 1, 2, 3, 4, 5（5を含む）
```

#### TypeScript: for...in と for...of の違い

```typescript
const arr = ['a', 'b', 'c'];

// for...of は値を取得（配列向け）
for (const value of arr) {
    console.log(value);  // 'a', 'b', 'c'
}

// for...in はキー（インデックス）を取得（オブジェクト向け）
for (const index in arr) {
    console.log(index);  // '0', '1', '2'（文字列！）
}
```

:::note warn
**TypeScript の罠**: 配列に `for...in` を使うと、インデックスが**文字列**で取得され、プロトタイプチェーンのプロパティも列挙される可能性があります。配列には `for...of` を使いましょう。
:::

---

## 4. switch 文 / パターンマッチング

### 基本的な switch 文

```csharp
// C# - break が必須（フォールスルー防止）
switch (value) {
    case 1:
        Console.WriteLine("One");
        break;
    case 2:
    case 3:
        Console.WriteLine("Two or Three");
        break;
    default:
        Console.WriteLine("Other");
        break;
}

// C# 8.0 以降の switch 式
var result = value switch {
    1 => "One",
    2 or 3 => "Two or Three",
    _ => "Other"
};
```

```java
// Java - break が必要（忘れるとフォールスルー）
switch (value) {
    case 1:
        System.out.println("One");
        break;
    case 2:
    case 3:
        System.out.println("Two or Three");
        break;
    default:
        System.out.println("Other");
        break;
}

// Java 14 以降の switch 式
var result = switch (value) {
    case 1 -> "One";
    case 2, 3 -> "Two or Three";
    default -> "Other";
};
```

```go
// Go - break 不要（自動で break）、fallthrough で明示的に継続
switch value {
case 1:
    fmt.Println("One")
case 2, 3:
    fmt.Println("Two or Three")
default:
    fmt.Println("Other")
}

// 条件式なしの switch
switch {
case value < 0:
    fmt.Println("Negative")
case value == 0:
    fmt.Println("Zero")
default:
    fmt.Println("Positive")
}
```

```typescript
// TypeScript - break が必要
switch (value) {
    case 1:
        console.log("One");
        break;
    case 2:
    case 3:
        console.log("Two or Three");
        break;
    default:
        console.log("Other");
}
```

```python
# Python 3.10 以降の match 文
match value:
    case 1:
        print("One")
    case 2 | 3:
        print("Two or Three")
    case _:
        print("Other")
```

```ruby
# Ruby の case 文（break 不要）
case value
when 1
  puts "One"
when 2, 3
  puts "Two or Three"
else
  puts "Other"
end

# 条件式として使用
result = case value
         when 1 then "One"
         when 2, 3 then "Two or Three"
         else "Other"
         end
```

```rust
// Rust の match 式（網羅性チェックあり）
let result = match value {
    1 => "One",
    2 | 3 => "Two or Three",
    _ => "Other",
};

// パターンマッチングが強力
match some_option {
    Some(x) if x > 0 => println!("Positive: {}", x),
    Some(x) => println!("Non-positive: {}", x),
    None => println!("Nothing"),
}
```

### switch / match 比較表

| 言語 | キーワード | break | フォールスルー |
|------|-----------|-------|----------------|
| C# | switch | 必須 | 明示的に goto case |
| Java | switch | 必須 | break 忘れで発生 |
| Go | switch | 不要（自動） | fallthrough で明示 |
| TypeScript | switch | 必須 | break 忘れで発生 |
| Python | match | 不要 | なし |
| Ruby | case/when | 不要 | なし |
| Rust | match | 不要 | なし |

:::note info
**Go の設計思想**: Go は「break を忘れるバグ」を防ぐため、デフォルトで自動 break にしました。フォールスルーが必要な場合は `fallthrough` を明示的に書きます。
:::

---

## 5. よくあるミス・注意点まとめ

### 言語を切り替えるときに注意すべきポイント

| 状況 | 注意点 |
|------|--------|
| C系 → Ruby | `0` が真になる |
| C系 → Python | インデントがブロックを決定 |
| Java → Go | 条件の括弧を外す、else if の位置に注意 |
| JS → Go/Rust | truthy/falsy が使えない、明示的な比較が必要 |
| C系 → Go | switch で break 不要 |
| 他言語 → Rust | if や match が式として値を返す |

### 言語別チェックリスト

#### C# / Java を書くとき
- [ ] if 文の条件は bool 型のみ
- [ ] switch の各 case に break を忘れずに

#### Go を書くとき
- [ ] if の条件に括弧は不要
- [ ] else は `}` と同じ行に書く
- [ ] switch に break は不要
- [ ] while はない（for で代用）

#### TypeScript を書くとき
- [ ] `==` ではなく `===` を使う
- [ ] 配列には for...in ではなく for...of を使う
- [ ] truthy/falsy に注意（`0`, `""`, `null` は偽）

#### Python を書くとき
- [ ] インデントは揃える（スペース4つ推奨）
- [ ] `elif`（else if ではない）
- [ ] `and`, `or`, `not`（`&&`, `||`, `!` ではない）

#### Ruby を書くとき
- [ ] `0` や `""` は真
- [ ] `elsif`（elif でも else if でもない）
- [ ] `end` で閉じる

#### Rust を書くとき
- [ ] if 文は式として値を返せる
- [ ] match は網羅性が必須
- [ ] `..` は終端を含まない、`..=` は含む

---

## まとめ

各言語の制御構文を比較してみると、それぞれの言語の設計思想が見えてきます。

- **C# / Java**: 型安全性を重視。`0` を暗黙的に偽とみなさない
- **Go**: シンプルさを追求。構文は最小限
- **TypeScript**: JavaScript の互換性を維持しつつ型を追加
- **Python**: 可読性を重視。インデントで構造を表現
- **Ruby**: 開発者の幸福度を重視。柔軟で表現力豊か
- **Rust**: 安全性を追求。式指向でパターンマッチングが強力

複数の言語を扱う際は、この記事を参考に各言語の「個性」を意識してコードを書くと、バグを減らし、より良いコードが書けるようになるでしょう。

## 参考

- [コーディングを支える技術 ―成り立ちから学ぶプログラミング作法](https://gihyo.jp/book/2013/978-4-7741-5654-5)
