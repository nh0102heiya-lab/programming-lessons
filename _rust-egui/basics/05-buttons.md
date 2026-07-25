---
layout: default
title: ボタンをつくる
description: ui.button() でボタンを作り、クリックに反応する
order: 5
section: egui-basics
updated: 2026-07-25
---

変数と画面の表示方法を学びました。今回は、ユーザーが**クリックできるボタン**を追加します。


## ボタンを作る

`ui.button()` でボタンを作ります。

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.heading("Counter");
            if ui.button("Click").clicked() {
                // クリックされたときの処理
            }
        });
    }
}
```

`ui.button("Click")` がボタンを表示し、`.clicked()` でクリックされたかどうかを判定します。


## カウンターアプリを作る

ボタンと変数を組み合わせて、カウンターアプリを作りましょう。

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.heading("Counter");
            let count = self.count;
            ui.label(format!("Count: {count}"));
            if ui.button("+").clicked() {
                self.count += 1;
            }
        });
    }
}
```

### このコードの流れ

1. `self.count` の値を画面に表示する
2. `+` ボタンを表示する
3. ボタンがクリックされたら `self.count` を1増やす
4. 次のフレームで増えた値が表示される

ボタンをクリックするたびに数字が1ずつ増えます。


## ボタンを2つ作る

`+` と `-` のボタンを作って、増減できるようにしましょう。

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.heading("Counter");
            let count = self.count;
            ui.label(format!("Count: {count}"));

            ui.horizontal(|ui| {
                if ui.button("-").clicked() {
                    self.count -= 1;
                }
                if ui.button("+").clicked() {
                    self.count += 1;
                }
            });
        });
    }
}
```

`ui.horizontal(|ui| { ... })` を使うと、ボタンが横に並びます。
これは次の章で詳しく説明します。


## `Response` を使う

ボタンの `.clicked()` は、ボタンの操作結果を返す `Response` という型のメソッドです。

```rust
let response = ui.button("Click");
if response.clicked() {
    // ...
}
```

`Response` には他にも便利なメソッドがあります。

* `.clicked()` — クリックされたかどうか
* `.hovered()` — マウスが乗っているかどうか
* `.on_hover_text("...")` — ホバー時にツールチップを表示

```rust
if ui.button("Warning").on_hover_text("Please do not click").clicked() {
    // ...
}
```


## まとめ

* `ui.button("テキスト")` でボタンを表示する
* `.clicked()` でクリックを検知する
* クリックされたら `self` を変更し、次のフレームに反映される
* `ui.horizontal()` でボタンを横に並べられる

---

## 課題

<details markdown="1" class="exercise">
  <summary>課題を見る</summary>

カウンターアプリに「リセット」ボタンを追加してください。
リセットボタンを押すと `count` が `0` に戻ります。

---

<details markdown="1">
  <summary>ヒント</summary>

* `ui.button("Reset")` を追加する
* `.clicked()` したら `self.count = 0;` にする

</details>

---

<details markdown="1">
  <summary>答えを見る</summary>

```rust
impl eframe::App for MyApp {
    fn ui(&mut self, ui: &mut egui::Ui, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ui, |ui| {
            ui.heading("Counter");
            let count = self.count;
            ui.label(format!("Count: {count}"));

            ui.horizontal(|ui| {
                if ui.button("-").clicked() {
                    self.count -= 1;
                }
                if ui.button("+").clicked() {
                    self.count += 1;
                }
                if ui.button("Reset").clicked() {
                    self.count = 0;
                }
            });
        });
    }
}
```

</details>

</details>
