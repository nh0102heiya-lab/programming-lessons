---
layout: default
title: 変数を使ってみよう
description: 値に名前をつけて扱う「変数」について学ぶ
order: 4
section: basics
updated: 2026-06-29
---

これまでに、関数の作り方と呼び出し方を学びました。

今回は、**変数（へんすう）** という仕組みを学びます。変数を使うと、値に名前をつけて、プログラムの中で自由に使えるようになります。


## 値に名前をつける

いままで、`println!` の中で直接文字列を書いていました。

```rust
fn main() {
    println!("Hello, Rust!");
}
```

これを、変数を使って書くと次のようになります。

```rust
fn main() {
    let message = "Hello, Rust!";
    println!("{message}");
}
```

実行すると、同じように表示されます。

```console
Hello, Rust!
```

### このコードの意味

* `let message = "Hello, Rust!";`
  文字列 `"Hello, Rust!"` に `message` という名前をつけている

* `println!("{message}");`
  `{message}` の部分が変数 `message` の値で置き換えられる

`let` は「値に名前を結びつける」ためのキーワードです。この「名前と値の組み合わせ」を**変数**と呼びます。

`{name}` のように書くと、変数の値を文字列の中に埋め込めます。変数名以外にも、数字などを直接書くこともできます。


## なぜ変数を使うのか

### 同じ値を何度も使う

例えば、`println!` で同じ文字列を何度も表示したい場合を考えます。

```rust
fn main() {
    let message = "Hello!";
    println!("{message}");
    println!("{message}");
    println!("{message}");
}
```

```console
Hello!
Hello!
Hello!
```

変数を使わない場合と比べてみてください。

```rust
fn main() {
    println!("Hello!");
    println!("Hello!");
    println!("Hello!");
}
```

どちらも同じ結果ですが、上のコードでは `"Hello!"` という文字列を1箇所だけ書けばいいので、変更が簡単です。たとえば `"Hello!"` を `"Hi!"` に変えたいとき、変数を使っていれば1箇所を変えるだけで済みます。

### コードの意味がわかりやすくなる

```rust
let name = "Alice";
let age = 10;
```

`name` や `age` という名前を見れば、その値が何を表しているかがわかります。これはプログラムが長くなるほど重要になります。


## 値を変える

変数は、デフォルトでは値を変えられません。

```rust
let x = 1;
x = 2;  // エラーになる
```

このコードを Rust Playground で実行しようとすると、コンパイルエラーになります。これは Rust の設計によるものです。「一度決めた値をうっかり変えてしまう」ことを防ぎます。

### `mut` をつけると値を変えられる

意図的に値を変えたい場合は、`mut`（ミュータブル）をつけます。

```rust
let mut x = 1;
println!("x は {x}");  // x は 1

x = 2;
println!("x は {x}");  // x は 2
```

```console
x は 1
x は 2
```

`mut` は「mutable（変更可能）」の略です。プログラマーが「この変数は変更するつもりだ」という意思表示をすることで、Rust はそれを許可します。

このように、Rust は「変更していいもの」と「変更してはいけないもの」を区別します。これにより、思わぬバグを防ぐことができます。


## まとめ

* `let` で変数を作れる
* 変数を使うと値を再利用しやすくなる
* `{name}` を使うと変数の値を文字列に埋め込める
* デフォルトでは値は変えられない
* `mut` をつけると値を変えられる

---

## 課題

<details markdown="1">
  <summary>課題を見る</summary>

次のプログラムの `____` の部分を埋めて、`message` という変数に `"Hello, variables!"` という文字列を入れて、画面に表示するプログラムを完成させてください。

```rust
fn main() {
    let ____ = "Hello, variables!";
    println!("{____}");
}
```

---

<details markdown="1">
  <summary>答えを見る</summary>

```rust
fn main() {
    let message = "Hello, variables!";
    println!("{message}");
}
```

</details>

---

## 発展課題

`count` という名前の変数を作り、0から2まで値を変えながら表示してみましょう。

<details markdown="1">
  <summary>ヒント</summary>

* `let mut count = 0;` で初期化
* `count = 1;` で値を変更
* 値を変えるたびに `println!` で表示

</details>

<details markdown="1">
  <summary>答えを見る</summary>

```rust
fn main() {
    let mut count = 0;
    println!("{count}");
    count = 1;
    println!("{count}");
    count = 2;
    println!("{count}");
}
```

<a href="https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=9f6e5d4c3b2a1f8e7d6c5b4a3f2e1d0c" target="_blank" rel="noopener noreferrer">
  Rust Playground で実行する
</a>

</details>

</details>
