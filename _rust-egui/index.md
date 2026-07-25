---
layout: index
title: Rust egui
permalink: /rust-egui/
description: egui を使った Rust GUI アプリ開発入門
---

# Rust egui

この教材は、Rust で GUI アプリケーションを開発するための入門です。

egui（イーグゥイ）は、Rust 製の GUI ライブラリです。
Windows、Mac、Linux のデスクトップアプリはもちろん、Web ブラウザ向けのアプリも作ることができます。

---

## 前提知識

このコースを始める前に、Rust の基本的な文法を理解していることが望ましいです。

<a href="{{ '/rust-from-zero/' | relative_url }}">Rust from Zero</a> の「基本を動かす」「データと型」の章を完了しているとスムーズに進みます。

## 使用するバージョン

この教材では **eframe 0.35**（egui 0.35）を使用します。
Rust 1.92.0 以上が必要です。

## 章構成

- <a href="{{ '/rust-egui/setup/01-setup' | relative_url }}">第1章: 準備</a> — プロジェットの作成と環境構築
- <a href="{{ '/rust-egui/basics/03-hello-world' | relative_url }}">第2章: Window とウィジェット</a> — テキスト表示、変数、ボタン
- <a href="{{ '/rust-egui/layout/06-layout' | relative_url }}">第3章: レイアウトと画面構成</a> — ウィジェットの並べ方、パネル分割
- <a href="{{ '/rust-egui/advanced/09-theme' | relative_url }}">第4章: 仕上げ</a> — テーマ切り替え、実践演習
