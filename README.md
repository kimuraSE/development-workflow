# Claude Code Development Workflow

Claude Code (Anthropic CLI) を使ったモバイルアプリ開発のためのワークフローテンプレートです。

要件定義からストアリリース・保守まで、12フェーズの開発プロセスを `CLAUDE.md` と Phase プロンプトで定義し、Claude Code に一貫した品質で開発を進めさせます。

## 構成

```
.
├── CLAUDE.md              # ワークフロー全体のルール定義
├── prompts/               # 各フェーズの実行プロンプト
│   ├── phase-1.md         #   要件定義
│   ├── phase-2.md         #   詳細設計
│   ├── phase-3.md         #   ドメイン層
│   ├── phase-4.md         #   ユースケース層
│   ├── phase-5.md         #   リポジトリ(データ)層
│   ├── phase-6.md         #   画面設計
│   ├── phase-7.md         #   UIデザイン (Figma連携)
│   ├── phase-8.md         #   ViewModel
│   ├── phase-9.md         #   View実装
│   ├── phase-10.md        #   結合テスト
│   ├── phase-11.md        #   ストア申請準備
│   └── phase-12.md        #   リリース後保守
├── docs/                  # フェーズ成果物テンプレート
│   ├── requirements.md    #   要件定義書
│   ├── design.md          #   詳細設計書
│   ├── domain-design.md   #   ドメイン設計書
│   ├── db-schema.md       #   DBスキーマ定義
│   ├── screen.md          #   画面設計書
│   ├── images-design.md   #   画像アセット定義
│   └── store-submission.md #  ストア申請チェックリスト
└── tasks/                 # タスク管理
    ├── todo.md            #   進捗管理
    └── lessons.md         #   学習ログ
```

## 開発フェーズ

| # | フェーズ | 主な成果物 | 承認 |
|---|---------|-----------|------|
| 1 | 要件定義 | `requirements.md` | 要 |
| 2 | 詳細設計 | `design.md`, `domain-design.md`, `db-schema.md` | 要 |
| 3 | ドメイン層実装 | Domain layer + テスト | - |
| 4 | ユースケース層実装 | UseCase layer + テスト | - |
| 5 | リポジトリ層実装 | Data layer + テスト | - |
| 6 | 画面設計 | `screen.md`, `images-design.md` | 要 |
| 7 | UIデザイン | Figma デザイン検証 | 要 |
| 8 | ViewModel実装 | ViewModel + テスト | - |
| 9 | View実装 | 画面実装 (iOS/Android) | - |
| 10 | 結合 | E2E検証 | - |
| 11 | ストア申請準備 | `store-submission.md` | 要 |
| 12 | リリース後保守 | 変更サイクル管理 | 要 |

## 使い方

1. このリポジトリをプロジェクトのルートにクローンまたはコピーします
2. Claude Code のセッションを開始すると、`CLAUDE.md` が自動的に読み込まれます
3. `/phase 1` コマンドで Phase 1 (要件定義) から開発を開始します
4. 各フェーズの Exit Criteria を満たしてから次のフェーズへ進みます

## 主な特徴

- **フェーズゲート制御** - 各フェーズの完了条件を満たさないと次に進めない
- **厳格な障害ハンドリング** - テスト失敗・ビルドエラー時は即座に停止し、3回リトライ後にユーザーへエスカレーション
- **スコープ制御** - 要求外の変更を防止
- **ドキュメント同期** - 実装とドキュメントの乖離を障害として扱う
- **自己改善ループ** - 修正のたびに `lessons.md` へパターンを記録し再発を防止

## ライセンス

MIT
