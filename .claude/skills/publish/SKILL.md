---
name: publish
description: Zenn リポジトリの articles/ にある published: false の記事を一覧表示し、ユーザーが選択した記事を published: true に変更してコミットするスキル。「記事を公開して」「publish したい」「published にして」「Zenn に公開」「記事を公開する」といった依頼で発動する。公開対象の記事が特定されていない場合でも、Zenn の記事公開フローに関連する依頼には積極的にこのスキルを使う。
---

# Publish Zenn Article

## 概要

Zenn リポジトリの `articles/` ディレクトリにある未公開記事（`published: false`）を一覧し、ユーザーが選択した記事を公開状態（`published: true`）に変更してコミットする。

## ワークフロー

### Step 1: 未公開記事の収集

`articles/` ディレクトリの全 `.md` ファイルを走査し、フロントマターに `published: false` を含む記事を抽出する。

```bash
grep -rl "published: false" articles/
```

抽出した各ファイルから `title` フィールドも読み取り、記事の一覧を作成する。

### Step 2: ユーザーへの選択提示

`AskUserQuestion` ツールを使い、未公開記事の一覧をユーザーに提示して公開対象を選ばせる。

- 選択肢の label は `「{title}」({filename})` の形式にする
- 記事が 0 件の場合は「公開待ちの記事はありません」と伝えて終了する

### Step 3: `published: false` → `published: true` に変更

選択されたファイルの `published: false` を `published: true` に書き換える。

Edit ツールを使う:
- `old_string`: `published: false`
- `new_string`: `published: true`

### Step 4: コミット

以下の形式でコミットする（CLAUDE.md のコミットルールに従う）:

```
feat: publish article "{title}"
```

コミットメッセージの本文（2行目以降）は不要。

### Step 5: push

コミット後、そのまま `git push` を実行する。

push 完了後、「`{title}` を公開しました」と報告する。

## 注意事項

- 作業対象は常に `articles/` ディレクトリ。`books/` は対象外。
- 複数記事を一度に公開するケースは想定しない（1回の実行で1記事）。
