# CCCP - Claude Code Command Pack

Claude Code用のプラグインで、Git操作スペシャリストエージェント、Gitワークフローコマンド、タスク計画・実行ワークフロー（todo-task-planning/todo-task-run）を提供します。

## 概要

このプラグインはClaude Codeにタスク計画・実行ワークフロー（todo-task-planning/todo-task-run）を中心とした統合開発支援を提供し、Git操作コマンドで補強します：

| 機能 | 種類 | 説明 | オプション |
|------|------|------|---------|
| **git-operations-specialist** | Agent | 履歴分析、競合解決、ブランチ戦略、GitHub CLI操作を含む高度なGit操作に対応したエキスパート | - |
| **project-manager** | Agent | プロジェクト管理とタスク組織のスペシャリスト | - |
| **commit** | Command | 適切なコミットメッセージでステージされた変更をコミット | - |
| **micro-commit** | Command | Lucas Rochaのマイクロコミット方法に従った細粒度のコミットを作成 | - |
| **pull-request** | Command | 現在のブランチに対するプルリクエストを作成または更新 | `<issue-number>` - Issue番号を指定 |
| **todo-task-planning** | Command | TODOリストでタスクを計画・整理 | `<filepath>` - TODOファイルパス<br>`--pr` - PR作成<br>`--branch [name]` - ブランチ作成 |
| **todo-task-run** | Command | TODOリストから計画されたタスクを実行 | `<filepath>` - TODOファイルパス<br>`--no-pr` - PR作成なし |

## 前提条件

- [Claude Code](https://claude.com/claude-code)がインストール済み
- Git（バージョン2.0以上）
- GitHub操作用の[GitHub CLI (gh)](https://cli.github.com/)

## インストール

### マーケットプレイスから（推奨）

Claude Code で以下のコマンドを実行して、gendosu-claude-plugins マーケットプレイスを追加し、CCCP プラグインをインストールします：

**Step 1: gendosu-claude-plugins マーケットプレイスを追加**
```bash
/plugin marketplace add gendosu/gendosu-claude-plugins
```

**Step 2: CCCP プラグインをインストール**
```bash
/plugin install cccp@gendosu-claude-plugins
```

または、対話的インターフェースから：
```bash
/plugin
```

`Discover` タブで `cccp` を検索し、インストールします。

### ソースから

このリポジトリをクローン：
```bash
git clone https://github.com/gendosu/agent-skills.git
```

次に、Claude Codeでプラグインをインストール：
```bash
/plugin install /path/to/agent-skills
```

## スキル

このプラグインは、タイプ別に整理された統合されたスキルコレクションを提供します：

### エージェントスキル

エージェントスキルは、専門タスクのために特定のモデルを使用し、独立したコンテキストで実行されます：

#### Git操作スペシャリスト (`/git-operations-specialist`)

以下を含むエキスパートGit操作：

- **Git履歴分析**: コミット履歴、ブランチ関係、ファイル変更を分析
- **競合解決**: 適切な戦略でマージ競合を解決するガイダンス
- **ブランチ戦略**: GitFlowやGitHub Flowなどのブランチワークフローを推奨・実装
- **高度なGit操作**: インタラクティブリベース、チェリーピック、スタッシュ管理、reflog操作
- **GitHub CLI操作**: PR作成・管理、Issue追跡、API操作

#### プロジェクトマネージャー (`/project-manager`)

プロジェクト管理とタスク組織のスペシャリスト。

### コマンドスキル

コマンドスキルは、ワークフロー操作と開発自動化を提供します：

#### コミットコマンド (`/commit`)
- 適切なコミットメッセージでステージされた変更をコミット
- Conventional Commitフォーマットとプロジェクトガイドラインに従う

#### マイクロコミットコマンド (`/micro-commit`)
- テスト駆動開発サイクルに従った細粒度のコミットを作成
- 関連する変更を論理的にグループ化
- 清潔で意味のあるコミット履歴を保持
- 1コミット1変更の原則に従う

#### プルリクエストコマンド (`/pull-request`)
- 現在のブランチに対して新しいプルリクエストを作成
- 既存のプルリクエストを最新の変更で更新
- プルリクエストをGitHub Issueにリンク
- PRのタイトルと説明を自動生成

#### Todoタスクワークフロー

このプラグインは、タスク管理のための2段階ワークフローを提供します：

**第1段階:計画 (`/todo-task-planning`)**
- 要件を分析して実行可能なタスクに変換
- チェックボックス形式のTODOリストを構造化して作成
- TDD方法論を使用して複雑な要件を分解
- 明確なタスク依存関係と優先順位を定義

**第2段階:実行 (`/todo-task-run`)**
- 事前作成されたTODO.mdファイルからタスクを実行
- Git操作（ブランチ作成、コミット、プッシュ）を管理
- タスク進捗でプルリクエストを作成・更新
- チェックボックスステータスを更新して完了を追跡

**ワークフロー図：**
```
要件 → /todo-task-planning → TODO.md → /todo-task-run → プルリクエスト
```

**実装例：**

**Step 1: TODO.mdを作成** - やりたいことを記載
```markdown
ログインページのemail項目の名前をaccountに変更
```

**Step 2: タスク計画を実行**
```bash
/todo-task-planning TODO.md
```
このコマンドが要件を分析して、実行可能なタスクリストを自動生成します。

**Step 3: タスク実行を実行**
```bash
/todo-task-run TODO.md
```
このコマンドが生成されたタスクを実行します。

### テンプレートスキル

テンプレートスキルは、リファレンス資料と標準フォーマットを提供します：

#### キーガイドライン (`/key-guidelines`)
- コア開発ガイドラインとベストプラクティス
- 一貫した開発標準のためのリファレンス資料

#### Todoアウトプットテンプレート (`/todo-output-template`)
- 標準的なTODO.mdフォーマット仕様
- 一貫したタスク計画構造を保証

## 使用方法

すべてのスキルはスラッシュコマンド構文で呼び出します:

### エージェントスキル

エージェントスキルは、明示的またはコンテキストに基づいて自動的に呼び出すことができます：

```
/git-operations-specialist        # Git操作スペシャリストを明示的に呼び出し
/project-manager                  # プロジェクトマネージャーを明示的に呼び出し

# エージェントスキルはコンテキストに基づいて自動的に呼び出されます：
"このフィーチャーブランチのgit履歴を分析して"
"このマージ競合の解決を手伝って"
"このプロジェクトのブランチ戦略を提案して"
```

### コマンドスキル

コマンドスキルはオプションの引数とともに直接呼び出します：

```
/commit                           # ステージされた変更をコミット
/micro-commit                     # 細粒度のコミットを作成
/pull-request                     # プルリクエストを作成または更新
/pull-request 123                 # Issue #123にリンクされたPRを作成

# 2段階のタスクワークフロー：
/todo-task-planning TODO.md       # 第1段階：計画してTODO.mdを作成
/todo-task-run TODO.md            # 第2段階：TODO.mdからタスクを実行
/todo-task-run TODO.md --no-pr    # PR作成なしでタスクを実行
```

### テンプレートスキル

テンプレートスキルはリファレンス情報を提供します：

```
/key-guidelines                   # コア開発ガイドラインを表示
/todo-output-template             # TODO.mdフォーマット仕様を表示
```

## ドキュメント

TODOタスクワークフローの詳細については、以下を参照してください：

- **[TODO Task Development Guide JP](docs/todo-task-development-guide.jp.md)** - 以下を網羅した包括的ガイド：
  - 基本概念とFeasibility Markers
  - `todo-task-planning` コマンド詳細
  - `todo-task-run` コマンド詳細
  - 実践的なワークフローとベストプラクティス
  - よくある間違いとトラブルシューティング

## プロジェクト構造

```
cccp/
├── .claude-plugin/
│   └── plugin.json                    # プラグイン設定
├── skills/                            # 全スキル（エージェント、コマンド、テンプレート）
│   ├── git-operations-specialist/     # Git操作エージェントスキル
│   ├── project-manager/               # プロジェクト管理エージェントスキル
│   ├── commit/                        # コミットコマンドスキル
│   ├── micro-commit/                  # マイクロコミットコマンドスキル
│   ├── pull-request/                  # プルリクエストコマンドスキル
│   ├── todo-task-planning/            # タスク計画コマンドスキル
│   ├── todo-task-run/                 # タスク実行コマンドスキル
│   ├── key-guidelines/                # コアガイドラインテンプレートスキル
│   └── todo-output-template/          # TODOアウトプットフォーマットテンプレートスキル
├── docs/
│   └── todo-task-development-guide.md # 包括的なTODOタスクガイド
├── LICENSE                            # MITライセンス
└── README.md                          # このファイル
```

## コントリビューション

コントリビューションを歓迎します！気軽にIssueやプルリクエストを送信してください。

## ライセンス

このプロジェクトはMITライセンスの下でライセンスされています - 詳細は[LICENSE](LICENSE)ファイルを参照してください。

## 謝辞

- Git のベストプラクティスとワークフローに基づいています
- Lucas Rochaのマイクロコミット方法論を実装しています
- Claude Codeプラグインエコシステム用に設計されています
