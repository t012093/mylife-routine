# MyLife タスク管理システム

このシステムは、GitHub ProjectsとGitHub Actionsを使用して、生活のさまざまな側面のタスクを効率的に管理するためのフレームワークです。

## システム概要

> **Note**: 単一リポジトリでの管理方法については[こちらのガイド](docs/single-repo-guide.md)を参照してください。

### リポジトリ構成

現在、タスクは3つの専用リポジトリに分けて管理されています：

1. [mylife-work](https://github.com/t012093/mylife-work)
   - 仕事関連のタスク管理
   - ミーティングノート
   - プロジェクト進捗管理

2. [mylife-routine](https://github.com/t012093/mylife-routine)
   - 日常的なルーティンタスク
   - 習慣化したい活動
   - 健康・運動の記録

3. [mylife-dev](https://github.com/t012093/mylife-dev)
   - 開発プロジェクト
   - 技術検証タスク
   - インフラ整備

### 自動化機能

1. プロジェクト連携
   - Issue作成時に自動的にプロジェクトボードに追加
   - ステータスに応じて自動的に列を移動
   - プロジェクトボードとIssueの状態を常に同期

2. ラベル管理
   - `backlog`: 未着手のタスク
   - `inprogress`: 作業中のタスク
   - `done`: 完了したタスク

## セットアップ手順

1. 必要なリポジトリの作成
   ```bash
   gh repo create mylife-work --public
   gh repo create mylife-routine --public
   gh repo create mylife-dev --public
   ```

2. GitHub Personal Access Token(PAT)の生成
   - GitHub Settings > Developer settings > Personal access tokens
   - 必要な権限: `repo` スコープ

3. シークレットの設定
   - 各リポジトリのSettings > Secrets
   - `KEY`という名前でPATを登録

4. プロジェクトボードの作成
   - プロジェクト名: `MyLife Task Management`
   - 以下の列を作成:
     - `Backlog`
     - `In Progress`
     - `Done`

5. ワークフローファイルの設定
   - `.github/workflows/`ディレクトリに`issue-project-management.yml`を配置
   - プロジェクトURLを正しく設定

## 使用方法

詳細な使用方法とベストプラクティスは[Issue作成ガイド](docs/issue-guide.md)を参照してください。

### 1. タスクの作成
```bash
# 例：ルーティンタスクの作成
gh issue create --title "朝の運動" --body "## 目標
- [ ] ストレッチ
- [ ] 軽いジョギング
- [ ] 深呼吸"
```

### 2. タスクの進行管理
- Issue作成時：
  - 自動的にプロジェクトボードの`Backlog`列に追加
  - `backlog`ラベルが付与

- 作業開始時：
  - 担当者をアサイン
  - 自動的に`In Progress`列に移動
  - `inprogress`ラベルに更新

- タスク完了時：
  - Issueをクローズ
  - 自動的に`Done`列に移動
  - `done`ラベルに更新

### 3. プロジェクトボードの活用
- カンバン方式でタスクを視覚的に管理
- ドラッグ＆ドロップで状態を更新
- フィルターやソートで必要なタスクを素早く確認

## ベストプラクティス

1. タスクの分類
   - 仕事関連 → mylife-work
   - 日常ルーティン → mylife-routine
   - 開発タスク → mylife-dev

2. Issue作成時のポイント
   - 具体的なタイトル
   - チェックリスト形式での目標設定
   - 期限や目標時間の明記

3. プロジェクト管理
   - 定期的なバックログの整理
   - 完了したタスクの振り返り
   - ラベルを活用した優先度付け

## カスタマイズ

必要に応じて以下の要素をカスタマイズできます：

1. ラベル
   - 優先度（high, medium, low）
   - カテゴリー（meeting, document, coding）
   - ステータス（waiting, blocked, review）

2. プロジェクトビュー
   - カンバンボード
   - ガントチャート
   - カレンダービュー

3. 自動化ルール
   - ステータス変更条件
   - ラベル付与ルール
   - 通知設定
