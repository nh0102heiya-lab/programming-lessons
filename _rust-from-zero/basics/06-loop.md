---
layout: default
title: ループ
description: 処理を繰り返す loop, while, for の使い方を学ぶ
order: 7
section: basics
updated: 2026-07-27
---

前の記事では、条件によって処理を分ける方法を学びました。

今回は、同じ処理を**繰り返し実行する**、ループについて学びます。


## なぜループが必要か

例えば、次のようなプログラムを書いてみましょう。

```rust
fn main() {
    println!("1回目");
    println!("2回目");
    println!("3回目");
}
```

3回だけならこのままでもいいですが、100回表示したい場合はどうしますか？ `println!` を100回書くのは大変です。

ループを使うと、「同じ処理を何度も繰り返す」ことをシンプルに書けます。


## `loop`：無限ループ

`loop` は、`break` で止めるまで永遠に繰り返すループです。

```rust
fn main() {
    let mut count = 0;

    loop {
        count += 1;
        println!("{count} 回目");

        if count >= 3 {
            break;
        }
    }
}
```

```console
1 回目
2 回目
3 回目
```

### このコードの意味

```rust
loop {
```

* `loop` の中の処理が繰り返される

```rust
        count += 1;
```

* `count` の値に1を足す
* `+= 1` は `count = count + 1` の略

```rust
        if count >= 3 {
            break;
        }
```

* 条件が揃ったら `break` でループを終了

{: .note}
> `break` を忘れると、ループが止まらなくなります（無限ループ）。
> `loop` を使うときは、必ずどこかで `break` するようにしましょう。


## `while`：条件付きループ

`while` は、「条件が true のあいだ繰り返す」ループです。

```rust
fn main() {
    let mut count = 0;

    while count < 3 {
        count += 1;
        println!("{count} 回目");
    }
}
```

```console
1 回目
2 回目
3 回目
```

* `count < 3` が `true` の間、繰り返す
* `count` が `3` になると条件が `false` になり、ループが終了

`loop` と `while` を比べてみると：

```rust
// loop + break
let mut count = 0;
loop {
    count += 1;
    println!("{count}");
    if count >= 3 { break; }
}

// while
let mut count = 0;
while count < 3 {
    count += 1;
    println!("{count}");
}
```

どちらも同じ結果ですが、`while` の方が条件がわかりやすいですね。


## `for`：繰り返し

`for` は、「なにかに対して繰り返し」、「その、なにかの中身を変数に入れたい」ときに使います。

### 範囲を指定する

```rust
fn main() {
    for i in 1..=3 {
        println!("{i} 回目");
    }
}
```

```console
1 回目
2 回目
3 回目
```

* `1..=3` は「1から3まで」の範囲
* `i` に順番に `1`, `2`, `3` が入る

### 範囲の表記

| 書き方 | 意味 |
|--------|------|
| `1..5` | 1から4まで（5は含まない） |
| `1..=5` | 1から5まで（5を含む） |

```rust
fn main() {
    for i in 0..3 {
        println!("{i}");
    }
}
```

```console
0
1
2
```

* `0..3` なので、`0`, `1`, `2` の3回繰り返される


### `for` で配列を扱う

`for` は配列の要素を順番に処理するときにも便利です。配列は後で詳しく学びますが、ここでは使い方だけ見ておきましょう。

```rust
fn main() {
    let fruits = ["りんご", "バナナ", "みかん"];

    for fruit in fruits {
        println!("私は {fruit} が好きです");
    }
}
```

```console
私は りんご が好きです
私は バナナ が好きです
私は みかん が好きです
```

* `fruits` の要素（=中身）が1つずつ `fruit` に入りながら、繰り返される


## ループの戻り値

Rust のループには、**値を返す**機能があります。

```rust
fn main() {
    let mut count = 0;

    let result = loop {
        count += 1;
        if count >= 5 {
            break count * 2;
        }
    };

    println!("result = {result}");
}
```

```console
result = 10
```

* `break count * 2` で、ループの値を返している
* `result` に `10`（`5 × 2`）が入る

これは他の言語にはあまりない機能で、ループの結果を変数に代入するのに便利です。


## まとめ

* `loop` は `break` で止めるまで繰り返す
* `while` は条件が `true` の間繰り返す
* `for` は決まった回数や要素数だけ繰り返す
* `for i in 1..=n` で n 回繰り返せる
* `break` でループを終了できる
* `break 値` でループから値を返せる

---

## 課題

<details markdown="1" class="exercise">
  <summary>課題を見る</summary>

### 課題1

`for` を使って、1から10まで全ての数を表示するプログラムを書きましょう。

<details markdown="1">
  <summary>答えを見る</summary>

```rust
fn main() {
    for i in 1..=10 {
        println!("{i}");
    }
}
```

</details>

---

### 課題2

`sum_to` という関数を作りましょう。1から `n` までの合計を返します。

```rust
fn sum_to(n: i32) -> i32 {
    ____
}

fn main() {
    println!("{}", sum_to(5));   // 15
    println!("{}", sum_to(10));  // 55
}
```

<details markdown="1">
  <summary>答えを見る</summary>

```rust
fn sum_to(n: i32) -> i32 {
    let mut sum = 0;
    for i in 1..=n {
        sum += i;
    }
    sum
}

fn main() {
    println!("{}", sum_to(5));   // 15
    println!("{}", sum_to(10));  // 55
}
```

</details>

---

### 課題3

`while` を使って、カウントを `10` から `0` まで下げて表示するプログラムを書きましょう。

<details markdown="1">
  <summary>答えを見る</summary>

```rust
fn main() {
    let mut count = 10;

    while count >= 0 {
        println!("{count}");
        count -= 1;
    }
}
```

</details>

</details>
