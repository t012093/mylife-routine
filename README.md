# GitHub Projects Task Management Template

✅ このテンプレートは、GitHub ProjectsとGitHub Actionsを活用した効率的なタスク管理システムを提供します。

## 🌟 主な機能

- issueの自動ラベル付け
- プロジェクトへの自動追加
- ステータスの自動更新
- プライオリティの自動設定

## 🚀 セットアップ手順

### 1. リポジトリの準備

1. このテンプレートをベースに新しいリポジトリを作成
2. リポジトリの Settings > Actions > General で以下を有効化：
   - `Allow all actions and reusable workflows`
   - `Read and write permissions` in Workflow permissions

### 2. GitHub Projectsの設定

1. リポジトリの Projects タブから新規プロジェクトを作成
2. 以下のカスタムフィールドを追加：
   - Status（単一選択）
     - Backlog
     - In Progress
     - Done
   - Priority（単一選択）
     - Daily
     - Weekly
     - Monthly

### 3. 環境変数の設定

1. GitHub上でプロジェクトの作成
   1. リポジトリの`Projects`タブを開く
   2. `New project`をクリック
   3. `Board`テンプレートを選択
   4. プロジェクト名を入力して作成

2. カスタムフィールドの追加
   1. プロジェクトの`Settings`を開く（歯車アイコン）
   2. `Custom fields`セクションで以下を追加：
      - `Status`（単一選択）
        - `Backlog`
        - `In Progress`
        - `Done`
      - `Priority`（単一選択）
        - `Daily`
        - `Weekly`
        - `Monthly`

3. シークレット変数の設定

   1. リポジトリの`Settings` > `Secrets and variables` > `Actions`を開く
   2. `New repository secret`で以下を設定：

   | シークレット名 | 設定値の例 | 説明 |
   |-------------|------------|-----|
   | `PROJECT_ID` | `1` | プロジェクトのURL末尾の数字<br>例：`https://github.com/users/username/projects/1` → `1` |
   | `PROJECT_STATUS_FIELD` | `PVTSSF_lADOBJe6u85pcgJK6VI` | Status列のID |
   | `PROJECT_PRIORITY_FIELD` | `PVTSSF_lADOBJe6u85pcgJK6Vb` | Priority列のID |
   | `STATUS_OPTION_BACKLOG` | `47fc9ee4` | Backlogオプションの値 |
   | `STATUS_OPTION_IN_PROGRESS` | `47fc9ee5` | In Progressオプションの値 |
   | `STATUS_OPTION_DONE` | `47fc9ee6` | Doneオプションの値 |
   | `PRIORITY_OPTION_DAILY` | `47fc9ee7` | Dailyオプションの値 |
   | `PRIORITY_OPTION_WEEKLY` | `47fc9ee8` | Weeklyオプションの値 |
   | `PRIORITY_OPTION_MONTHLY` | `47fc9ee9` | Monthlyオプションの値 |

   **具体的な設定手順：**
   1. `New repository secret`ボタンをクリック
   2. Name欄に「PROJECT_ID」など、上記の シークレット名 を入力
   3. Value欄に対応する値を入力
   4. `Add secret`ボタンをクリック
   5. 他のシークレットも同様に追加

   **最小限の設定でテストする場合：**
   1. まずは`PROJECT_ID`だけを設定
   ```
   Name: PROJECT_ID
   Value: 1  # プロジェクトのURL末尾の数字
   ```
   2. 基本機能を確認後、他のIDを順次追加

4. フィールドIDとオプションIDの取得
   1. ブラウザの開発者ツールを開く（F12）
   2. `Network`タブを選択
   3. プロジェクトページでフィールドを操作（値を変更など）
   4. GraphQLリクエストを確認し、IDを特定

   または、以下のスクリプトでも取得可能：
   ```bash
   gh api graphql -f query='
   query{
     viewer {
       projectV2(number: プロジェクト番号) {
         id
         fields(first: 20) {
           nodes {
             ... on ProjectV2SingleSelectField {
               id
               name
               options {
                 id
                 name
               }
             }
           }
         }
       }
     }
   }
   '
   ```

## 🔧 カスタマイズ

### ラベルの設定

1. 以下のラベルを作成:
   - `work`: 仕事関連タスク（色: #0366d6）
   - `routine`: 日常的なルーティン（色: #d4c5f9）
   - `engineering`: 開発関連タスク（色: #fbca04）

```bash
gh label create work --color 0366d6 --description "仕事関連タスク"
gh label create routine --color d4c5f9 --description "日常的なルーティン"
gh label create engineering --color fbca04 --description "開発関連タスク"
```

### ワークフローのカスタマイズ

`.github/workflows/issue-automation.yml`の設定を必要に応じて変更できます：

- トリガーとなるイベントの追加/削除
- ラベルの種類の変更
- ステータス遷移ルールの変更

## 📝 使用方法

### 手動でのissue作成

1. issueの作成
   - テンプレートに従ってissueを作成
   - プライオリティを選択（Daily/Weekly/Monthly）

2. 自動化の確認
   - issue作成時に自動でラベルが付与
   - プロジェクトに自動で追加
   - ステータスが自動で更新

### ルーティンタスクの自動作成

定期的なissue作成を自動化する機能を提供しています：

1. ルーティン設定ファイル
`settings/routines.yml`でタスクを定義：
```yaml
daily:
  - title: "⚡ 日報作成"
    body: |
      ## 目的
      - タスクの目的
      
      ## 作業項目
      - [ ] 作業1
      - [ ] 作業2
    labels:
      - routine
      - priority:daily

weekly:
  - title: "⚡ 週次タスク"
    body: |
      ## 作業項目
      - [ ] タスク1
    labels:
      - routine
      - priority:weekly
    condition: monday  # 月曜日に実行

monthly:
  - title: "⚡ 月次タスク"
    body: |
      ## 作業項目
      - [ ] タスク1
    labels:
      - routine
      - priority:monthly
    condition: first-day  # 毎月1日に実行
```

2. 実行タイミング
   - 毎日のタスク：毎日午前9時に自動作成
   - 週次タスク：`condition: monday`指定で月曜日に作成
   - 月次タスク：`condition: first-day`指定で毎月1日に作成

3. カスタマイズ方法
   - YAMLファイルに新しいタスクを追加
   - 各タスクに対して以下を設定可能：
     - `title`: タスクのタイトル
     - `body`: タスクの詳細（Markdown形式）
     - `labels`: 付与するラベル
     - `condition`: 作成条件（monday/first-day）

4. 手動実行
   - GitHub Actionsから`Create Routine Issues`ワークフローを手動実行可能
   - 特定のカテゴリ（daily/weekly/monthly）のみを選択して実行可能

## 🤝 コントリビューション

1. このリポジトリをフォーク
2. 機能追加用のブランチを作成
3. 変更をコミット
4. プルリクエストを作成

## ⚖️ ライセンス

MITライセンスの下で公開されています。詳細は[LICENSE](LICENSE)ファイルを参照してください。
