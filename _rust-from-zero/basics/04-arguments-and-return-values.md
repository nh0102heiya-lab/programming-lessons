---
layout: default
title: 引数と戻り値
description: 関数に値を渡したり、値を返したりする方法を学ぶ
order: 5
section: basics
updated: 2026-07-27
---

前の記事では、関数を自分で作る方法を学びました。

しかし、今までの関数は決まった処理のみで、あまり柔軟性がありませんでした。今回は、関数に「値を渡す」方法と、関数から「値を返す」方法を学びます。


## 引数（ひきすう）：関数に値を渡す

### 関数に名前を渡してみる

前の記事で作った `greet` 関数はいつも "Hello!" と表示するものでした。

これを、「あいさつ対象の名前を指定できる」ように変更してみましょう。

```rust
fn greet(name: &str) {
    println!("Hello, {name}!");
}

fn main() {
    greet("Alice");
    greet("Bob");
}
```

```console
Hello, Alice!
Hello, Bob!
```

これで、`greet` を呼び出すたびに違う名前を渡せる、**より柔軟**な関数になりました。


### このコードの意味

```rust
fn greet(name: &str) {
```

* `name` は**引数**（ひきすう）と呼ばれるもの
* `&str` は文字列の型で、`name` が文字列であることを示す

引数を定義すると、呼び出し側で渡した値が、関数の中で `name` という名前で使えます。一種の変数みたいなものですね。

```rust
greet("Alice");
```

* `"Alice"` が関数に渡される
* 関数の中では `name` が `"Alice"` になる

{: .note}
> 引数は「関数への入力」です。
> 関数が受け取るデータを、呼び出し時に指定することができます。


### 複数の引数

引数はたくさん渡すことができます。

```rust
fn introduce(name: &str, age: i32) {
    println!("私は {name} です。{age} 歳です。");
}

fn main() {
    introduce("Alice", 10);
}
```

```console
私は Alice です。10 歳です。
```

* `name` と `age` の2つの引数がある
* 順番に値を渡す必要がある
* `,`（カンマ）で区切る


## 戻り値：関数から値を返す

### 関数の結果を受け取る

関数が計算した結果を、外で使いたいことがあります。その場合は**戻り値**（もどりち）を使います。

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b
}

fn main() {
    let result = add(3, 4);
    println!("3 + 4 = {result}");
}
```

```console
3 + 4 = 7
```


### このコードの意味

```rust
fn add(a: i32, b: i32) -> i32 {
```

* `-> i32` は「この関数は `i32` 型の値を返します」という意味

```rust
    a + b
```

* 最後に書いた式が**戻り値**になる
* `return` を使っても書けますが、最後の式の場合は省略するのが Rust の一般的な書き方

```rust
let result = add(3, 4);
```

* `add(3, 4)` の結果（`7`）を `result` に代入


### `return` を使う書き方

戻り値は `return` を使って明示的に返すこともできます。

```rust
fn add(a: i32, b: i32) -> i32 {
    return a + b;
}
```

どちらの書き方でも同じように動きます。Rust では、関数の最後の値を返す場合は `return` を省略するのが一般的です。


## 引数と戻り値を組み合わせる

```rust
fn multiply(a: i32, b: i32) -> i32 {
    a * b
}

fn main() {
    let x = 5;
    let y = 3;
    let result = multiply(x, y);
    println!("{x} × {y} = {result}");
}
```

```console
5 × 3 = 15
```

* 関数に値を渡し（引数）、結果を受け取る（戻り値）
* この組み合わせで、関数を自由に使いこなせるようになります


## まとめ

* 関数に値を渡すには**引数**を使う
* 引数には型を指定する
* 関数から値を返すには**戻り値**を使う
* `-> 型` で返す値の型を指定する
* 最後の式の値が自動的に戻り値になる

---

## 課題

<details markdown="1" class="exercise">
  <summary>課題を見る</summary>

### 課題1

`square` という関数を作りましょう。この関数は `i32` 型の引数を1つ受け取り、その2乗（かけた値）を返します。

<details markdown="1">
  <summary>ヒント</summary>

```rust
fn square(____: ____) -> ____ {
    ____
}
fn main() {
    let result = square(5);
    println!("5 × 5 = {result}");
}
```

</details>


<details markdown="1">
  <summary>答えを見る</summary>

```rust
fn square(x: i32) -> i32 {
    x * x
}

fn main() {
    let result = square(5);
    println!("5 × 5 = {result}");
}
```

</details>

---

### 課題2

`greet_with_age` という関数を作りましょう。名前と年齢を引数に取り、次のように表示します。

```console
Hello, Alice! 10歳ですね。
```

<details markdown="1">
  <summary>答えを見る</summary>

```rust
fn greet_with_age(name: &str, age: i32) {
    println!("Hello, {name}! {age}歳ですね。");
}

fn main() {
    greet_with_age("Alice", 10);
}
```

</details>

</details>
