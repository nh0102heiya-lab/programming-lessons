---
layout: default
title: 最初の Window を表示する
description: eframe で Window を表示する基本構造を学ぶ
order: 2
section: egui-setup
updated: 2026-07-25
---

前回の記事ではプロジェクトを作成し、動作するコードを書ききました。
今回は、そのコードがどういう意味なのかを一つずつ説明します。


## コードの全体像

前回作った `src/main.rs` のコードをもう一度見てみましょう。

```rust
use eframe::egui;

fn main() -> eframe::Result {
    let options = eframe::NativeOptions::default();

    eframe::run_native(
        "my_egui_app",
        options,
        Box::new(|_cc| Ok(Box::new(MyApp::default()))),
    )
}

#[derive(Default)]
struct MyApp {}

impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.heading("Hello, egui!");
        });
    }
}
```

このコードは大きく分けて **2つの部分** に分かれます。

1. `main` 関数 — アプリを起動するための設定
2. `MyApp` 構造体と `App` トレイトの実装 — アプリの中身


## `main` 関数

```rust
fn main() -> eframe::Result {
    let options = eframe::NativeOptions::default();

    eframe::run_native(
        "my_egui_app",
        options,
        Box::new(|_cc| Ok(Box::new(MyApp::default()))),
    )
}
```

### `eframe::Result`

`main` 関数は `eframe::Result` を返します。
アプリの起動に失敗したときのエラーを表します。

### `NativeOptions`

Window の設定を指定します。`default()` を使うと、すべての設定がデフォルト値になります。

```rust
let options = eframe::NativeOptions::default();
```

カスタム設定が必要な場合は、`ViewportBuilder` を使って Window のサイズやタイトルなどを変更できます。

### `run_native`

アプリを起動するための関数です。

```rust
eframe::run_native(
    "my_egui_app",
    options,
    Box::new(|_cc| Ok(Box::new(MyApp::default()))),
)
```

* 第1引数 — アプリの名前（ Window のタイトルや、設定の保存先に使われる）
* 第2引数 — Window のオプション
* 第3引数 — アプリのインスタンスを作成する関数（「クロージャ」と呼ばれる）


## `MyApp` 構造体

```rust
#[derive(Default)]
struct MyApp {}
```

`struct` は「データの形」を定義する仕組みです。
今は中が空ですが、後にアプリの状態（カウンターや入力テキストなど）をここに格納します。

`#[derive(Default)]` は、初期値 (`default()`) を自動で作れるようにする属性です。


## `App` トレイトの実装

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.heading("Hello, egui!");
        });
    }
}
```

### `eframe::App` トレイト

egui でアプリを作るには、`eframe::App` トレイトを実装する必要があります。
トレイトとは「一定のルールを守ることを約束する仕組み」です。

`eframe::App` には **`ui` メソッド** を必ず実装します。

### `ui` メソッド

```rust
fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
    egui::CentralPanel::default().show(ui, |ui| {
        ui.heading("Hello, egui!");
    });
}
```

このメソッドは 1 秒間に約 60 回呼び出され、そのたびに Window の中身を最初から描き直します。

{: .note}
> 即時モード UI は、毎フレーム UI をコードから描画し直す方式です。
> 状態管理やデバッグがシンプルで、ゲームやリアルタイムアプリと相性が良いのが特徴です。
> 
> 一方、保持モード UI は UI を内部で保持するため、複雑なアプリや高度なアクセシビリティに向いています。

引数の意味：

* `&mut self` — アプリ自身（状態を変更するために「可変」の参照を受け取る）
* `ui: &mut egui::Ui` — ウィジェット（ボタンやテキストなど）を配置するための「キャンバス」
* `_frame: &mut eframe::Frame` — Window 自体を制御するための情報（今は使いません）

### `CentralPanel`

```rust
egui::CentralPanel::default().show(ui, |ui| {
    ui.heading("Hello, egui!");
});
```

* `CentralPanel` — Window と同じ大きさの描画領域（今回は Window が空なので全体に広がる）
* `.show(ui, |ui| { ... })` — この中身を画面に表示する
* `ui.heading("Hello, egui!")` — 見出しを表示する


## まとめ

* `main` 関数で Window の設定とアプリの起動を行う
* `MyApp` でアプリの状態を保持する設計
* `eframe::App` トレイトの `ui` メソッドで UI を定義する
* `CentralPanel` は Window と同じ大きさの描画領域（最後に定義する）
