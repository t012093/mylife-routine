# 単一リポジトリでのタスク管理ガイド

## ラベル構造

### 1. カテゴリラベル（色: #0366d6）
- `work`: 仕事関連タスク
- `routine`: 日常的なルーティン
- `dev`: 開発関連タスク

### 2. ステータスラベル（色: #d4c5f9）
- `backlog`: 未着手
- `inprogress`: 進行中
- `done`: 完了

### 3. 優先度ラベル（色: #d93f0b）
- `p1`: 最優先
- `p2`: 高優先
- `p3`: 中優先
- `p4`: 低優先

### 4. 時間枠ラベル（色: #0e8a16）
- `quick`: 15分以内
- `short`: 30分以内
- `medium`: 1-2時間
- `long`: 半日以上

### 5. 性質ラベル（色: #fbca04）
- `recurring`: 定期的なタスク
- `milestone`: 重要な節目
- `blocked`: 他の作業待ち
- `review`: レビュー必要

## プロジェクトビューの活用

### 1. マルチボードビュー
```
Status    | Backlog | In Progress | Done
Work      |   □    |     □      |  □
Routine   |   □    |     □      |  □
Dev       |   □    |     □      |  □
```

### 2. カレンダービュー
- 日付ベースでタスクを可視化
- カテゴリ別に色分け表示

### 3. テーブルビュー
カスタムフィールド:
- Priority
- Category
- Due Date
- Time Estimate
- Progress

## フィルタリング例

### 1. 仕事タスクの優先度管理
```
label:work label:p1,p2 -label:done
```

### 2. 今日のルーティン
```
label:routine label:recurring created:@today
```

### 3. 開発タスクの進捗
```
label:dev label:inprogress
```

### 4. ブロック中のタスク
```
label:blocked sort:created-desc
```

## Issueテンプレート活用

### 1. 仕事タスク用テンプレート
```yaml
name: 仕事タスク
description: 仕事関連のタスクを作成
labels: [work]
body:
  - type: dropdown
    id: priority
    attributes:
      label: 優先度
      options:
        - P1 - 最優先
        - P2 - 高優先
        - P3 - 中優先
        - P4 - 低優先
    validations:
      required: true
  
  - type: dropdown
    id: time
    attributes:
      label: 予想所要時間
      options:
        - Quick (15分以内)
        - Short (30分以内)
        - Medium (1-2時間)
        - Long (半日以上)
    validations:
      required: true

  - type: textarea
    id: description
    attributes:
      label: タスク内容
      value: |
        ## 目的
        
        ## タスク
        - [ ] 
        - [ ] 
        
        ## 期待される成果
        
        ## 関連リソース
```

### 2. ルーティン用テンプレート
```yaml
name: ルーティンタスク
description: 定期的な習慣化タスクを作成
labels: [routine, recurring]
body:
  - type: dropdown
    id: frequency
    attributes:
      label: 頻度
      options:
        - 毎日
        - 平日のみ
        - 週1回
        - 月1回
    validations:
      required: true

  - type: textarea
    id: checklist
    attributes:
      label: チェックリスト
      value: |
        ## 今日の目標
        - [ ] 
        - [ ] 
        
        ## 記録
        - 実施時間：
        - 気づき：
```

### 3. 開発タスク用テンプレート
```yaml
name: 開発タスク
description: 開発関連のタスクを作成
labels: [dev]
body:
  - type: dropdown
    id: type
    attributes:
      label: タスクタイプ
      options:
        - 機能開発
        - バグ修正
        - リファクタリング
        - ドキュメント
    validations:
      required: true

  - type: textarea
    id: detail
    attributes:
      label: 詳細
      value: |
        ## 概要
        
        ## 技術的要件
        - [ ] 
        - [ ] 
        
        ## テスト項目
        - [ ] 
        
        ## 参考資料
```

## 自動化ワークフロー

### 1. イシュー作成時の自動化
- テンプレート選択に基づく自動ラベル付与
- プロジェクトボードへの自動追加
- 優先度に基づくマイルストーン設定

### 2. 期限管理の自動化
- 期限切れ警告の自動通知
- 定期タスクの自動作成
- 完了期限のカレンダー連携

### 3. ステータス管理の自動化
- アサイン時のステータス自動更新
- クローズ時の完了ラベル自動付与
- ブロック状態の自動検知

## 運用のベストプラクティス

### 1. ラベルの効果的な使用
- 必須ラベル: カテゴリ、ステータス
- 推奨ラベル: 優先度、時間枠
- 状況に応じて: 性質ラベル

### 2. プロジェクト管理
- 週次でのバックログ整理
- 月次での完了タスク振り返り
- 四半期ごとのラベル体系見直し

### 3. 効率的な検索
- カテゴリ×優先度での検索
- 期限×ステータスでの検索
- 担当者×進捗状況での検索

### 4. メンテナンス
- 完了タスクの定期アーカイブ
- 重複ラベルの統合
- 未使用ラベルの整理

## 移行のためのチェックリスト

1. 既存リポジトリの統合準備
   - [ ] 既存イシューの棚卸し
   - [ ] 重複タスクの特定と統合
   - [ ] 必要なラベルの作成

2. プロジェクト設定
   - [ ] 新規プロジェクトボードの作成
   - [ ] ビューの設定
   - [ ] 自動化ルールの設定

3. テンプレートの準備
   - [ ] 仕事用テンプレートの作成
   - [ ] ルーティン用テンプレートの作成
   - [ ] 開発用テンプレートの作成

4. 移行作業
   - [ ] 既存イシューの移行
   - [ ] ラベルの付け直し
   - [ ] プロジェクトへの再割り当て

5. 検証
   - [ ] ラベルの機能確認
   - [ ] 自動化の動作確認
   - [ ] 検索・フィルタリングの確認
