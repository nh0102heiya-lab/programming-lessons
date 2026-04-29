---
layout: default
title: "Rust Playgroundで最初のコードを動かす"
order: 3
section: core
updated: 2026-04-29
---

Rustを学ぶ最初の一歩として、まずは「コードを実際に動かすこと」に慣れます。  
この記事では Rust Playground というサイトを使って、最も基本的なプログラムを実行し、その意味を理解します。


## Rust Playground を開く

まずはブラウザで Rust Playground を開きます。

https://play.rust-lang.org/

このサイトでは、Rust のプログラムを書き、オンラインで実行することができます。


## 最初のコード

画面中央に、以下のコードが書かれていると思います。

```rust
fn main() {
    println!("Hello, world!");
}
```

まずは先ほどのコードを実行します。上側にある、「RUN ▶」というボタンを押しましょう。

実行すると、画面下側に新しい小窓が開き、に次のように表示されます。

```
Execution
------Standard Error------

   Compiling playground v0.0.1 (/playground)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.65s
     Running `target/debug/playground`

------Standard Output------

Hello, world!
```


## コードの意味

このプログラムはとてもシンプルですが、Rustの基本が詰まっています。
実際にどういう意味なのか、見ていきましょう。

### `fn main()`

```rust
fn main() {
```

* `fn` は関数を意味します
* `main` はプログラムの開始地点です
* Rustのプログラムは必ずここから始まります


### `println!`

```rust
println!("Hello, world!");
```

* 画面に文字列を表示する命令です
* `!` が付いているのは「マクロ」と呼ばれる特別な機能です。特殊な関数と考えて大丈夫です
* `"Hello, world!"` が表示される文字列です

{: .note}
> 文字列は、「`"`（ダブルクォーテーション）」で囲まなければいけません。
>
> 囲まれていなかったらどうなるでしょうか。実は、囲まれていないものは、コンピューターに「特別な意味を持つもの」
> として認識されます。この、「特別な意味を持つもの」が具体的にどういうものなのかは、この後に出てきます。


### `{}`

```rust
{
    ...
}
```

* 処理のまとまりを表します
* この、`{}`のカッコの中に実際のコードを書きます。今回は、`println!("Hello, world!");`と書かれていますね


## 少し変更してみる

次は表示される文字列を変えてみましょう。

```rust
fn main() {
    println!("My first Rust code!");
}
```

実行すると表示が変わります。

```
My first Rust code!
```

実際に表示される文字列を変えたことで、`println!`が文字列を画面に表示させる機能を持つことが分かりました。


## まとめ

この章では次のことを学びました。

* Rust Playground の使い方
* `main` がプログラムの入口であること
* `println!` で文字を表示できること
* コードを書き換えて、結果を変えられること
