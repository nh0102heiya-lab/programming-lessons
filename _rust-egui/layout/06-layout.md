---
layout: default
title: レイアウトを工夫する
description: horizontal, vertical, separator, group, collapsing で画面を整える
order: 6
section: egui-layout
updated: 2026-07-25
---

ボタンの使い方を学びました。今回は、ウィジェットを **どう並べるか** を学びます。
egui には便利なレイアウト機能がたくさんあります。


## `ui.horizontal()` で横に並べる

`ui.horizontal()` の中に入れたウィジェットは、**横に並び**ます。

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.horizontal(|ui| {
                ui.label("First");
                ui.label("Middle");
                ui.label("Last");
            });
        });
    }
}
```

実行すると、3つのラベルが横に並びます。

```
 First Middle Last
```


## `ui.vertical()` で縦に並べる

`ui.vertical()` の中に入れたウィジェットは、**縦に並び**ます。
実際には、デフォルトで縦に並ぶので、明示的に使うことは少ないです。

```rust
ui.vertical(|ui| {
    ui.label("Top");
    ui.label("Middle");
    ui.label("Bottom");
});
```


## ネストする

レイアウトの中にレイアウトを入れることができます。

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.heading("Layout Example");

            ui.horizontal(|ui| {
                ui.label("Name:");
                ui.label("Alice");
            });

            ui.horizontal(|ui| {
                ui.label("Age:");
                ui.label("25");
            });
        });
    }
}
```

実行結果：

```
 レイアウトの例
 名前: Alice
 年齢: 25
```


## `ui.separator()` で区切り線を入れる

`separator()` は水平な区切り線を表示します。

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.label("Above");
            ui.separator();
            ui.label("Below");
        });
    }
}
```


## `ui.group()` でグループ化する

`group()` はウィジェットを箱で囲みます。

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.group(|ui| {
                ui.label("This is a group");
                ui.button("Button");
            });
        });
    }
}
```


## `ui.collapsing()` で折りたたみ表示

`collapsing()` はヘッダーをクリックすると中身が表示/非表示になるウィジェットです。

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.collapsing("Show Details", |ui| {
                ui.label("Details are shown here");
                ui.label("Click to toggle");
            });
        });
    }
}
```


## 実践的なレイアウト

これまでの機能を組み合わせて、情報カードのような UI を作ってみましょう。

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.heading("User Info");

            ui.group(|ui| {
                ui.horizontal(|ui| {
                    ui.label("Name:");
                    ui.label("Alice");
                });
                ui.horizontal(|ui| {
                    ui.label("Age:");
                    ui.label("25");
                });
                ui.separator();
                ui.collapsing("Details", |ui| {
                    ui.label("Job: Engineer");
                    ui.label("Location: Tokyo");
                });
            });
        });
    }
}
```


## まとめ

* `ui.horizontal()` — ウィジェットを横に並べる
* `ui.vertical()` — ウィジェットを縦に並べる
* `ui.separator()` — 区切り線を入れる
* `ui.group()` — ウィジェットを箱で囲む
* `ui.collapsing()` — 折りたたみ表示を作る
* レイアウトはネストできる

---

## 課題

<details markdown="1" class="exercise">
  <summary>課題を見る</summary>

以下のレイアウトを持つ UI を作ってみましょう。

```
 タスク一覧
 ─────────
 □ 買い物に行く
 □ レポートを書く
 ─────────
 [追加] ボタン
```

* 「タスク一覧」は見出し
* 各タスクは `ui.label()` で表示
* 区切り線で上と下を分ける
* 「追加」ボタンは `ui.button()` で作る（まだクリック動作は不要）

---

<details markdown="1">
  <summary>答えを見る</summary>

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.heading("Task List");
            ui.separator();
            ui.label("[ ] Go shopping");
            ui.label("[ ] Write report");
            ui.separator();
            ui.button("Add");
        });
    }
}
```

</details>

</details>
