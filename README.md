# Privacy Policies

App Store 申請に必要なプライバシーポリシーを置くサイト。
GitHub Pages が自動でビルドして公開する。

公開URL: https://sono-hong.github.io/privacy/

## 考え方：ページはアプリごと、部品は共有

**1枚のポリシーを全アプリで使い回すことはしない。**
「何も集めていない」と書いたポリシーは、解析SDKを入れたアプリには当てはまらない。
統合すると、いずれ「どのアプリにも当てはまるが、どれも正確でない」文章に劣化する。

そのかわり、**共通する節は `_includes/` で共有**している。
連絡先やメールアドレスを変えるときは `_config.yml` の1か所だけ直せばよい。

## 構成

```
_config.yml            発行者名・連絡先。ここを直すと全ページに反映される
_layouts/
  policy.html          ポリシーページの外枠。共通の節はここで差し込む
  index.html           一覧ページの外枠
_includes/
  rights.html          ← 全ページに自動で入る
  children.html        ←
  security.html        ←
  changes.html         ←
  contact.html         ←
  no-collection.html   ← 使うページで {% include %} して呼ぶ
  apple-analytics.html ←
  health-disclaimer.html ←
assets/style.css       見た目
index.md               アプリ一覧
go-poop/index.html     Go Poop のポリシー本文
```

`_layouts/policy.html` が末尾に差し込む5つ（rights / children / security / changes / contact）は
**書かなくても全ページに入る**。アプリ固有の内容だけを書けばよい。

## アプリを追加する

1. フォルダを作る（`myapp/index.html`）
2. 先頭に front matter を書く

```yaml
---
layout: policy
app: My App
tagline: what it does
platforms: iPhone
effective: "2026-09-01"
---
```

3. そのアプリ固有の節を書く。共通の節を使うなら呼ぶ

```liquid
{% include no-collection.html %}
{% include apple-analytics.html %}
{% include health-disclaimer.html %}
```

4. `index.md` の一覧に1行足す

`go-poop/index.html` を写して直すのが早い。

## 気をつけること

- **ポリシーは事実でなければならない。** 解析SDKを入れたアプリで
  `no-collection.html` を呼んではいけない。実装を確認してから書く
- **URL を変えない。** App Store Connect に登録したあとにパスを変えると、
  審査でリンク切れとして扱われる。フォルダ名は最初に決めきる
- **`Last updated` を更新する。** 内容を変えたら front matter に
  `updated: "YYYY-MM-DD"` を足す（無ければ `effective` が使われる）

## ローカルで確認する

GitHub Pages が自動ビルドするので、通常は push するだけでよい。
手元で見たい場合は Ruby と Jekyll が要る。

```bash
bundle exec jekyll serve
```
