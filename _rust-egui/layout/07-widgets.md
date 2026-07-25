---
layout: default
title: その他のウィジェット
description: checkbox, slider, text_edit, Grid を使う
order: 7
section: egui-layout
updated: 2026-07-25
---

レイアウトの基本を学びました。今回は、入力用のウィジェットをいくつか紹介します。


## `ui.checkbox()` でチェックボックス

チェックボックスは、`bool` 型の値を On/Off するのに使います。

```rust
#[derive(Default)]
struct MyApp {
    show_detail: bool,
}

impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.checkbox(&mut self.show_detail, "Show Details");

            if self.show_detail {
                ui.label("Details are shown here");
            }
        });
    }
}
```

* `&mut self.show_detail` — チェックボックスの状態を `show_detail` に連動
* チェックを入れると `true`、外すと `false` になる


## `ui.add(egui::Slider)` でスライダー

スライダーは数値を範囲内で選択するのに使います。

```rust
#[derive(Default)]
struct MyApp {
    volume: f32,
}

impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.heading("Volume Settings");
            ui.add(egui::Slider::new(&mut self.volume, 0.0..=100.0).text("Volume"));
            let volume = self.volume as i32;
            ui.label(format!("Current volume: {volume}"));
        });
    }
}
```

* `0.0..=100.0` — 選べる範囲
* `.text("Volume")` — スライダーの横に表示するラベル


## `ui.text_edit_singleline()` でテキスト入力

テキスト入力欄は、ユーザーが文字列を入力できるウィジェットです。

```rust
#[derive(Default)]
struct MyApp {
    name: String,
}

impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.heading("Name Input");
            ui.text_edit_singleline(&mut self.name);

            if !self.name.is_empty() {
                let name = &self.name;
                ui.label(format!("Hello, {name}!"));
            }
        });
    }
}
```

* `&mut self.name` — 入力された文字列が `name` に保存される
* 入力のたびに `self.name` が更新され、次のフレームで表示が変わる


## `egui::Grid` でフォーム風レイアウト

`Grid` を使うと、ウィジェットを表のように揃えられます。

```rust
#[derive(Default)]
struct MyApp {
    name: String,
    age: f32,
    email: String,
}

impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.heading("User Registration");

            egui::Grid::new("my_grid").show(ui, |ui| {
                ui.label("Name:");
                ui.text_edit_singleline(&mut self.name);
                ui.end_row();

                ui.label("Age:");
                ui.add(egui::Slider::new(&mut self.age, 0.0..=120.0).text("years"));
                ui.end_row();

                ui.label("Email:");
                ui.text_edit_singleline(&mut self.email);
                ui.end_row();
            });
        });
    }
}
```

* `egui::Grid::new("my_grid")` — グリッドを作成（名前をつける必要がある）
* `ui.end_row()` — 行の終わりを告げる（これを呼ばないと列が揃わない）


## ウィジェットの組み合わせ

これまでのウィジェットを全部使って、設定画面を作りましょう。

```rust
#[derive(Default)]
struct MyApp {
    dark_mode: bool,
    volume: f32,
    username: String,
}

impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.heading("Settings");

            ui.checkbox(&mut self.dark_mode, "Dark Mode");
            ui.add(egui::Slider::new(&mut self.volume, 0.0..=100.0).text("Volume"));

            ui.separator();

            ui.label("Username:");
            ui.text_edit_singleline(&mut self.username);

            if !self.username.is_empty() {
                let username = &self.username;
                ui.label(format!("Logged in: {username}"));
            }
        });
    }
}
```


## まとめ

* `ui.checkbox()` — チェックボックス（`bool` 値を On/Off）
* `egui::Slider` — スライダー（数値を範囲で選択）
* `ui.text_edit_singleline()` — テキスト入力欄
* `egui::Grid` — ウィジェットを表形式で揃える

---

## 課題

<details markdown="1" class="exercise">
  <summary>課題を見る</summary>

簡易計算機の設定画面を作りましょう。

* `Slider` で「割引率」を 0〜50% の範囲で選べる
* `text_edit_singleline` で「商品名」を入力できる
* `checkbox` で「税込み」の On/Off ができる

---

<details markdown="1">
  <summary>ヒント</summary>

* 各フィールドを `struct` のフィールドとして定義する
* `egui::Grid` でフォーム風に揃える

</details>

---

<details markdown="1">
  <summary>答えを見る</summary>

```rust
use eframe::egui;

fn main() -> eframe::Result {
    let options = eframe::NativeOptions::default();

    eframe::run_native(
        "settings",
        options,
        Box::new(|_cc| Ok(Box::new(MyApp::default()))),
    )
}

#[derive(Default)]
struct MyApp {
    discount: f32,
    product_name: String,
    include_tax: bool,
}

impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.heading("Product Settings");

            egui::Grid::new("settings_grid").show(ui, |ui| {
                ui.label("Product:");
                ui.text_edit_singleline(&mut self.product_name);
                ui.end_row();

                ui.label("Discount:");
                ui.add(egui::Slider::new(&mut self.discount, 0.0..=50.0).text("%"));
                ui.end_row();

                ui.label("Tax included:");
                ui.checkbox(&mut self.include_tax, "");
                ui.end_row();
            });
        });
    }
}
```

</details>

</details>
