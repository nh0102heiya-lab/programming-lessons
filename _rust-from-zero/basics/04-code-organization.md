---
layout: default
title: プログラムの書き方を整理しよう
description: main関数の役割を理解し、コードをわかりやすく整理する方法を学ぶ
order: 5
section: basics
updated: 2026-06-29
---

ここまでで、`main` 関数、自分で作る関数、変数の3つを学びました。

今回は、これらの組み合わせ方と、**わかりやすいコードの書き方**について学びます。


## main 関数の役割をおさらい

最初の記事で学んだように、`main` はプログラムの開始地点です。Rust は必ず `main` から実行を始めます。

```rust
fn main() {
    println!("Hello, world!");
}
```

`main` は、プログラムの「入口」です。ここからすべてが始まります。


## 全部を main に書くとどうなるか

例えば、あいさつを表示して、数を数えるプログラムを書いてみます。

```rust
fn main() {
    let name = "Alice";
    println!("Hello, {name}!");
    println!("Welcome!");

    let mut count = 0;
    println!("Count: {count}");
    count = 1;
    println!("Count: {count}");
    count = 2;
    println!("Count: {count}");
}
```

動きますが、少し見にくいと思いませんか？

「あいさつ」と「カウント」という異なる処理が、ひとつの `main` の中に混ざっています。このままプログラムが長くなると、どこに何が書いてあるかわからなくなってしまいます。


## main には「全体の流れ」だけを書く

ここで学んだ関数を使います。「あいさつ」と「カウント」をそれぞれ関数に分けてみましょう。

```rust
fn greet() {
    let name = "Alice";
    println!("Hello, {name}!");
    println!("Welcome!");
}

fn count_up() {
    let mut count = 0;
    println!("Count: {count}");
    count = 1;
    println!("Count: {count}");
    count = 2;
    println!("Count: {count}");
}

fn main() {
    greet();
    count_up();
}
```

`main` の中はたったの2行になりました。

`main` を読むだけで「あいさつをして、カウントをするプログラムなんだ」と、全体の流れがひと目でわかります。

このように、`main` には「プログラムの全体の流れ」だけを書き、具体的な処理は関数に分けるのがよい書き方です。


## 関数の名前が説明になる

関数に `greet` や `count_up` という名前をつけたことで、それぞれの関数が「何をするか」が名前からわかるようになりました。

関数の名前を考えるときは、「この関数は何をするものか」を短く表すようにすると、コードが読みやすくなります。

わかりやすい名前の例：

* `greet` — あいさつをする
* `count_up` — カウントを上げる
* `show_message` — メッセージを表示する

{: .note}
> 関数の名前は自由につけられますが、「何をするか」が伝わる名前を選ぶのが大切です。
>
> あとで自分がコードを読み返したときや、他の人がコードを読んだときに、名前を見ればだいたいの内容がわかるようにしておくと便利です。


## まとめ

* `main` はプログラムの入口で、全体の流れを書く場所
* 具体的な処理は関数に分けると整理しやすい
* 関数の名前で「何をするか」を説明する
* `main` が短いほど、プログラムの全体像がわかりやすい

---

## 課題

<details markdown="1">
  <summary>課題を見る</summary>

次のプログラムは、`main` の中にすべての処理が書かれています。

```rust
fn main() {
    let message = "Rust is fun!";
    println!("{message}");
    println!("{message}");
    println!("{message}");
}
```

このプログラムを、`print_message` という関数を使って書き換えてください。

* `print_message` 関数を作り、その中で `println!` を3回実行する
* `main` の中では `print_message();` を1回だけ呼び出す

---

<details markdown="1">
  <summary>答えを見る</summary>

```rust
fn print_message() {
    let message = "Rust is fun!";
    println!("{message}");
    println!("{message}");
    println!("{message}");
}

fn main() {
    print_message();
}
```

</details>

---

## 発展課題

「あいさつをする関数」と「さようならをする関数」の2つを作り、`main` から呼び出してみましょう。

<details markdown="1">
  <summary>ヒント</summary>

* `greet` 関数と `farewell` 関数を作る
* 各関数の中で `println!` を使ってメッセージを表示する
* `main` から両方を呼び出す

</details>

<details markdown="1">
  <summary>答えの例</summary>

```rust
fn greet() {
    println!("Hello!");
}

fn farewell() {
    println!("Goodbye!");
}

fn main() {
    greet();
    farewell();
}
```

<a href="https://play.rust-lang.org/?version=stable&mode=debug&edition=2024" target="_blank" rel="noopener noreferrer">
  Rust Playground で実行する
</a>

</details>

</details>
