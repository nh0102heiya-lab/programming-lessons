---
layout: default
title: プロジェットを準備する
description: eframe プロジェットの作り方と環境構築
order: 1
section: egui-setup
updated: 2026-07-25
---

この章では、egui を使って GUI アプリを作るための準備を行います。
ローカルの開発環境で、最初のプロジェクトを動かすところまでを説明します。

{: .note}
> この記事は **eframe 0.35**（egui 0.35）用です。
> Rust 1.92.0 以上が必要です。

## プロジェクトを作る

`cargo new` で新しいプロジェクトを作ります。

```console
$ cargo new my_egui_app
     Created binary (application) `my_egui_app` package
$ cd my_egui_app
```

作成されたファイル構成は以下のようになります。

```
my_egui_app/
├── Cargo.toml
└── src/
    └── main.rs
```

## Cargo.toml を編集する

`Cargo.toml` を開き、`[dependencies]` セクションに eframe を追加します。

```toml
[package]
name = "my_egui_app"
version = "0.1.0"
edition = "2024"

[dependencies]
eframe = "0.35"
```

- **eframe** — egui を使ってアプリを動かすためのフレームワーク

## main.rs を書く

`src/main.rs` を開き、以下のコードに書き換えます。

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

各行の意味は次の章で詳しく説明します。

## ビルドと実行

ターミナルで以下を実行します。

```console
$ cargo run
```

初回はダウンロードとコンパイルにまぁまぁ時間がかかります。。。気長に待ちましょう！

完了すると、Window が表示され「Hello, egui!」と表示されます。

## まとめ

- `cargo new` でプロジェクトを作成した
- `Cargo.toml` に eframe を追加した
- `cargo run` でアプリを実行した
