---
layout: default
title: 変数を画面に反映する
description: struct でアプリの状態を管理し、画面に表示する
order: 4
section: egui-basics
updated: 2026-07-25
---

テキストの表示方法を学びました。今回は、アプリの**状態**を変数で管理し、画面に反映する方法を学びます。


## 状態とは

egui のアプリでは、アプリが持つデータを「**状態**」と呼びます。
例えば、カウンターの値や入力されたテキストなどが状態にあたります。

状態は `struct` で定義します。

```rust
struct MyApp {
    name: String,
}
```

`MyApp` は `name` という名前の `String` 型のフィールドを持つ構造体です。


## 状態を初期化する

`Default` トレイトを実装すると、デフォルトの値で初期化できるようになります。

```rust
#[derive(Default)]
struct MyApp {
    name: String,
}
```

`String` のデフォルト値は空の文字列 `""` です。


## 画面に状態を表示する

`ui` メソッドの中で `self` を使って状態にアクセスできます。

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.heading("Name Input App");
            let name = &self.name;
            ui.label(format!("Your name: {name}"));
        });
    }
}
```

`self.name` は `MyApp` の `name` フィールドを指します。
今は空文字列なので、「あなたの名前: 」とだけ表示されます。


## 状態を変更する

`ui` メソッドの引数 `&mut self` は「可変の参照」です。
これにより、`self` のフィールドを変更できます。

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.heading("Name Input App");
            self.name = "Alice".to_string();
            let name = &self.name;
            ui.label(format!("Your name: {name}"));
        });
    }
}
```

これで毎フレーム `name` が `"Alice"` に設定され、表示されます。


## `format!` マクロ

`format!` は文字列の中に変数の値を埋め込むマクロです。

```rust
let age = 25;
let message = format!("Age: {age}");
```

`{age}` の部分が `25` に置き換えられます。

複数の変数も使えます。

```rust
let name = "Bob";
let age = 30;
format!("{name} is {age}")
```


## まとめ

* `struct` でアプリの状態を定義する
* `#[derive(Default)]` でデフォルト値を設定する
* `self.フィールド名` で状態にアクセスする
* `&mut self` により状態を変更できる
* `format!` で文字列に変数の値を埋め込める
* 状態はフレーム間で保持される

---

## 課題

<details markdown="1" class="exercise">
  <summary>課題を見る</summary>

`count` という名前の `i32` 型のフィールドを持った `MyApp` を作り、
毎フレーム `count` の値を1増やすアプリを作ってみましょう。

画面には「カウント: ○」のように表示してください。

---

<details markdown="1">
  <summary>ヒント</summary>

* `count` を `0` で初期化する
* `ui` メソッドの中で `self.count += 1;` と書く

</details>

---

<details markdown="1">
  <summary>答えを見る</summary>

```rust
use eframe::egui;

fn main() -> eframe::Result {
    let options = eframe::NativeOptions::default();

    eframe::run_native(
        "counter",
        options,
        Box::new(|_cc| Ok(Box::new(MyApp::default()))),
    )
}

#[derive(Default)]
struct MyApp {
    count: i32,
}

impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            self.count += 1;
            let count = self.count;
            ui.label(format!("Count: {count}"));
        });
    }
}
```

毎フレームカウントが増えるので、Window が開いた瞬間から数字が高速で変わります。

</details>

</details>
