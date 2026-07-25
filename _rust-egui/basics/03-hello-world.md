---
layout: default
title: ハローワールドを表示する
description: heading と label でテキストを表示する
order: 3
section: egui-basics
updated: 2026-07-25
---

Window の基本構造を理解したところで、今回はテキストの表示方法を学びます。
`heading` と `label` の2つの方法を使い分けてみましょう。


## `ui.heading()` で見出しを表示する

`heading` は、太字で大きなフォントのテキストを表示します。
HTML の `<h2>` タグのようなものです。

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.heading("Hello, egui!");
        });
    }
}
```

実行すると、 Window の上部に太字の大きなテキストが表示されます。


## `ui.label()` で本文を表示する

`label` は、通常サイズのテキストを表示します。
HTML の `<p>` タグのようなものです。

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.heading("Title");
            ui.label("This is body text.");
        });
    }
}
```

実行すると、見出しの下に通常のテキストが表示されます。


## 複数のテキストを表示する

`heading` と `label` を組み合わせて、より複雑な画面を作ってみましょう。

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.heading("How to use egui");
            ui.label("egui is a GUI library for Rust.");
            ui.label("It is simple, fast, and easy to use.");
        });
    }
}
```

テキストは上から順に縦に並びます。


## `format!` で動的なテキストを表示する

`label` の中には、`format!` マクロを使って動的な文字列を渡すことができます。

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            let version = "0.35";
            ui.label(format!("egui version: {version}"));
        });
    }
}
```

`{version}` の部分が変数の値で置き換えられます。


## まとめ

* `ui.heading()` — 太字の見出しを表示
* `ui.label()` — 通常のテキストを表示
* 複数のウィジェットは上から順に縦に並ぶ
* `format!` を使うと動的なテキストを表示できる

---

## 課題

<details markdown="1" class="exercise">
  <summary>課題を見る</summary>

以下の要素を含む UI を作ってみましょう。

* 見出し「My First App」
* 本文「I started using egui!」
* 本文「Making GUI with Rust is fun」

---

<details markdown="1">
  <summary>答えを見る</summary>

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.heading("My First App");
            ui.label("I started using egui!");
            ui.label("Making GUI with Rust is fun");
        });
    }
}
```

</details>

</details>
