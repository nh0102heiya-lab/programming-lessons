---
layout: default
title: 実践：TODO アプリを作る
description: これまで学んだことを総合して、自力で TODO アプリを作る
order: 10
section: egui-advanced
updated: 2026-07-25
series_final: true
---

これまでのレッスンで学んだことを全て使い、**TODO アプリ** を自力で作りましょう。
解答は最初は見ずに、自分で考えてみてください。


## 作るもの

* タスクを入力して追加できる
* タスク一覧が表示される
* タスクにチェックを付けられる
* タスクを削除できる


## Step 1: プロジェクトの準備

`cargo new todo-app` でプロジェクトを作り、`Cargo.toml` を編集してください。


## Step 2: データ構造

TODO アプリに必要なデータは何か考えましょう。

* 1つのタスクには「テキスト」と「完了したかどうか」が必要です
* アプリ全体には「入力欄の内容」と「タスクの一覧」が必要です

<details markdown="1">
  <summary>ヒントを見る</summary>

```rust
struct Todo {
    text: String,
    done: bool,
}
```

</details>

<details markdown="1">
  <summary>解答を見る</summary>

```rust
#[derive(Clone)]
struct Todo {
    text: String,
    done: bool,
}

struct TodoApp {
    input: String,
    todos: Vec<Todo>,
}

impl Default for TodoApp {
    fn default() -> Self {
        Self {
            input: String::new(),
            todos: Vec::new(),
        }
    }
}
```

</details>


## Step 3: 起動部分

`main` 関数と `run_native` を書いて、アプリが表示されるようにしましょう。


## Step 4: タイトルバー

画面上部に「TODO App」見出しを表示するパネルを作ってみましょう。

<details markdown="1">
  <summary>ヒントを見る</summary>

`TopBottomPanel::top()` を使います。

</details>

<details markdown="1">
  <summary>解答を見る</summary>

```rust
egui::TopBottomPanel::top("title").show(ui, |ui| {
    ui.heading("TODO App");
});
```

</details>


## Step 5: 入力欄

画面下部にテキスト入力欄と「Add」ボタンを作りましょう。

* Enter キーを押してもタスクが追加されるようにしてください
* 空のテキストは追加しないでください

<details markdown="1">
  <summary>ヒントを見る</summary>

* `text_edit_singleline()` で入力欄
* `ui.input()` でキー入力を検知
* `trim()` で空白を除去

</details>

<details markdown="1">
  <summary>解答を見る</summary>

```rust
egui::TopBottomPanel::bottom("input").show(ui, |ui| {
    ui.horizontal(|ui| {
        ui.text_edit_singleline(&mut self.input);
        if ui.button("Add").clicked() || ui.input(|i| i.key_pressed(egui::Key::Enter)) {
            let text = self.input.trim().to_string();
            if !text.is_empty() {
                self.todos.push(Todo { text, done: false });
                self.input.clear();
            }
        }
    });
});
```

</details>


## Step 6: タスク一覧

CentralPanel にタスク一覧を表示しましょう。

* タスクが0個のときは「Please add a task」と表示
* 各タスクにはチェックボックスと削除ボタンを付ける
* 完了済みタスクは取り消し線を引く

<details markdown="1">
  <summary>ヒントを見る</summary>

* `ui.checkbox()` でチェックボックス
* `RichText::new().strikethrough()` で取り消し線
* 削除は `Vec::remove()` で行うが、ループ中に削除するとインデックスがずれるので注意

</details>

<details markdown="1">
  <summary>解答を見る</summary>

```rust
egui::CentralPanel::default().show(ui, |ui| {
    if self.todos.is_empty() {
        ui.label("Please add a task");
    } else {
        let mut to_remove = None;
        for (i, todo) in self.todos.iter().enumerate() {
            ui.horizontal(|ui| {
                ui.checkbox(&mut self.todos[i].done, "");
                let text = if todo.done {
                    egui::RichText::new(&todo.text).strikethrough().weak()
                } else {
                    egui::RichText::new(&todo.text)
                };
                ui.label(text);

                if ui.small_button("Delete").clicked() {
                    to_remove = Some(i);
                }
            });
        }
        if let Some(i) = to_remove {
            self.todos.remove(i);
        }
    }
});
```

</details>


## このコードで学んだこと

| 機能 | 使ったもの |
|------|-----------|
| パネル | `TopBottomPanel`, `CentralPanel` |
| テキスト入力 | `text_edit_singleline()` |
| ボタン | `ui.button()`, `ui.small_button()` |
| チェックボックス | `ui.checkbox()` |
| レイアウト | `ui.horizontal()` |
| 状態管理 | `struct`, `Vec<Todo>` |
| キー入力 | `ui.input()` |
| テキスト装飾 | `RichText::new().strikethrough().weak()` |


## 発展課題

さらに機能を追加してみましょう。

* タスクに優先度（高/中/低）を付けられる
* 優先度でソートできる
* タスクをフィルタリングできる（全部 / 未完了 / 完了済み）
* タスクの編集機能


## まとめ

* egui を使えば、Rust でシンプルに GUI アプリを作れる
* 状態管理が分かりやすい
* egui にはまだまだたくさんのウィジェットと機能がある
* 公式ドキュメントやデモサイトでさらに学べる

---

## おわりに

このコースでは egui の基本を学びました。

egui の公式リソースでは、さらに多くの機能やサンプルが公開されています。

* <a href="https://docs.rs/egui/latest/egui/" target="_blank" rel="noopener noreferrer">egui API ドキュメント</a>
* <a href="https://www.egui.rs/" target="_blank" rel="noopener noreferrer">egui Web デモ</a>
* <a href="https://github.com/emilk/egui" target="_blank" rel="noopener noreferrer">egui GitHub リポジトリ</a>

GUI 開発の楽しさを感じていただけたら幸いです。
