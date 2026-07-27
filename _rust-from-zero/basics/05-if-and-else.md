---
layout: default
title: 条件分岐
description: 条件によって処理を分ける if/else の使い方を学ぶ
order: 6
section: basics
updated: 2026-07-27
---

これまでは「書いたコードがそのまま全部実行される」プログラムを見てきました。

しかし、実際のプログラムでは「条件によって処理を変えたい」という場面がたくさんあります。今回は、条件によって処理を切り替える `if` と `else` を学びます。


## まず条件分岐とは

例えば、次のような場面を考えてみましょう。

* 点数が60点以上なら「合格」、それ以外なら「不合格」
* 年齢が18歳以上なら「成人」、それ以外なら「未成年」

このように、「条件に当てはまるかどうか」で処理を分けたいときがあります。


## `if` の基本

```rust
fn main() {
    let score = 75;

    if score >= 60 {
        println!("合格です");
    }
}
```

```console
合格です
```

### このコードの意味

```rust
if score >= 60 {
```

* `if` は「もし」
* `score >= 60` が条件式
* 条件式が `true` なら `{}` の中が実行される
* 条件式が `false` なら `{}` の中は飛ばされる

{: .note}
> 条件式は `true` か `false` のどちらかになります。
> これは**ブール型**（bool）と呼ばれる型です。後で詳しく学びます。


### 比較の書き方

条件式では、次の演算子が使えます。

| 演算子 | 意味 | 例 |
|--------|------|-----|
| `==` | 等しい | `x == 5` |
| `!=` | 等しくない | `x != 5` |
| `>` | より大きい | `x > 5` |
| `<` | より小さい | `x < 5` |
| `>=` | 以上 | `x >= 5` |
| `<=` | 以下 | `x <= 5` |


## `else` をつける

条件に当てはまらない場合に別の処理をしたくて、`else` を使います。

```rust
fn main() {
    let score = 45;

    if score >= 60 {
        println!("合格です");
    } else {
        println!("不合格です");
    }
}
```

```console
不合格です
```

* `score` が `45` なので、`score >= 60` は `false`
* したがって `else` の中が実行される

```
score が 60 以上？
├──  YES → 「合格です」
└──  NO  → 「不合格です」
```


## `else if` で複数の条件

条件が2つ以上ある場合は、`else if` を使います。

```rust
fn main() {
    let score = 75;

    if score >= 80 {
        println!("優秀");
    } else if score >= 60 {
        println!("合格");
    } else {
        println!("不合格");
    }
}
```

```console
合格
```

* まず `score >= 80` をチェック → `false`
* 次に `score >= 60` をチェック → `true`
* なので `else if` の中が実行される

```
score が 80 以上？
├── YES → 「優秀」
└── NO
    └── score が 60 以上？
        ├── YES → 「合格」
        └── NO  → 「不合格」
```


## 条件分岐を関数で使い分ける

条件分岐と関数を組み合わせると、もっと便利なプログラムが書けます。

```rust
fn check_age(age: i32) {
    if age >= 18 {
        println!("成人です");
    } else {
        println!("未成年です");
    }
}

fn main() {
    check_age(20);
    check_age(15);
}
```

```console
成人です
未成年です
```

* 引数で年齢を渡して、条件分岐で結果を表示
* 関数を使うことで、同じ処理を違う値で繰り返せる


## 条件式に bool 型を使う

条件分岐で使う比較演算子の結果は、`true` か `false` の**ブール型**になります。

```rust
fn main() {
    let is_adult = true;

    if is_adult {
        println!("成人です");
    } else {
        println!("未成年です");
    }
}
```

```console
成人です
```

* `is_adult` が `true` なので、`if` の中が実行される
* 条件式には変数を直接入れることができる


## まとめ

* `if` で条件に応じた処理ができる
* `else` で条件に当てはまらない場合の処理を書ける
* `else if` で複数の条件をチェックできる
* 条件式は `true` か `false` になる
* 比較演算子（`==`, `!=`, `>`, `<`, `>=`, `<=`）で条件を書く

---

## 課題

<details markdown="1" class="exercise">
  <summary>課題を見る</summary>

### 課題1

`abs` という関数を作りましょう。`i32` 型の引数を1つ受け取り、絶対値を返します。

* 正の数ならそのまま返す
* 0以下なら符号を変えて返す

<details markdown="1">
  <summary>ヒント</summary>

```rust
fn abs(x: i32) -> i32 {
    ____
}

fn main() {
    println!("{}", abs(5));   // 5
    println!("{}", abs(-3));  // 3
}
```

</details>


<details markdown="1">
  <summary>答えを見る</summary>

```rust
fn abs(x: i32) -> i32 {
    if x >= 0 {
        x
    } else {
        -x
    }
}

fn main() {
    println!("{}", abs(5));   // 5
    println!("{}", abs(-3));  // 3
}
```

</details>

---

### 課題2

文字列の長さを受け取り、次のように表示する関数を作りましょう。

* 長さが0なら「空です」
* 長さが10以下なら「短いです」
* 長さが11以上なら「長いです」

<details markdown="1">
  <summary>答えを見る</summary>

```rust
fn describe_length(len: i32) {
    if len == 0 {
        println!("空です");
    } else if len <= 10 {
        println!("短いです");
    } else {
        println!("長いです");
    }
}

fn main() {
    describe_length(0);   // 空です
    describe_length(5);   // 短いです
    describe_length(20);  // 長いです
}
```

</details>

</details>
