---
layout: default
title: パネルで画面を分割する
description: TopBottomPanel, SidePanel, CentralPanel で画面を構成する
order: 8
section: egui-layout
updated: 2026-07-25
---

ウィジェットとレイアウトの基本を学びました。今回は、画面を **パネルで分割** して、より実用的な UI を作ります。


## パネルの種類

egui には3つのパネルがあります。

| パネル | 用途 |
|--------|------|
| `TopBottomPanel` | 上端または下端 |
| `SidePanel` | 左端または右端 |
| `CentralPanel` | 空いている場所を占領する領域 |


## 上部パネルを作る

`TopBottomPanel::top()` で画面上部にパネルを作ります。

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::TopBottomPanel::top("top_panel").show(ui, |ui| {
            ui.heading("Header Bar");
        });

        egui::CentralPanel::default().show(ui, |ui| {
            ui.label("This is the central area");
        });
    }
}
```

{: .note}
> `CentralPanel` は「残り物を全て占領する」特性を持つため、
> 先に定義すると他のパネルが置ける領域がなくなってしまいます。
> 気をつけましょう。


## 下部パネルを作る

`TopBottomPanel::bottom()` で画面下部にパネルを作ります。

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::TopBottomPanel::bottom("bottom_panel").show(ui, |ui| {
            ui.label("Status Bar");
        });

        egui::CentralPanel::default().show(ui, |ui| {
            ui.label("This is the central area");
        });
    }
}
```


## サイドパネルを作る

`SidePanel::left()` で画面左にパネルを作ります。

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::SidePanel::left("side_panel")
            .default_width(150.0)
            .show(ui, |ui| {
                ui.heading("Menu");
                ui.separator();
                ui.label("Item 1");
                ui.label("Item 2");
                ui.label("Item 3");
            });

        egui::CentralPanel::default().show(ui, |ui| {
            ui.label("This is the central area");
        });
    }
}
```


## メニューバーを作る

上部パネルにメニューバーを配置しましょう。

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::TopBottomPanel::top("menu_bar").show(ui, |ui| {
            egui::menu::bar(ui, |ui| {
                ui.menu_button("File", |ui| {
                    if ui.button("New File").clicked() {
                        // 新規作成の処理
                    }
                    if ui.button("Quit").clicked() {
                        ui.ctx().send_viewport_cmd(egui::ViewportCommand::Close);
                    }
                });

                ui.menu_button("Help", |ui| {
                    ui.label("egui Sample App");
                });
            });
        });

        egui::CentralPanel::default().show(ui, |ui| {
            ui.label("Select a command from the menu");
        });
    }
}
```

* `egui::menu::bar()` — メニューバーの水平レイアウト
* `ui.menu_button()` — ドロップダウンメニュー
* `ViewportCommand::Close` — Window を閉じるコマンド


## 実践的な画面構成

上部にメニューバー、左にサイドパネル、中央にコンテンツを持つアプリを作りましょう。

### `selectable_label()` で選択可能なラベル

`selectable_label()` は、クリックできるラベルです。選択されているかどうかで見た目が変わります。

```rust
if ui.selectable_label(self.selected == "home", "Home").clicked() {
    self.selected = "home".to_string();
}
```

* 第1引数 — 選択されているかどうか（`true` でハイライトされる）
* 第2引数 — 表示するテキスト

### コード例

```rust
#[derive(Default)]
struct MyApp {
    selected: String,
}

impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        // 上部: メニューバー
        egui::TopBottomPanel::top("menu").show(ui, |ui| {
            egui::menu::bar(ui, |ui| {
                ui.menu_button("File", |ui| {
                    if ui.button("Quit").clicked() {
                        ui.ctx().send_viewport_cmd(egui::ViewportCommand::Close);
                    }
                });
            });
        });

        // 左: サイドパネル
        egui::SidePanel::left("side")
            .default_width(120.0)
            .show(ui, |ui| {
                ui.heading("Menu");
                ui.separator();

                if ui
                    .selectable_label(self.selected == "home", "Home")
                    .clicked()
                {
                    self.selected = "home".to_string();
                }
                if ui
                    .selectable_label(self.selected == "settings", "Settings")
                    .clicked()
                {
                    self.selected = "settings".to_string();
                }
            });

        // 中央: コンテンツ
        egui::CentralPanel::default().show(ui, |ui| match self.selected.as_str() {
            "home" => {
                ui.heading("Home");
                ui.label("Welcome!");
            }
            "settings" => {
                ui.heading("Settings");
                ui.label("This is the settings screen");
            }
            _ => {
                ui.label("Select from the menu");
            }
        });
    }
}
```

* `selectable_label()` — クリックできるラベル（選択状態を視覚的に示せる）
* `match` で選択された項目に応じて表示内容を切り替え


## まとめ

* `TopBottomPanel::top()` / `bottom()` — 上端/下端パネル
* `SidePanel::left()` / `right()` — 左端/右端パネル
* `CentralPanel` — 空いている場所を占領する描画領域（最後に定義する）
* `egui::menu::bar()` でメニューバーを作れる
* `ViewportCommand::Close` で Window を閉じられる

---

## 課題

<details markdown="1" class="exercise">
  <summary>課題を見る</summary>

以下の画面構成を持つアプリを作りましょう。

* 上部: 「TODO アプリ」というタイトルのパネル
* 左: 「未完了」「完了済み」の2つのメニュー
* 中央: 選択に応じて「未完了のタスク一覧」「完了したタスク一覧」を表示

---

<details markdown="1">
  <summary>ヒント</summary>

* `SidePanel::left()` で2つの `selectable_label` を置く
* `self.selected` で選択状態を管理
* `CentralPanel` の中で `match` を使って表示を切り替える

</details>

---

<details markdown="1">
  <summary>答えを見る</summary>

```rust
use eframe::egui;

fn main() -> eframe::Result {
    let options = eframe::NativeOptions::default();

    eframe::run_native(
        "todo-app",
        options,
        Box::new(|_cc| Ok(Box::new(MyApp::default()))),
    )
}

#[derive(Default)]
struct MyApp {
    view: String,
}

impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::TopBottomPanel::top("title").show(ui, |ui| {
            ui.heading("TODO App");
        });

        egui::SidePanel::left("nav")
            .default_width(120.0)
            .show(ui, |ui| {
                if ui
                    .selectable_label(self.view == "pending", "Pending")
                    .clicked()
                {
                    self.view = "pending".to_string();
                }
                if ui
                    .selectable_label(self.view == "done", "Done")
                    .clicked()
                {
                    self.view = "done".to_string();
                }
            });

        egui::CentralPanel::default().show(ui, |ui| match self.view.as_str() {
            "pending" => {
                ui.heading("Pending Tasks");
                ui.label("[ ] Go shopping");
                ui.label("[ ] Write report");
            }
            "done" => {
                ui.heading("Completed Tasks");
                ui.label("[x] Submit manuscript");
            }
            _ => {
                ui.label("Select from the menu");
            }
        });
    }
}
```

</details>

</details>
