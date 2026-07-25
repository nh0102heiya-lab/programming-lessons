---
layout: default
title: テーマを変える
description: ダークモード / ライトモードの切り替えと色のカスタマイズ
order: 9
section: egui-advanced
updated: 2026-07-25
---

画面の構成方法を学びました。今回は、アプリの **見た目（テーマ）** を変える方法を学びます。


## ダークモードとライトモード

egui にはデフォルトで2つのテーマがあります。

* **ライトモード** — 白い背景に黒いテキスト（デフォルト）
* **ダークモード** — 黒い背景に白いテキスト

`Visuals` を変更することで切り替えられます。


## ダークモードに切り替える

`CreationContext` を使って、起動時にダークモードを設定できます。

```rust
fn main() -> eframe::Result {
    let options = eframe::NativeOptions::default();

    eframe::run_native(
        "my-app",
        options,
        Box::new(|cc| {
            cc.egui_ctx.set_visuals(egui::Visuals::dark());
            Ok(Box::new(MyApp::default()))
        }),
    )
}
```

* `cc.egui_ctx` — egui の設定を変えるための入口
* `set_visuals()` — テーマ（見た目）を設定する
* `Visuals::dark()` — ダークモードのテーマ


## ライトモードに戻す

`Visuals::light()` を使うとライトモードになります。

```rust
let visuals = egui::Visuals::light();
ui.ctx().set_visuals(visuals);
```


## ボタンで切り替える

アプリの中でボタンを押してテーマを切り替えましょう。

```rust
#[derive(Default)]
struct MyApp {
    dark_mode: bool,
}

impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::TopBottomPanel::top("toolbar").show(ui, |ui| {
            if ui.button("Toggle Theme").clicked() {
                self.dark_mode = !self.dark_mode;
                let visuals = if self.dark_mode {
                    egui::Visuals::dark()
                } else {
                    egui::Visuals::light()
                };
                ui.ctx().set_visuals(visuals);
            }
        });

        egui::CentralPanel::default().show(ui, |ui| {
            ui.heading("Theme Toggle Demo");
            ui.label("Use the button above to toggle the theme");
        });
    }
}
```


## カスタムカラーを設定する

テーマの色を個別に変更することもできます。

```rust
fn main() -> eframe::Result {
    let options = eframe::NativeOptions::default();

    eframe::run_native(
        "my-app",
        options,
        Box::new(|cc| {
            let mut visuals = egui::Visuals::dark();
            visuals.override_text_color = Some(egui::Color32::from_rgb(200, 200, 255));
            cc.egui_ctx.set_visuals(visuals);
            Ok(Box::new(MyApp::default()))
        }),
    )
}
```

* `override_text_color` — 全テキストの色を上書き
* `Color32::from_rgb(r, g, b)` — RGB 値で色を指定（各値は 0〜255）


## 色の指定方法

egui には色を指定する方法がいくつかあります。

```rust
// Color32 で直接指定
let red = egui::Color32::from_rgb(255, 0, 0);

// 事前定義された色
let blue = egui::Color32::BLUE;

// 透過色付き
let semi_transparent = egui::Color32::from_rgba_premultiplied(255, 0, 0, 128);
```


## テキストの色を変える

`RichText` を使って、個別のテキストの色を変えることができます。

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.label("Normal text");
            ui.label(egui::RichText::new("Red text").color(egui::Color32::RED));
            ui.label(
                egui::RichText::new("Large blue text")
                    .color(egui::Color32::BLUE)
                    .size(24.0),
            );
        });
    }
}
```


## まとめ

* `cc.egui_ctx.set_visuals()` でテーマを変更できる
* `Visuals::dark()` でダークモード、`Visuals::light()` でライトモード
* `Color32::from_rgb()` で色を指定できる
* `RichText` で個別のテキストの色やサイズを変えられる

---

## 課題

<details markdown="1" class="exercise">
  <summary>課題を見る</summary>

ダークモードで起動し、ボタンでライト/ダークを切り替えられるアプリを作りましょう。

---

<details markdown="1">
  <summary>ヒント</summary>

* 起動時に `Visuals::dark()` を設定する
* ボタンのクリックで `self.dark_mode` を切り替え
* `set_visuals()` で現在のモードに応じたテーマを適用する

</details>

---

<details markdown="1">
  <summary>答えを見る</summary>

```rust
use eframe::egui;

fn main() -> eframe::Result {
    let options = eframe::NativeOptions::default();

    eframe::run_native(
        "theme-demo",
        options,
        Box::new(|cc| {
            cc.egui_ctx.set_visuals(egui::Visuals::dark());
            Ok(Box::new(MyApp { dark_mode: true }))
        }),
    )
}

struct MyApp {
    dark_mode: bool,
}

impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::TopBottomPanel::top("toolbar").show(ui, |ui| {
            let label = if self.dark_mode {
                "Light Mode"
            } else {
                "Dark Mode"
            };
            if ui.button(label).clicked() {
                self.dark_mode = !self.dark_mode;
                let visuals = if self.dark_mode {
                    egui::Visuals::dark()
                } else {
                    egui::Visuals::light()
                };
                ui.ctx().set_visuals(visuals);
            }
        });

        egui::CentralPanel::default().show(ui, |ui| {
            ui.heading("Theme Toggle Demo");
            ui.label(format!(
                "Current: {}",
                if self.dark_mode {
                    "Dark"
                } else {
                    "Light"
                }
            ));
        });
    }
}
```

</details>

</details>
