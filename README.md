# zenn-claude-desktop-template

Zenn の記事を [zenn-cli](https://zenn.dev/zenn/articles/zenn-cli-guide) と Claude Code Desktop で書くためのテンプレートリポジトリです。

- Claude Code Desktop のセッションごとに worktree が作られ、記事ごとに並行して執筆できます
- `Cmd + Shift + P` で zenn preview を開き、公開時と同じ見た目で記事を確認できます。複数のセッションでプレビューを開いてもポートは衝突しません
- 下書きは PR で main にマージし、公開するときは `/publish` スキルで `published: true` に書き換えて push します

## 始め方

1. このリポジトリの「Use this template」から自分のリポジトリを作る
2. Zenn のダッシュボードの「GitHub からのデプロイ」から、作ったリポジトリを連携する（[公式ガイド](https://zenn.dev/zenn/articles/connect-to-github)）
3. Claude Code Desktop で、作ったリポジトリを選んでセッションを作る
4. Claude に記事を書いてもらう（自分で雛形を作る場合は `pnpm exec zenn new:article`）
5. `Cmd + Shift + P` でプレビューを開き、記事の見た目を確認する
6. PR を作って下書きを main にマージし、公開したいタイミングで `/publish` を実行する

pnpm が必要です。pnpm のバージョンは `package.json` の `packageManager` で固定しています。依存パッケージは、worktree でセッションを開始したときに hook で自動的にインストールされます。

## 含まれているファイル

```
.
├── .claude
│   ├── launch.json      # Claude Code Desktop のプレビューで zenn preview を起動する設定
│   ├── settings.json    # worktree でセッションを開始したときに pnpm install する hook
│   └── skills
│       └── publish      # 記事を公開するスキル
│           └── SKILL.md
├── articles
├── books
├── LICENSE
├── package.json
├── pnpm-lock.yaml
└── pnpm-workspace.yaml
```

| ファイル | 役割 |
| --- | --- |
| `.claude/launch.json` | Claude Code Desktop のプレビューで `pnpm run preview` を起動します。`autoPort: true` により、ポートが使用中なら空きポートが `PORT` 環境変数で渡されます |
| `.claude/settings.json` | worktree でセッションを開始したときに `pnpm install` を実行する `SessionStart` hook です |
| `.claude/skills/publish/SKILL.md` | `published: false` の記事を一覧から選び、`published: true` に書き換えてコミット・push するスキルです |
| `package.json` | `preview` スクリプト（`zenn preview --port ${PORT:-8000}`）と zenn-cli への依存、`packageManager` を定義しています |
| `pnpm-workspace.yaml` | pnpm の設定です。`minimumReleaseAge` で公開から 3 日経っていないパッケージをインストールしないようにしています。`confirmModulesPurge: false` は、worktree の `node_modules` を作り直すときに確認プロンプトで止まらないようにする設定です |
| `articles/`, `books/` | 記事と本を置くディレクトリです |
| `LICENSE` | このテンプレートのライセンス（MIT-0）です。テンプレートから作ったリポジトリでは、削除したり自分のライセンスに置き換えたりして構いません |

## ライセンス

[MIT-0](LICENSE) です。著作権表示なしで自由に複製・改変・再配布できます。

## 参考

- [Zenn CLIで記事・本を管理する方法](https://zenn.dev/zenn/articles/zenn-cli-guide)
- [アカウントにGitHubリポジトリを連携してZennのコンテンツを管理する](https://zenn.dev/zenn/articles/connect-to-github)
